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
| `EVIDENCIA.md` | Documento completo de evidencias |
| `FALTANTES.md` | Pendientes para entrega |
| `cp01_resultados.json` | Resultados búsqueda Sabre |
| `cp05_resultados.json` | Resultados revalidación + seatmap |
