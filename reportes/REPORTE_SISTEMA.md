# Reporte del Sistema — CorpoDG Trip593

## Arquitectura

| Capa | Tecnología | Deploy |
|------|-----------|--------|
| Frontend | Vue 3 + Vite | `https://proyecto-corpodg-frontend.onrender.com` |
| Backend | Django + DRF | `https://corpodg-backend-staging.onrender.com` |
| Base de datos | PostgreSQL 16 | Render Free Tier (`corpodg-db-staging`) |
| GDS | Sabre (sandbox) | API externa |
| Chatbot | Groq (LLaMA 3.3) | API externa |

## Prueba de Carga — 100 usuarios concurrentes

**Herramienta:** Locust 2.44.4 | **Duración:** 60s | **Usuarios:** 100 (rampa 10/s)

| Endpoint | Requests | Fallos | Avg (ms) | p50 | p95 | p99 |
|----------|:--------:|:------:|:--------:|:---:|:---:|:---:|
| GET /api/destinos/ | 741 | 0 | 669 | 550 | 2,200 | 2,700 |
| GET /api/destinos/?destacado | 164 | 0 | 535 | 440 | 2,100 | 2,600 |
| GET /api/paquetes/ | 528 | 0 | 763 | 720 | 2,100 | 3,000 |
| GET /api/vuelos/ | 362 | 0 | 4,752 | 5,100 | 6,600 | 7,600 |
| **Total** | **1,795** | **0 (0%)** | **1,508** | 660 | 5,500 | 7,600 |

## Pipeline de Pruebas de Performance (k6 + GitHub Actions)

Se implementó un pipeline automatizado en `.github/workflows/performance.yml` que:

1. **Configura PostgreSQL 17** como servicio de CI
2. **Ejecuta migraciones y seed** de la base de datos
3. **Inicia el servidor Django** en modo desarrollo
4. **Ejecuta k6** con el script `tests/performance/scenarios.js`
5. **Evalúa thresholds** según RNF-02:
   - `destinos` p(95) < 3000ms
   - `paquetes` p(95) < 3000ms
   - `vuelos_live` p(95) < 30000ms
6. **Sube artifact** con resultados JSON (summary + raw)

### Escenario de Carga (k6)

| Fase | Duración | Usuarios simultáneos |
|:----:|:--------:|:--------------------:|
| Ramp up | 10s | 0 → 5 |
| Steady | 20s | 5 → 20 |
| Ramp down | 10s | 20 → 0 |

### Endpoints Evaluados

| Endpoint | Método | Threshold RNF-02 |
|----------|:------:|:----------------:|
| `/api/destinos/` | GET | p95 < 3s |
| `/api/paquetes/` | GET | p95 < 3s |
| `/api/buscar-vuelos-live/` | POST | p95 < 30s |

### Resultados de la última ejecución

Ver `reportes/REPORTE_PERFORMANCE.md` para resultados detallados.

---

## Resultados Globales

| Componente | Pruebas | Resultado |
|-----------|---------|:---------:|
| Backend | 78 tests unitarios | ✅ 78/78 |
| Frontend | 14 tests E2E (Playwright) | ✅ 14/14 |
| Carga | 100 usuarios concurrentes | ✅ 0% fallos |
| Sabre CP-01 | Búsqueda de vuelos | ✅ 20 vuelos |
| Sabre CP-05 | Revalidación + seatmap | ✅ $104.93 |
| Sabre CP-07 | Retry 401 | ✅ 2 tests |
| CI Backend | Pipeline completo | ✅ ~57s |
| CI Frontend | Pipeline completo | ✅ ~1m13s |

## Archivos de Reporte

| Archivo | Descripción |
|---------|-------------|
| `reportes/REPORTE_BACKEND.md` | Tests unitarios, Sabre, CI backend |
| `reportes/REPORTE_FRONTEND.md` | Tests E2E, responsive, chatbot UI |
| `reportes/capturas/frontend/` | 9 screenshots del sistema |
| `reportes/REPORTE_PERFORMANCE.md` | Pipeline de performance y cobertura RNF |
| `EVIDENCIA.md` | Documento completo de evidencias |
| `FALTANTES.md` | Pendientes para entrega |
| `cp01_resultados.json` | Resultados búsqueda Sabre |
| `cp05_resultados.json` | Resultados revalidación + seatmap |
