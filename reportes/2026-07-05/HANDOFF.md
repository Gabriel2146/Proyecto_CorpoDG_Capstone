# Handoff — CorpoDG Trip593 (Sistema Web Responsive B2C)

Documento de traspaso para completar los requerimientos funcionales (RF) y no
funcionales (RNF) del proyecto, y ejecutar la planificación restante.

**Exclusión explícita de alcance (ver Limitaciones del proyecto):**
- **RF-09** (Reserva y pago en línea de vuelos) — queda fuera de este handoff.
- **RNF-02** (Seguridad / integridad de las transacciones de pago) — queda fuera de este handoff.

> Nota de dependencia: RF-10 (reserva y pago de paquetes turísticos) usa el
> mismo mecanismo de checkout/webhook de Stripe que RF-09/RNF-02. Aquí se
> mantiene RF-10 en el alcance porque no fue nombrado en la exclusión, pero
> si el equipo decide retirar todo el módulo de pagos, RF-10 debe salir
> también y `bookingPaquete.py` / `PaqueteCheckoutView` / `PaqueteConfirmView`
> quedarían sin uso.

---

## 1. Resumen ejecutivo

| Área | Estado |
|---|---|
| Backend (Django/DRF) | Funcional, ~7 900 líneas en [servicios/](Proyecto_CorpoDG_Backend/servicios) |
| Frontend (Vue 3/Vite) | Funcional, ~17 800 líneas en [src/](Proyecto_CorpoDG_Frontend/src) |
| CI/CD | Configurado en ambos repos (`.github/workflows/ci.yml`) |
| Control de versiones | **Sin commits todavía** — la rama actual (`feat/gabriel/chatbot_v2`) está vacía. Esto es lo primero que debe resolverse (ver §6). |
| Pruebas automatizadas | 10 tests unitarios (`servicios/tests.py`, solo chatbot) + 1 spec e2e (`tests/e2e/chatbot.spec.js`) |

---

## 2. Requisitos funcionales — pendientes/for verificar (excluye RF-09)

### RF-01 — Búsqueda de vuelos en tiempo real
- **Código:** [BuscadorVuelosSabreView](Proyecto_CorpoDG_Backend/servicios/views.py:484), [searchFlights.py](Proyecto_CorpoDG_Backend/servicios/searchFlights.py)
- **Pendiente para cumplir 100%:**
  - Prueba de sistema documentada (CP-01) con capturas reales del entorno sandbox Sabre.
  - Verificar manejo de reintento automático ante error 401 (cubierto en código, falta test automatizado específico — hoy solo hay tests del chatbot).
  - Confirmar ordenamiento (escalas → duración → precio) con un caso de prueba de regresión.

### RF-02 — Navegación de paquetes, destinos, vuelos
- **Código:** `DestinoViewSet`, `PaqueteTuristicoViewSet` (views.py), [Destinos.vue](Proyecto_CorpoDG_Frontend/src/views/Destinos.vue), [Paquetes.vue](Proyecto_CorpoDG_Frontend/src/views/Paquetes.vue), [ModalPdfViewer.vue](Proyecto_CorpoDG_Frontend/src/components/ModalPdfViewer.vue)
- **Pendiente:**
  - Prueba de aceptación de filtros por región/país/ciudad y destacados.
  - Validar que solo se listan destinos `activo=True` (regla de negocio) con un test unitario del serializer.

### RF-03 — Sistema de administración
- **Código:** [admin.py](Proyecto_CorpoDG_Backend/servicios/admin.py) (727 líneas), endpoints AJAX en cascada (región→país→ciudad→aeropuerto).
- **Pendiente:**
  - Documentar el esquema de datos del admin (mencionado como condición del RF) en un README corto.
  - Prueba manual de creación de un país/ciudad/aeropuerto con validación de duplicados (unique_together).

### RF-04 — Interfaz adaptable y navegación
- **Código:** [Navbar.vue](Proyecto_CorpoDG_Frontend/src/components/Navbar.vue), [Footer_Info.vue](Proyecto_CorpoDG_Frontend/src/components/Footer_Info.vue), estilos en `src/styles/`.
- **Pendiente:**
  - Prueba responsive real en 320px, 768px y 1280px (mobile/tablet/desktop) — no hay evidencia guardada.
  - Confirmar visibilidad permanente del número de WhatsApp (regla de negocio) en todas las vistas, no solo Home.

### RF-05 — Restricción de dominio del chatbot
- **Código:** [chatbot.py](Proyecto_CorpoDG_Backend/servicios/chatbot.py) (SYSTEM_PROMPT + `_build_accion`)
- **Estado:** Cubierto por tests unitarios existentes (`tests.py`). **Sin pendientes críticos**, solo ampliar casos límite (consultas ambiguas, cambio de idioma).

### RF-06 — Configuración segura de conexión al proveedor LLM
- **Código:** `GROQ_API_KEY` vía `python-decouple` en [settings.py:201](Proyecto_CorpoDG_Backend/corpodg/settings.py)
- **Pendiente:**
  - Agregar manejo explícito de error si `GROQ_API_KEY` falta al arrancar (hoy usa `default=''`, lo que puede fallar silenciosamente en runtime en vez de al boot).

### RF-07 — Endpoint único de conversación del chatbot
- **Código:** `ChatbotView` (views.py:876), ruta `chatbot/` en [urls.py](Proyecto_CorpoDG_Backend/servicios/urls.py)
- **Pendiente:** Ampliar la suite e2e de Playwright más allá del flujo feliz (mensajes vacíos, rate limiting).

### RF-08 — Revalidación de tarifa y mapa de asientos
- **Código:** `RevalidarVueloView`, `SeatMapView` (views.py:511, 554), [revalidateFlight.py](Proyecto_CorpoDG_Backend/servicios/revalidateFlight.py), [seatMapFlight.py](Proyecto_CorpoDG_Backend/servicios/seatMapFlight.py)
- **Pendiente:**
  - Prueba de sistema (CP-05) con captura real del mapa de asientos en sandbox.
  - Test unitario del caso "tarifa cambió → usuario debe confirmar antes de pagar".

### RF-10 — Reserva y pago en línea de paquetes turísticos
- **Código:** `PaqueteCheckoutView`, `PaqueteConfirmView`, `PaqueteVoucherView` (views.py:730-774+), [bookingPaquete.py](Proyecto_CorpoDG_Backend/servicios/bookingPaquete.py), [paqueteDocs.py](Proyecto_CorpoDG_Backend/servicios/paqueteDocs.py)
- **Pendiente:**
  - **Decisión de alcance requerida:** confirmar con el equipo si este RF se mantiene (comparte infraestructura de pago con RF-09/RNF-02 excluidos). Si se mantiene, necesita su propia verificación de integridad de webhook (ver riesgo en §4).
  - Prueba de sistema con tarjeta de prueba de Stripe y verificación de voucher PDF.

---

## 3. Requisitos no funcionales — pendientes/por verificar (excluye RNF-02)

### RNF-01 — Seguridad (confidencialidad de credenciales)
- **Estado:** Cumplido — todas las claves (Sabre, Groq, Stripe, SMTP, DB) están en `.env` vía `python-decouple`, no hardcodeadas.
- **Pendiente:** Auditoría puntual del repositorio antes de hacer público el primer commit, para confirmar que `.env` está en `.gitignore` y nunca se subió.

### RNF-03 — Eficiencia de desempeño
- **Pendiente:** Medir tiempos reales de búsqueda de vuelos y carga de catálogo (percentil 95); hoy no hay métricas registradas, solo el objetivo (≤30s búsqueda, ≤3s catálogo).

### RNF-04 — Fiabilidad (tolerancia a fallos)
- **Código:** Caché de token thread-safe en el módulo Sabre (`LlamadosAPIS/`), reintento automático ante 401.
- **Pendiente:** Test de integración que fuerce expiración del token y confirme el reintento (CP-07 mencionado en el documento, aún no implementado como test automatizado).

### RNF-05 — Usabilidad
- **Pendiente:** Verificar los 3 estados visuales (carga/error/sin resultados) en **todas** las vistas de búsqueda, no solo vuelos.

### RNF-06 — Mantenibilidad
- **Estado:** Buena base (módulos desacoplados, CI en cada push).
- **Pendiente:** Elevar cobertura de tests automatizados más allá del chatbot — hoy la suite del backend son 10 tests, todos sobre `chatbot.py`. Bloque crítico: **sin ningún test de vuelos, paquetes, reservas ni admin.**

### RNF-07 — Compatibilidad (portabilidad del cliente web)
- **Pendiente:** Corrida manual en Chrome/Firefox/Edge en escritorio, tablet y móvil; no hay evidencia registrada.

---

## 4. Riesgos y gaps críticos (no ligados a RF-09/RNF-02)

1. **Repositorio sin commits.** Es el bloqueante más urgente: sin historial no hay evidencia de metodología ni de Sprint-by-sprint. Ver plan en §6.
2. **Cobertura de pruebas concentrada en el chatbot.** El 100% de los tests unitarios actuales cubren `chatbot.py`; búsqueda de vuelos, paquetes, admin y reservas no tienen ningún test automatizado.
3. **API pública en `AllowAny`.** Todos los ViewSets usan `permission_classes = [AllowAny]` (`views.py`); no hay autenticación de usuario final. Esto es aceptable para el alcance B2C de solo lectura, pero debe quedar explícito en la documentación (ya corregido en el RNF, no prometer "100% autenticado").
4. **RF-10 depende de la misma pieza de pago que RF-09/RNF-02** (Stripe checkout + webhook). Si se retira RF-09/RNF-02 del alcance sin decisión sobre RF-10, queda código de pago sin verificación de integridad documentada. Se recomienda: (a) mantener el checkout de paquetes pero simplificarlo a "solicitud de reserva sin pago en línea" (equivalente al formulario de contacto, ya existente), o (b) sacar RF-10 también y usar el flujo de contacto para paquetes.

---

## 5. Planificación completa (Scrum + fases)

**Marco de trabajo:** Scrum + prácticas DevOps. Sprints de 2 semanas, 12 sprints en 6 meses.

**Roles:**
- Product Owner: representante de CORPODG – Trip593
- Scrum Master: Milton Andrés Dávila Torres
- Development Team: Marcelo Gabriel Padilla Rodríguez, Milton Andrés Dávila Torres

**Ceremonias:** Sprint Planning (quincenal), Daily Scrum (15 min), Sprint Review, Sprint Retrospective.

| Fase | Sprints | Duración | Actividades principales | Estado |
|---|---|---|---|---|
| 1. Análisis y levantamiento de requerimientos | 1–2 | 4 semanas | Entrevistas con agentes/administradores; BPMN de procesos actuales; RF/RNF; historias de usuario con criterios de aceptación; Product Backlog priorizado; arquitectura de alto nivel | Completado (ver documento Capstone) |
| 2. Diseño y prototipado | 3–4 | 4 semanas | Diagramas C4 (niveles 2–3); modelo entidad-relación; prototipos de alta fidelidad; validación con usuarios; contratos de API REST; estándares de codificación | Completado |
| 3. Desarrollo del sistema | 5–9 | 10 semanas | Sprint 5: autenticación/usuarios · Sprint 6: búsqueda y catálogo de paquetes · Sprint 7: cotización · Sprint 8: integración GDS/aerolíneas · Sprint 9: pruebas unitarias/integración + pipelines CI/CD | Código construido; **evidencia de sprint-by-sprint pendiente de commits** |
| 4. Pruebas y validación | 10–11 | 4 semanas | Pruebas funcionales exhaustivas; usabilidad con usuarios reales; rendimiento (100 usuarios concurrentes); seguridad; corrección de errores críticos; validación de RNF; documentación de usuario final | **Pendiente en su mayoría** — ver §2/§3 |
| 5. Despliegue y capacitación | 12 | 2 semanas | Entorno de producción en Render; migración de catálogos; despliegue; capacitación al personal; monitoreo post-despliegue; entrega de documentación técnica | Pendiente |

### Backlog inmediato sugerido (próximo sprint)
1. Inicializar Git: primer commit con el estado actual del código (backend + frontend), luego historia normal por rama de funcionalidad.
2. Escribir tests unitarios para `searchFlights.py`, `revalidateFlight.py`, `seatMapFlight.py`, `models.py` (validadores) y los ViewSets de catálogo.
3. Ejecutar y documentar la prueba de carga de 100 usuarios concurrentes contra `buscar-vuelos-live/`.
4. Prueba responsive formal (320/768/1280px) con capturas.
5. Decisión de producto sobre RF-10 (ver riesgo #4).
6. Preparar entorno de staging en Render y ensayar el pipeline de despliegue (Fase 5).

---

## 6. Configuración de entorno necesaria

Variables de entorno (`.env`) requeridas para correr localmente y en CI (ver [settings.py](Proyecto_CorpoDG_Backend/corpodg/settings.py)):

```
SECRET_KEY=
DEBUG=
ALLOWED_HOSTS=
DB_ENGINE=          # django.db.backends.sqlite3 en local, postgresql en CI/prod
DB_NAME= / DB_USER= / DB_PASSWORD= / DB_HOST= / DB_PORT=
CORS_ALLOWED_ORIGINS=
EMAIL_HOST_USER= / EMAIL_HOST_PASSWORD=
GROQ_API_KEY=
SABRE_AUTH_URL=  (default sandbox ya provisto)
STRIPE_SECRET_KEY= / STRIPE_PUBLISHABLE_KEY= / STRIPE_WEBHOOK_SECRET=   # solo si RF-10 se mantiene
SEATMAP_SANDBOX=True
BOOKING_SANDBOX=True
```

Comandos de verificación local:
```bash
cd Proyecto_CorpoDG_Backend && ../.venv/Scripts/python.exe manage.py test servicios
cd Proyecto_CorpoDG_Frontend && npm run build && npx playwright test
```
