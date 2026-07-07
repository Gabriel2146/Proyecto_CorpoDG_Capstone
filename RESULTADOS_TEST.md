# Resultados de Pruebas — CorpoDG Trip593

## Reportes separados por componente

| Reporte | Descripción |
|---------|-------------|
| 📘 [`reportes/REPORTE_BACKEND.md`](reportes/REPORTE_BACKEND.md) | 78 tests unitarios, integración Sabre, CI backend |
| 📗 [`reportes/REPORTE_FRONTEND.md`](reportes/REPORTE_FRONTEND.md) | 14 tests E2E Playwright, responsive, chatbot UI |
| 📙 [`reportes/REPORTE_SISTEMA.md`](reportes/REPORTE_SISTEMA.md) | Prueba de carga 100 usuarios, arquitectura, resumen global |

## Capturas del sistema

`reportes/capturas/frontend/` — 9 screenshots (homepage, destinos, paquetes en 320/768/1280px).

## Resumen ejecutivo

| Componente | Pruebas | Resultado |
|-----------|---------|:---------:|
| Backend | 78 tests unitarios | ✅ 78/78 PASS |
| Frontend | 14 tests E2E (Playwright) | ✅ 14/14 PASS |
| Carga | 100 usuarios concurrentes | ✅ 0% fallos (1,795 reqs) |
| Sabre CP-01 | Búsqueda de vuelos | ✅ 20 vuelos UIO→GYE |
| Sabre CP-05 | Revalidación + seatmap | ✅ $104.93 USD |
| CI Backend | Pipeline completo | ✅ ~57s |
| CI Frontend | Pipeline completo | ✅ ~1m13s |
