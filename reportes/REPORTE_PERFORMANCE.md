# Reporte de Pruebas de Performance — CorpoDG Trip593

## Pipeline: Performance Tests (k6 + GitHub Actions)

| Workflow | Estado | Última ejecución |
|----------|:------:|:----------------:|
| Performance Tests | ✅ Pass | `staging` (commit d2629d9) |
| Backend CI | ✅ Pass | `staging` (commit d2629d9) |
| Frontend CI | ✅ Pass | `staging` (commit ef6d0c7) |

## Resumen de Resultados (k6)

### Escenario de Carga

| Fase | Duración | Usuarios |
|:----:|:--------:|:--------:|
| Ramp up | 10s | 0 → 5 |
| Steady | 20s | 5 → 20 |
| Ramp down | 10s | 20 → 0 |

### Endpoints Evaluados contra RNF-02

| Endpoint | Método | Threshold RNF-02 | Resultado |
|----------|:------:|:----------------:|:---------:|
| `/api/destinos/` | GET | p95 < 3000ms | ✅ Pass |
| `/api/paquetes/` | GET | p95 < 3000ms | ✅ Pass |
| `/api/buscar-vuelos-live/` | POST | p95 < 30000ms | ✅ Pass |

## Artefactos Generados

- `k6-local-results/summary-local.json` — Resumen de métricas
- `k6-local-results/resultados-local.json` — Resultados crudos por petición

## Pipeline CI

El workflow `.github/workflows/performance.yml`:
1. Inicia PostgreSQL 17 como servicio
2. Corre migraciones + seed de datos
3. Instala k6 desde el repositorio oficial
4. Inicia Django dev server
5. Ejecuta `tests/performance/scenarios.js` con k6
6. Evalúa thresholds de RNF-02
7. Sube artifacts con resultados

## Cobertura de Requerimientos No Funcionales

| RNF | Descripción | Estado | Evidencia |
|:---:|-------------|:------:|-----------|
| RNF-01 | Seguridad (credenciales en .env) | ✅ | `servicios/LlamadosAPIS/` sin tokens hardcodeados |
| RNF-02 | Eficiencia de desempeño | ✅ | Pipeline k6 en CI valida p95 < 3s / < 30s |
| RNF-03 | Tolerancia a fallos (401 retry) | ✅ | Tests `BuscarVuelosSabre401RetryTest` (3 tests) |
| RNF-04 | Usabilidad (estados carga/error/empty) | ✅ | Tests E2E `states.spec.js` (5 tests) — API delays para capturar loading state |
| RNF-05 | Mantenibilidad (CI modular) | ✅ | 3 pipelines: Backend CI, Frontend CI, Performance Tests — todos ✅ |
| RNF-08 | Compatibilidad (Chrome + Firefox) | ✅ | Chromium en CI (todos los tests), Firefox en CI (responsive), Edge cubierto por Chromium engine |

## Notas

- **RNF-08**: Firefox ejecuta solo los tests responsive (`responsive.spec.js`) en CI. Los tests funcionales (chatbot, estados) corren en Chromium. Edge comparte motor con Chromium, cubierto automáticamente. Se recomienda verificación manual adicional en Firefox + Edge antes de entrega.

## Archivos Relacionados

- `Proyecto_CorpoDG_Backend/tests/performance/scenarios.js`
- `Proyecto_CorpoDG_Backend/.github/workflows/performance.yml`
- `reportes/REPORTE_SISTEMA.md`
- `reportes/REPORTE_BACKEND.md`
- `reportes/REPORTE_FRONTEND.md`
