# FALTANTES — Handoff rápido

## ✅ Lo que YA está

| Item | Estado |
|------|--------|
| Código backend (Django/DRF) ~7900 líneas | ✅ |
| Código frontend (Vue 3/Vite) ~17800 líneas | ✅ |
| 78 tests unitarios (modelos, servicios, viewsets) | ✅ |
| 11 tests e2e (chatbot + responsive) | ✅ |
| CI/CD pipelines (`.github/workflows/ci.yml`) | ✅ **Pasando** |
| Repos en GitHub con commits | ✅ Pusheados a `feat/gabriel/chatbot_v2` |
| Credenciales saneadas (placeholders) | ✅ |
| Diseño responsive (320/768/1280px) | ✅ |
| GROQ_API_KEY fail-fast al boot | ✅ |
| Issues de usuario creados en GitHub | ✅ (~10 issues) |
| Labels creadas (feature/testing/infra) | ✅ |
| Capturas de CI (backend + frontend) | ✅ `tests/e2e/ci_backend.png`, `ci_frontend.png` |
| Prueba de carga 100 usuarios — 0% fallos | ✅ `loadtest_results_*.csv` |
| Documento de evidencia | ✅ `EVIDENCIA.md` |

## ❌ Lo que FALTA para entregar

### 1. Staging en Railway/Render
- [ ] Hacer deploy de backend
- [ ] Hacer deploy de frontend
- [ ] Verificar integración

### 2. Documentación Capstone (.docx)
- [ ] Alinear con el código real (backend excede el documento actual)
- [ ] Insertar capturas de CI (ya existen en `tests/e2e/`)
- [ ] Insertar resultados de prueba de carga
- [ ] Revisar RNF (AllowAny vs IsAuthenticated real)
