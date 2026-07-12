# Guía de Defensa — CorpoDG Trip593

---

## 1. Checklist de Subida al Repositorio del Tribunal

### 📦 Código Fuente y Bibliotecas

| Item | Estado | Acción |
|------|--------|--------|
| Backend Django (7,900+ líneas) | ✅ Listo | `Proyecto_CorpoDG_Backend/` |
| Frontend Vue 3 (17,800+ líneas) | ✅ Listo | `Proyecto_CorpoDG_Frontend/` |
| `requirements.txt` | ✅ Listo | Dependencias Python (Django, DRF, Groq, Stripe, etc.) |
| `package.json` | ✅ Listo | Dependencias JS (Vue 3, Vite, Playwright, etc.) |

### 🧪 Pruebas y Resultados

| Item | Estado | Acción |
|------|--------|--------|
| 78 tests unitarios backend | ✅ Listo | `servicios/tests.py` |
| 19 tests E2E frontend | ✅ Listo | `tests/e2e/` (chatbot, responsive, estados) |
| Pipeline k6 performance | ✅ Listo | `tests/performance/scenarios.js` |
| Reporte HTML Playwright | ✅ Listo | `playwright-report/` (artifact en CI) |
| Reporte tests backend | ✅ Listo | Artifact en CI (`backend-test-report`) |
| Capturas responsive (9) | ✅ Listo | `reportes/capturas/frontend/` |
| Reportes .md individuales | ✅ Listo | `reportes/REPORTE_*.md` (BACKEND, FRONTEND, SISTEMA, PERFORMANCE) |
| Evidencia consolidada | ✅ Listo | `EVIDENCIA.md`, `RESULTADOS_TEST.md` |
| JSON resultados Sabre | ✅ Listo | `cp01_resultados.json`, `cp05_resultados.json` |

### 📐 Diagramas y Documentación

| Item | Estado | Acción |
|------|--------|--------|
| **Arquitectura de la solución** | ✅ Listo | `reportes/SECCIONES_TESIS.md` §1 |
| **Metodología aplicada** | ✅ Listo | `reportes/SECCIONES_TESIS.md` §2 |
| **Recursos técnicos** | ✅ Listo | `reportes/SECCIONES_TESIS.md` §3 |
| **Diagrama de capas (texto)** | ✅ Listo | `reportes/SECCIONES_TESIS.md` §1.1 |
| **Diagrama de infraestructura** | ⚠️ Sugerido | Generar diagrama draw.io o similar (ver abajo) |
| API Documentation | ✅ Listo | `API_DOCUMENTATION.md` (en cada sub-repo) |
| README | ✅ Listo | `README.md` (en cada sub-repo) |

### 📄 Documento Final y Póster

| Item | Estado | Acción |
|------|--------|--------|
| Documento .docx de tesis | ✅ Completado (según usuario) | Insertar capturas de `reportes/capturas/` y referencias a los reportes |
| Póster científico | ❌ No encontrado | Falta crear. Recomendación: canva.com tiene plantillas para proyectos de titulación |

### 🔧 CI/CD y Despliegue

| Item | Estado | Acción |
|------|--------|--------|
| Frontend en Render | ✅ `proyecto-corpodg-frontend.onrender.com` | Incluir URL en documento |
| Backend en Render | ✅ `corpodg-backend-staging.onrender.com` | Incluir URL en documento |
| Frontend CI pipeline | ✅ Pasando | 19 tests, Chromium + Firefox |
| Backend CI pipeline | ✅ Pasando | 78 tests, PostgreSQL 17 |
| Performance pipeline | ✅ Pasando | k6 con thresholds RNF-02 |

### ⚠️ Cosas a Verificar Antes de Subir

1. **Reemplazar tokens reales** en `.env` del backend — asegurar que solo hay placeholders en el código (`TU_TOKEN_AQUI`)
2. **Verificar `.gitignore`** incluya: `test-results/`, `playwright-report/`, `__pycache__/`, `.ruff_cache/`, `*.sqlite3`
3. **Seed data** — confirmar que `python manage.py seed_data` funciona en limpio
4. **GROQ_API_KEY** en Render — ya verificaste que el chatbot responde ✅

---

## 2. Guía de Presentación Oral (10-15 minutos)

### 🎯 Punto 1: Definición del Problema y Propuesta de Solución

**Problema** (30-45s):
> "Las agencias de viaje tradicionales en Ecuador enfrentan limitaciones para ofrecer cotizaciones en tiempo real. Los procesos manuales retrasan la atención al cliente y dificultan la comparación de opciones de vuelos, paquetes y destinos desde un solo lugar."

**Propuesta** (30-45s):
> "CorpoDG Trip593 es un sistema web responsive B2C que integra:
> - **Catálogo dinámico** de destinos y paquetes turísticos administrable vía panel Django
> - **Búsqueda en tiempo real** de vuelos conectada al GDS Sabre
> - **Asistente virtual** (Cory) basado en inteligencia artificial con restricción de dominio
> - **Diseño responsive** para acceso desde cualquier dispositivo"

**Tecnologías clave** (30s):
> "Construido con Vue 3 en el frontend, Django REST Framework en el backend, PostgreSQL como base de datos, y desplegado en Render con integración continua mediante GitHub Actions."

---

### 🎯 Punto 2: Arquitectura, Metodología y Recursos

**Arquitectura** (2-3 min):

| Capa | Tecnología | Rol |
|------|-----------|-----|
| Presentación | Vue 3 + Vite + Vue Router | SPA con 14 vistas y 11 componentes |
| API REST | Django 4.2 + DRF 3.14 | 28 endpoints, autenticación vía sesión |
| Datos | PostgreSQL 16 en Render | 18 modelos de datos |
| GDS | Sabre (sandbox) | Búsqueda de vuelos, revalidación, seatmap |
| LLM | Groq + LLaMA 3.3-70B | Chatbot Cory con function calling |
| CI/CD | GitHub Actions | 3 pipelines: Backend CI, Frontend CI, Performance |

**Explicar brevemente el flujo**:
> "El usuario interactúa con la SPA de Vue 3. Las peticiones viajan por HTTP/JSON al backend Django. Cuando busca vuelos, el backend orquesta la llamada a Sabre. Cuando conversa con Cory, el backend usa Groq. Los datos de catálogo se sirven directamente desde PostgreSQL."

**Metodología** (1-2 min):
> "Se aplicó Scrum adaptado a equipo unipersonal, con 5 sprints de 1-2 semanas cada uno: configuración inicial, catálogo, integración Sabre, chatbot y experiencia de usuario, y finalmente pruebas y cierre. Se gestionó el backlog mediante GitHub Issues con labels (feature, testing, infra) y el progreso se siguió con GitHub Projects."

**Recursos técnicos** (1 min):
> "Se utilizó Python 3.13 + Django para el backend, JavaScript ES2022 + Vue 3 para el frontend, PostgreSQL 16 como base de datos, y servicios cloud como Render (despliegue), Sabre (GDS), Groq (LLM) y GitHub (CV + CI/CD). Las pruebas incluyeron 78 tests unitarios con Django TestCase, 19 tests E2E con Playwright, y pruebas de performance con k6 y Locust."

---

### 🎯 Punto 3: Demostración Funcional y Defensa Técnica

**Preparar la demostración en vivo** (5-7 min):

| # | Funcionalidad | Qué mostrar | Posibles preguntas |
|:-:|---------------|-------------|-------------------|
| 1 | **Homepage responsive** | Abrir en desktop y redimensionar a mobile | ¿Cómo se manejan los breakpoints? |
| 2 | **Catálogo destinos** | Navegar destinos, mostrar PDF de Google Drive | ¿Cómo se validan las URLs? |
| 3 | **Catálogo paquetes** | Filtrar por región/tipo/temporada | ¿Qué criterios de ordenamiento usan? |
| 4 | **Búsqueda de vuelos** | UIO → GYE, mostrar resultados ordenados | ¿Cómo manejan el timeout de Sabre? |
| 5 | **Chatbot Cory** | "¿Qué destinos tienen?" "Quiero viajar a Galápagos" | ¿Cómo se restringe el dominio? |
| 6 | **Admin Django** | Mostrar panel jazzmin con datos | ¿Qué modelos se administran? |
| 7 | **CI/CD** | Mostrar pipelines verdes en GitHub Actions | ¿Qué pasa si un test falla? |

**Puntos técnicos para reforzar durante la demo:**

- **Retry 401**: "Las llamadas a Sabre tienen retry automático. Si el token expira, se refresca y reintenta automáticamente antes de devolver error."
- **Restricción de dominio**: "El system prompt de Cory limita explícitamente las respuestas al dominio de turismo de CorpoDG, usando function calling para consultar la base de datos."
- **Seguridad de credenciales**: "Todas las claves API se manejan mediante variables de entorno con python-decouple. El código fuente contiene solo placeholders."

---

## 3. Posibles Preguntas del Tribunal y Cómo Responder

### 🔴 Preguntas Técnicas Obligadas

**1. ¿Cómo aseguran que el chatbot no responda temas fuera del dominio?**

> "El chatbot usa un system prompt que define explícitamente su alcance: solo turismo y viajes de CorpoDG. Implementamos function calling con Groq: el modelo no tiene acceso a internet ni a conocimiento general, solo a las tools que definimos (consulta de paquetes, destinos, vuelos). Si la pregunta no corresponde al dominio, Cory responde amablemente que solo puede ayudar con temas de viajes. Esto está cubierto por 12 tests unitarios."

**2. ¿Qué pasa si Sabre no responde o el token expira?**

> "Implementamos un mecanismo de retry con backoff. Si Sabre devuelve 401 (token expirado), se refresca el token automáticamente y se reintenta la operación. Si falla dos veces consecutivas, se devuelve un error controlado al usuario. Esto está cubierto por 2 tests específicos (CP-07)."

**3. ¿Cómo manejan la seguridad de las credenciales?**

> "Todas las credenciales (Sabre, Groq, Stripe, WhatsApp, base de datos) se configuran mediante variables de entorno usando python-decouple. En el repositorio solo existen placeholders. En producción, se configuran desde el panel de Render."

**4. ¿Cómo demostraron que el sistema soporta carga concurrente?**

> "Realizamos dos tipos de pruebas: con Locust simulamos 100 usuarios concurrentes durante 60 segundos, obteniendo 1,795 requests con 0% de fallos. Además, implementamos un pipeline automatizado con k6 en GitHub Actions que se ejecuta en cada push y verifica thresholds de performance (p95)."

**5. ¿Por qué usaron AllowAny en lugar de autenticación?**

> "El sistema es B2C (Business to Consumer), es decir, los usuarios finales acceden al catálogo sin necesidad de registro. La autenticación con token fue eliminada intencionalmente porque no se requiere login para navegar destinos, paquetes ni buscar vuelos. El panel administrativo sí está protegido por autenticación de Django, accesible solo para administradores."

---

### 🟡 Preguntas Técnicas Probables

**6. ¿Cómo está estructurada la base de datos?**

> "18 modelos Django que cubren: clientes y solicitudes, destinos turísticos con validación de URLs de Google Drive, vuelos con relaciones a aerolíneas y aeropuertos, regiones/paises/ciudades en cascada, paquetes turísticos con tipos y temporadas, y una tabla de configuración de destacados con orden personalizado."

**7. ¿Qué patrones de diseño usaron?**

> "Varios: REST API para la comunicación frontend-backend, Repository Pattern en los módulos de servicio que encapsulan la lógica de integración con Sabre, Strategy Pattern en el mapa de asientos que alterna entre sandbox y Sabre real según configuración, y Circuit Breaker en el retry automático ante errores 401."

**8. ¿Cómo garantizan que el diseño sea responsive?**

> "Usamos CSS vanilla con media queries para 3 breakpoints: mobile (320px), tablet (768px) y desktop (1280px+). Automatizamos la verificación con 9 tests E2E en Playwright que capturan screenshots en cada viewport para homepage, destinos y paquetes, tanto en Chromium como en Firefox."

**9. ¿Qué cobertura de pruebas tienen?**

> "Backend: 78 tests unitarios (modelos, servicios Sabre, chatbot, viewsets) que se ejecutan en PostgreSQL 17 real en CI. Frontend: 19 tests E2E con Playwright (5 de chatbot, 9 responsive, 5 de estados carga/error). Performance: pipeline automatizado con k6 que verifica thresholds de respuesta."

**10. ¿El sistema está desplegado y accesible?**

> "Sí, el frontend está en https://proyecto-corpodg-frontend.onrender.com y el backend en https://corpodg-backend-staging.onrender.com. El admin está en /admin/ con usuario admin. Todo se despliega automáticamente desde la rama staging mediante integración con Render."

---

### 🟢 Preguntas Administrativas

**11. ¿Qué metodología usaron y cómo la adaptaron?**

> "Scrum, adaptado a equipo unipersonal. Trabajamos en 5 sprints con objetivos claros. Usamos GitHub Issues como product backlog y GitHub Projects como tablero. Las daily scrums eran auto-gestionadas. Cada sprint terminaba con un incremento funcional desplegado en staging."

**12. ¿Qué excluyeron del alcance y por qué?**

> "La reserva y pago en línea de vuelos (RF-09) quedó fuera porque el alcance del proyecto se definió hasta la cotización y contacto. El sistema permite buscar vuelos, ver detalles y contactar a la agencia, pero la transacción final se maneja por los canales tradicionales de CorpoDG."

**13. ¿Qué harían diferente si continuaran el proyecto?**

> "Agregaríamos autenticación de usuarios con perfiles de viajero frecuente, notificaciones push para ofertas, y un módulo de reservas completo con Stripe en producción. También migraríamos a Azure OpenAI para el chatbot y añadiríamos un cache con Redis para mejorar la performance de búsquedas repetitivas."

---

## 4. Demo: Script Paso a Paso

### Preparación
- [ ] Abrir frontend: https://proyecto-corpodg-frontend.onrender.com
- [ ] Abrir backend admin: https://corpodg-backend-staging.onrender.com/admin/
- [ ] Abrir GitHub Actions con pipelines verdes
- [ ] Abrir `reportes/SECCIONES_TESIS.md` para referencia rápida

### Guión de Demo (5-7 min)

**1. Homepage** (30s)
> "Esta es la página principal. Muestra los destinos destacados y paquetes disponibles. Si redimensiono, el layout se adapta — probamos en 320px, 768px y 1280px automáticamente."

**2. Destinos** (30s)
> "Aquí están los destinos de Ecuador: Galápagos, Baños, Quito... Cada destino tiene su PDF informativo que se visualiza en un modal. Los datos vienen del backend."

**3. Paquetes** (30s)
> "Los paquetes se pueden filtrar por región, tipo y temporada. La información incluye precios, duración y aerolíneas participantes."

**4. Búsqueda de vuelos** (1 min)
> "En la sección de vuelos, selecciono origen UIO, destino GYE. El sistema consulta Sabre en tiempo real y muestra resultados ordenados por escalas, duración y precio. El más barato: $104.93 con LATAM."

**5. Chatbot Cory** (1 min)
> "Abro el chat. Pregunto '¿Qué destinos hay?'. Cory consulta la base de datos y responde. Pregunto '¿Quiero volar a Galápagos?' y me da información del destino. Si pregunto algo fuera de tema, Cory redirige amablemente."

**6. Admin Django** (30s)
> "El panel de administración con jazzmin permite gestionar todo el catálogo: destinos, paquetes, vuelos, regiones. Los administradores pueden crear contenido sin conocimientos técnicos."

**7. CI/CD** (30s)
> "Cada cambio en la rama staging dispara 3 pipelines: Backend CI (78 tests, ~57s), Frontend CI (19 tests, ~73s), y Performance Tests (k6). Los 3 están pasando. Esto garantiza que cualquier cambio no rompa funcionalidad existente."
