# FALTANTES — Handoff rápido

## ✅ Lo que YA está

| Item | Estado |
|------|--------|
| Código backend (Django/DRF) ~7900 líneas | ✅ |
| Código frontend (Vue 3/Vite) ~17800 líneas | ✅ |
| 78 tests unitarios (modelos, servicios, viewsets) | ✅ |
| 14 tests e2e (chatbot + responsive) | ✅ |
| Reporte HTML Playwright generado | ✅ |
| CI/CD pipelines (`.github/workflows/ci.yml`) | ✅ **Pasando** |
| CI sube reporte como artifact (siempre) | ✅ |
| Repos en GitHub con commits | ✅ Pusheados a `feat/gabriel/chatbot_v2` |
| Credenciales saneadas (placeholders) | ✅ |
| Diseño responsive (320/768/1280px) | ✅ |
| GROQ_API_KEY fail-fast al boot | ✅ |
| Issues de usuario creados en GitHub | ✅ (~10 issues) |
| Labels creadas (feature/testing/infra) | ✅ |
| Prueba de carga 100 usuarios — 0% fallos | ✅ |
| Pruebas sistema Sabre (CP-01/05/07) | ✅ PASS |
| Deploy backend en Render | ✅ `corpodg-backend-staging.onrender.com` |
| Deploy frontend en Render | ✅ `proyecto-corpodg-frontend.onrender.com` |
| PostgreSQL en Render | ✅ `corpodg-db-staging` |
| Seed de datos ejecutado | ✅ Admin + datos Ecuador |
| Informe profesional RESULTADOS_TEST.md | ✅ Creado |
| Capturas reales del sistema | ✅ `evidencia_screenshots/` (11 capturas) |
| EVIDENCIA.md actualizado con deploy | ✅ |
## ❌ Lo que FALTA para entregar

### 1. Documentación Capstone (.docx)
- [ ] Alinear con el código real (backend excede el documento actual)
- [ ] Insertar capturas de `evidencia_screenshots/`
- [ ] Insertar resultados de `RESULTADOS_TEST.md`
- [ ] Revisar RNF (AllowAny vs IsAuthenticated real)
- [ ] Insertar capturas de prueba de carga
- [ ] Insertar capturas de sistema Sabre (CP-01/05)

### 2. Configuración adicional en Render
- [ ] SMTP email si se necesitan vouchers
- [ ] GROQ_API_KEY para chatbot en producción

### 3. Tests faltantes
- [ ] RNF-05: Verificar estados carga/error/sin resultados en todas las vistas
- [ ] RNF-07: Corrida manual Chrome/Firefox/Edge
