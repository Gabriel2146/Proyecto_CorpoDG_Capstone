# Evidencia — CorpoDG Trip593

## 1. CI/CD Pipelines

### Backend CI ✅
| Run ID | Branch | Status | Duración | Fecha |
|--------|--------|--------|----------|-------|
| 28756530927 | feat/gabriel/chatbot_v2 | ✅ success | 57s | 2026-07-05 |

Pasos ejecutados:
1. Set up Python 3.13
2. Install dependencies
3. Django system check ✅
4. Migrations (PostgreSQL 17) ✅
5. **78 tests unitarios** ✅

![Backend CI](Proyecto_CorpoDG_Frontend/tests/e2e/ci_backend.png)

### Frontend CI ✅
| Run ID | Branch | Status | Duración | Fecha |
|--------|--------|--------|----------|-------|
| 28756533435 | feat/gabriel/chatbot_v2 | ✅ success | 1m13s | 2026-07-05 |

Pasos ejecutados:
1. Set up Node 20
2. npm ci + build ✅
3. Playwright E2E tests ✅

![Frontend CI](Proyecto_CorpoDG_Frontend/tests/e2e/ci_frontend.png)

### Badges
- Backend: `https://github.com/MiltonDavila123/Proyecto_CorpoDG_Backend/workflows/Backend%20CI/badge.svg`
- Frontend: `https://github.com/MiltonDavila123/Proyecto_CorpoDG_Frontend/workflows/Frontend%20CI/badge.svg`

---

## 2. Prueba de Carga — 100 usuarios concurrentes

**Herramienta:** Locust 2.44.4  
**Duración:** 60s  
**Usuarios:** 100 (rampa 10/s)  
**Endpoints probados:** destinos, destacados, paquetes, vuelos

| Endpoint | # reqs | Fallos | Avg (ms) | p50 | p95 | p99 |
|----------|--------|--------|----------|-----|-----|-----|
| GET /api/destinos/ | 741 | 0 | 669 | 550 | 2,200 | 2,700 |
| GET /api/destinos/?destacado | 164 | 0 | 535 | 440 | 2,100 | 2,600 |
| GET /api/paquetes/ | 528 | 0 | 763 | 720 | 2,100 | 3,000 |
| GET /api/vuelos/ | 362 | 0 | 4,752 | 5,100 | 6,600 | 7,600 |
| **Total** | **1,795** | **0 (0%)** | **1,508** | 660 | 5,500 | 7,600 |

**Conclusión:** 0% de fallos con 100 usuarios concurrentes. Los endpoints de catálogo (destinos, paquetes) responden en <1s (p50). Vuelos necesita optimización (índices DB) — promedia 4.7s.

---

## 3. Tablero Scrum — Issues creados

### Backend (MiltonDavila123/Proyecto_CorpoDG_Backend)
| # | Título | Label |
|---|--------|-------|
| 1 | Sistema de administración | feature |
| 2 | Navegación de paquetes, destinos, vuelos | feature |
| 3 | Interfaz adaptable | feature |
| 4 | Chatbot con restricción de dominio | feature |
| 5 | Revalidación de tarifa y mapa de asientos | feature |
| 6 | Configuración segura GROQ_API_KEY | feature |
| 7 | Reserva y pago de paquetes turísticos | feature |
| 8 | Endpoint único de conversación | feature |
| - | Pruebas unitarias (78 tests) | testing |
| - | CI/CD Pipeline | infra |

### Frontend (MiltonDavila123/Proyecto_CorpoDG_Frontend)
| # | Título | Label |
|---|--------|-------|
| 1 | CI/CD Pipeline Frontend | infra |
| - | RF-04: Interfaz adaptable y navegación | feature |
| - | RF-05/RF-07: Chatbot en UI | feature |
| - | Catálogo de destinos turísticos | feature |

> **Nota:** Para crear el tablero visual en GitHub Projects, ir a la pestaña Projects del repositorio y seleccionar "Board" template. Agregar las issues existentes y mover a columnas To-do / In Progress / Done. El token de GitHub usado no tiene scope `project` para crearlo automáticamente.

---

## 4. Pruebas Unitarias — 78 tests

| Módulo | Tests | Estado |
|--------|-------|--------|
| Modelos (validadores, Destino, Paquete, ConfigDestacados) | 18 | ✅ |
| searchFlights (_construir_ids_fuente, formatear_duracion, ordenamiento, retry 401) | 11 | ✅ |
| revalidateFlight (_normalizar_segmento, payload, retry 401) | 7 | ✅ |
| seatMapFlight (sandbox, segmentos, payload, mapa asientos) | 7 | ✅ |
| ViewSets catálogo (filtros Destino/Paquete/Vuelo) | 9 | ✅ |
| Chatbot (existente) | ~10 | ✅ |
| Tests E2E responsive (9 screenshots 320/768/1280px) | 9 | ✅ |
| Tests E2E chatbot (flujo feliz) | 2 | ✅ |

---

## 5. Diseño Responsive

Capturas en 320px, 768px, 1280px para:
- Homepage
- Destinos
- Paquetes

Ubicación: `Proyecto_CorpoDG_Frontend/tests/e2e/`

---

## 6. Pruebas de Sistema — Sabre Sandbox

### CP-01: Búsqueda de vuelos en tiempo real ✅ PASS
| Campo | Valor |
|-------|-------|
| Entrada | UIO → GYE, 2026-08-15, 1 adulto |
| Resultados | 20 vuelos encontrados |
| Ordenamiento | Correcto (escalas → duración → precio) |
| Precio más bajo | $104.93 USD (LATAM, directo, 53 min) |
| Evidencia | `cp01_resultados.json` |

### CP-05a: Revalidación de tarifa ✅ PASS
| Campo | Valor |
|-------|-------|
| Disponible | ✅ True |
| Precio confirmado | $104.93 USD |
| Aerolínea validadora | LA |
| Última fecha compra | 2026-07-07 |

### CP-05b: Mapa de asientos ✅ PASS
| Campo | Valor |
|-------|-------|
| Vuelo | LA1419 UIO→GYE |
| Cabinas | Economy (Y) — 120 asientos en 20 filas |
| Modo | Sandbox (simulado) |
| Evidencia | `cp05_resultados.json` |

### CP-07: Reintento automático ante 401 ✅ PASS
Cubierto por 2 tests unitarios:
- `test_buscar_vuelos_retry_401`
- `test_buscar_vuelos_doble_401`

---

## 7. Pendiente para entregar

- [ ] **Staging en Railway/Render**
- [ ] **Documentación Capstone (.docx)** — alinear con el código real, insertar capturas
