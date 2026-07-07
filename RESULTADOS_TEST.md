# Informe de Resultados — CorpoDG Trip593

## 1. Resumen Ejecutivo

| Componente | Pruebas | Estado |
|-----------|---------|--------|
| Backend (Django/DRF) | 78 tests unitarios | ✅ 78/78 PASS (0.083s) |
| Frontend (Vue 3/Vite) | 14 tests E2E (Playwright) | ✅ 14/14 PASS (32.3s) |
| Prueba de Carga | 100 usuarios concurrentes (Locust) | ✅ 0% fallos (1,795 reqs) |
| Sistema Sabre (CP-01) | Búsqueda de vuelos en tiempo real | ✅ PASS — 20 vuelos UIO→GYE |
| Sistema Sabre (CP-05) | Revalidación de tarifa + mapa de asientos | ✅ PASS — $104.93 USD |
| Sistema Sabre (CP-07) | Reintento automático ante 401 | ✅ PASS — 2 tests unitarios |

---

## 2. Backend — Tests Unitarios (78 tests)

**Comando:** `python manage.py test servicios --verbosity=2`
**Duración:** 0.083s
**Resultado:** ✅ 78/78 PASS

| Módulo | Tests | Estado |
|--------|-------|--------|
| Modelos — validadores (Destino, Paquete, GoogleDrive, OpenStreetMap) | 18 | ✅ |
| searchFlights — IDs fuente, duración, ordenamiento, retry 401 | 11 | ✅ |
| revalidateFlight — payload, segmentos, normalización | 7 | ✅ |
| seatMapFlight — payload, segmentos, sandbox | 7 | ✅ |
| ViewSets — filtros Destino/Paquete/Vuelo | 9 | ✅ |
| Chatbot — restricción dominio, tools, redirect | ~12 | ✅ |
| Paquete — vencimiento, vigencia | 5 | ✅ |
| ConfigDestacados | 2 | ✅ |
| Sandbox — activo/inactivo | 3 | ✅ |
| BuildAccion — redirect vuelos/paquetes | 6 | ✅ |

### Captura de resultados
```
Ran 78 tests in 0.083s
OK
```
Los 78 tests cubren: modelos, búsqueda Sabre, revalidación, mapa de asientos, reintento 401, filtros de catálogo, chatbot, vencimiento de paquetes y urls de Google Drive.

---

## 3. Frontend — Tests E2E (Playwright)

**Comando:** `npx playwright test --reporter=html`
**Servidor:** Vite dev server en `http://localhost:5173`
**Duración:** 32.3s
**Resultado:** ✅ 14/14 PASS

### 3.1 Chatbot Cory (5 tests)
| # | Test | Resultado |
|---|------|-----------|
| 1 | Burbuja del chat visible en homepage | ✅ |
| 2 | Abre y cierra el chat al hacer clic en burbuja | ✅ |
| 3 | Muestra mensaje de bienvenida de Cory al abrir | ✅ |
| 4 | Botón de redirect aparece cuando chatbot devuelve acción | ✅ |
| 5 | Botón de redirect navega a `/vuelos/resultados` con query params | ✅ |

### 3.2 Diseño Responsive (9 tests)
Screenshots automáticos en 3 viewports × 3 páginas:

| Página | 320px (Mobile) | 768px (Tablet) | 1280px (Desktop) |
|--------|:---:|:---:|:---:|
| Homepage | ✅ | ✅ | ✅ |
| Destinos | ✅ | ✅ | ✅ |
| Paquetes | ✅ | ✅ | ✅ |

Las capturas se encuentran en la carpeta `evidencia_screenshots/`.

### Reporte HTML
El reporte interactivo de Playwright está disponible en:
`Proyecto_CorpoDG_Frontend/playwright-report/index.html`

---

## 4. Prueba de Carga — 100 Usuarios Concurrentes

**Herramienta:** Locust 2.44.4
**Duración:** 60s
**Usuarios:** 100 (rampa 10 usuarios/s)
**Endpoints probados:** destinos, destacados, paquetes, vuelos

| Endpoint | Requests | Fallos | Avg (ms) | p50 | p95 | p99 |
|----------|:--------:|:------:|:--------:|:---:|:---:|:---:|
| GET /api/destinos/ | 741 | 0 | 669 | 550 | 2,200 | 2,700 |
| GET /api/destinos/?destacado | 164 | 0 | 535 | 440 | 2,100 | 2,600 |
| GET /api/paquetes/ | 528 | 0 | 763 | 720 | 2,100 | 3,000 |
| GET /api/vuelos/ | 362 | 0 | 4,752 | 5,100 | 6,600 | 7,600 |
| **Total** | **1,795** | **0 (0%)** | **1,508** | 660 | 5,500 | 7,600 |

**Conclusión:** El sistema soporta 100 usuarios concurrentes sin fallos. Catálogo responde en <1s (p50). Búsqueda de vuelos necesita optimización de índices (promedio 4.7s).

---

## 5. Pruebas de Sistema — Integración Sabre (Sandbox)

### CP-01: Búsqueda de vuelos en tiempo real ✅

| Campo | Resultado |
|-------|-----------|
| Ruta | UIO → GYE |
| Fecha | 2026-08-15 |
| Pasajeros | 1 adulto |
| Vuelos encontrados | 20 |
| Ordenamiento | Correcto (escalas → duración → precio) |
| Precio más bajo | $104.93 USD (LATAM, directo, 53 min) |

Evidencia: `cp01_resultados.json`

### CP-05a: Revalidación de tarifa ✅

| Campo | Resultado |
|-------|-----------|
| Disponible | ✅ True |
| Precio confirmado | $104.93 USD |
| Aerolínea validadora | LA (LATAM) |
| Última fecha de compra | 2026-07-07 |

### CP-05b: Mapa de asientos ✅

| Campo | Resultado |
|-------|-----------|
| Vuelo | LA1419 UIO→GYE |
| Cabina | Economy (Y) |
| Asientos | 120 en 20 filas |
| Modo | Sandbox (simulado) |

Evidencia: `cp05_resultados.json`

### CP-07: Reintento automático ante 401 ✅
Cubierto por 2 tests unitarios específicos:
- `test_buscar_vuelos_retry_401` — verifica que un 401 force-refresca el token y reintenta
- `test_buscar_vuelos_doble_401` — verifica que dos 401 consecutivos retornan error

---

## 6. CI/CD — Pipelines Automatizados

### Backend CI ✅ — 57s
1. Set up Python 3.13
2. Instalar dependencias
3. Django system check
4. Migraciones (PostgreSQL 17)
5. **78 tests unitarios**

### Frontend CI ✅ — 1m13s
1. Set up Node 20
2. `npm ci` + build
3. Playwright E2E tests

### Reporte como Artifact
El pipeline de frontend ahora genera y sube el reporte HTML de Playwright como artifact descargable (ver `playwright-report/`).

---

## 7. Capturas del Sistema

Las siguientes capturas fueron tomadas del sistema funcionando localmente con datos reales:

### Homepage
- `evidencia_screenshots/homepage-desktop-1280.png`
- `evidencia_screenshots/homepage-tablet-768.png`
- `evidencia_screenshots/homepage-mobile-320.png`

### Destinos
- `evidencia_screenshots/destinos-desktop-1280.png`
- `evidencia_screenshots/destinos-tablet-768.png`
- `evidencia_screenshots/destinos-mobile-320.png`

### Paquetes
- `evidencia_screenshots/paquetes-desktop-1280.png`
- `evidencia_screenshots/paquetes-tablet-768.png`
- `evidencia_screenshots/paquetes-mobile-320.png`

### Chatbot
- `evidencia_screenshots/chatbot-abierto.png`
- `evidencia_screenshots/chatbot-conversacion.png`

---

## 8. Cobertura vs Requisitos

| Requisito | Estado | Evidencia |
|-----------|--------|-----------|
| RF-01 Búsqueda de vuelos | ✅ | CP-01, tests searchFlights (11) |
| RF-02 Catálogo paquetes/destinos | ✅ | Tests ViewSets (9), seed data |
| RF-03 Admin | ✅ | Admin 727 líneas, seed |
| RF-04 Interfaz adaptable | ✅ | 9 screenshots responsive |
| RF-05 Restricción chatbot | ✅ | Tests chatbot (12) |
| RF-06 Configuración LLM | ✅ | Fail-fast GROQ_API_KEY |
| RF-07 Endpoint chatbot | ✅ | Tests E2E chatbot (5) |
| RF-08 Revalidación/seatmap | ✅ | CP-05, tests específicos |
| RNF-01 Seguridad credenciales | ✅ | .env + placeholders |
| RNF-03 Eficiencia | ✅ | Prueba carga 100 usuarios |
| RNF-04 Fiabilidad | ✅ | CP-07 retry 401 |
| RNF-06 Mantenibilidad | ✅ | CI/CD pipelines |

---

## 9. Archivos de Evidencia

| Archivo | Descripción |
|---------|-------------|
| `EVIDENCIA.md` | Documento completo de evidencias |
| `cp01_resultados.json` | Resultados búsqueda Sabre (20 vuelos) |
| `cp05_resultados.json` | Resultados revalidación + seatmap |
| `evidencia_screenshots/` | 11 capturas del sistema en acción |
| `Proyecto_CorpoDG_Frontend/playwright-report/` | Reporte HTML interactivo de Playwright |
| `Proyecto_CorpoDG_Frontend/test-results/responsive/` | Capturas responsive generadas por tests |
