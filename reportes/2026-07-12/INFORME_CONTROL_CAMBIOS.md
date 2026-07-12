# Informe de Control de Cambios

**Proyecto:** CorpoDG Trip593 — Sistema Web B2C de Reservas Turísticas
**Código:** DG-TRIP-593
**Versión del Documento:** 1.0
**Fecha de Emisión:** 12 de julio de 2026

---

## 1. Identificación del Proyecto

| Campo | Valor |
|-------|-------|
| Nombre del proyecto | CorpoDG Trip593 |
| Tipo | Sistema web responsive B2C |
| Cliente | CorpoDG |
| Equipo de desarrollo | Milton Dávila (Backend), Gabriel M. (Frontend), Tutores |
| Repositorio backend | `Gabriel2146/Proyecto_CorpoDG_Backend` |
| Repositorio frontend | `Gabriel2146/Proyecto_CorpoDG_Frontend` |
| Repositorio padre | `Gabriel2146/Proyecto_CorpoDG_Capstone` |
| URL producción backend | `https://corpodg-backend-staging.onrender.com` |
| URL producción frontend | `https://proyecto-corpodg-frontend.onrender.com` |

---

## 2. Resumen Ejecutivo de Cambios

| Etapa | Período | Cambios | Entregables |
|-------|---------|---------|-------------|
| Fundación | Ene 2026 | 3 | Repositorio, estructura base, conexión a servicios |
| Construcción inicial | Ene–Feb 2026 | 8 | APIs, integración Sabre, WhatsApp, tokens |
| Evolución | Feb–Mar 2026 | 10 | Destinos, aeropuertos, filtros, traducción |
| Chatbot v1 | Abr–Jun 2026 | 9 | Chatbot Groq, reservas Stripe, PDF, vigencia |
| Chatbot v2 + CI/CD | Jun 2026 | 12 | Redirect estratégico, pipelines CI, 78 tests |
| RNF y hardening | Jul 2026 | 14 | Seguridad, performance tests, seed, admin UX |
| Defensa | Jul 2026 | 5 | Reportes, documentos de tesis, guía de defensa |

---

## 3. Registro Detallado de Cambios

### 3.1 Fundación del Proyecto (Enero 2026)

| ID | Fecha | Descripción | Módulo | Razón | Estado |
|----|-------|-------------|--------|-------|--------|
| C-001 | 19/01/2026 | Creación del repositorio backend con Django | Backend | Inicio del proyecto | Implementado |
| C-002 | 22/01/2026 | Configuración inicial del frontend con Vue.js | Frontend | Inicio del proyecto | Implementado |
| C-003 | 22/01/2026 | Conexión inicial backend-frontend | Integración | Comunicación entre capas | Implementado |

### 3.2 Construcción de APIs e Integraciones (Enero–Febrero 2026)

| ID | Fecha | Descripción | Módulo | Razón | Estado |
|----|-------|-------------|--------|-------|--------|
| C-004 | 23/01/2026 | Creación de APIs y envío de correo | Backend | HU correo electrónico | Implementado |
| C-005 | 29/01/2026 | Integración de nuevas funcionalidades | Backend | Requerimientos adicionales | Implementado |
| C-006 | 30/01/2026 | Integración con WhatsApp | Backend | HU notificaciones | Implementado |
| C-007 | 10/02/2026 | Cambios significativos en estructura | Ambos | Refactorización mayor | Implementado |
| C-008 | 11/02/2026 | Gestión de tokens de APIs externas | Backend | Autenticación servicios | Implementado |
| C-009 | 18/02/2026 | Actualización de componentes frontend | Frontend | Mejora de UI/UX | Implementado |

### 3.3 Evolución de Funcionalidades (Febrero–Marzo 2026)

| ID | Fecha | Descripción | Módulo | Razón | Estado |
|----|-------|-------------|--------|-------|--------|
| C-010 | 25/02/2026 | Cambios en lógica de negocio | Backend | Corrección de flujos | Implementado |
| C-011 | 28/02/2026 | Control de versión 1 | Ambos | Hito del proyecto | Implementado |
| C-012 | 19/03/2026 | Mejoras en módulo de destinos | Backend | HU destinos turísticos | Implementado |
| C-013 | 21/03/2026 | Actualización de datos de aeropuertos | Backend | Datos semilla | Implementado |
| C-014 | 21/03/2026 | Implementación de filtros de búsqueda | Backend | HU búsqueda avanzada | Implementado |
| C-015 | 21/03/2026 | Traducción de interfaz | Frontend | Internacionalización | Implementado |
| C-016 | 28/03/2026 | Cambios finales etapa 1 | Ambos | Cierre de sprint | Implementado |

### 3.4 Chatbot v1 y Reservas (Abril–Junio 2026)

| ID | Fecha | Descripción | Módulo | Razón | Estado |
|----|-------|-------------|--------|-------|--------|
| C-017 | 19/04/2026 | Implementación de chatbot con Groq (HU-CH-003) | Backend | HU asistente virtual | Implementado |
| C-018 | 19/04/2026 | Endpoint POST /api/chatbot (HU-CH-004) | Backend | Comunicación con chatbot | Implementado |
| C-019 | 15/06/2026 | Reservas con Stripe + vouchers PDF + correo | Backend | HU pago y confirmación | Implementado |
| C-020 | 16/06/2026 | Desactivación automática por vigencia | Backend | HU gestión paquetes | Implementado |
| C-021 | 22/06/2026 | Documentación chatbot v2 redirect + CI/CD | Docs | Planificación | Implementado |

### 3.5 Chatbot v2 y CI/CD (Junio 2026)

| ID | Fecha | Descripción | Módulo | Razón | Estado |
|----|-------|-------------|--------|-------|--------|
| C-022 | 22/06/2026 | Redirect estratégico en chatbot (HU-CH-010) | Backend | HU redirección | Implementado |
| C-023 | 22/06/2026 | Validación de paquete_id en _build_accion | Backend | Robustez | Implementado |
| C-024 | 22/06/2026 | Pipeline CI/CD para backend (GitHub Actions) | CI/CD | Automatización | Implementado |
| C-025 | 22/06/2026 | Refactor: aceptar nombre o ID en get_detalle_paquete | Backend | UX chatbot | Implementado |
| C-026 | 24/06/2026 | Corrección de año y mapeo Sabre en chatbot | Backend | Bug fix | Implementado |

### 3.6 Hardening y Requerimientos No Funcionales (Julio 2026)

| ID | Fecha | Descripción | Módulo | Razón | Estado |
|----|-------|-------------|--------|-------|--------|
| C-027 | 05/07/2026 | Sanitización de credenciales en LlamadosAPIS (RNF-01) | Backend | Seguridad | Implementado |
| C-028 | 05/07/2026 | 68 tests unitarios + fail-fast GROQ_API_KEY | Backend | Calidad | Implementado |
| C-029 | 07/07/2026 | Gunicorn + health check + deploy Render | DevOps | Despliegue | Implementado |
| C-030 | 07/07/2026 | Permisos AllowAny para frontend público | Backend | Accesibilidad | Implementado |
| C-031 | 07/07/2026 | Seed endpoint con datos reales de Ecuador | Backend | Datos demo | Implementado |
| C-032 | 07/07/2026 | Ruff linting + smoke test en pipeline | CI/CD | Calidad | Implementado |
| C-033 | 09/07/2026 | Rediseño admin Django con branding CorpoDG | Backend | UX administración | Implementado |
| C-034 | 09/07/2026 | Dashboard admin con métricas | Backend | UX administración | Implementado |
| C-035 | 11/07/2026 | Pipeline k6 performance tests (RNF-02) | CI/CD | Rendimiento | Implementado |
| C-036 | 11/07/2026 | Reemplazo de credenciales hardcodeadas por env vars | Backend | Seguridad | Implementado |
| C-037 | 11/07/2026 | Pipeline CI frontend con Playwright E2E (RNF-04, RNF-08) | CI/CD | Calidad frontend | Implementado |
| C-038 | 11/07/2026 | Tests de estados (carga, error, vacío) + responsive | Frontend | RNF-04, RNF-08 | Implementado |
| C-039 | 12/07/2026 | Corrección de seed: IATA keys y datos completos de paquetes | Backend | Bug fix datos | Implementado |
| C-040 | 12/07/2026 | Seed endpoint mejorado: crea 6 paquetes con imágenes y regiones | Backend | Datos demo | Implementado |

---

## 4. Estado de Requerimientos

| ID | Requerimiento | Estado | Pipeline asociado |
|----|--------------|--------|-------------------|
| RF-01 | Búsqueda de vuelos Sabre | Implementado | Backend CI |
| RF-02 | Reserva de vuelos | Implementado | Backend CI |
| RF-03 | Visualización de paquetes turísticos | Implementado | Backend CI + Frontend CI |
| RF-04 | Chatbot Cory con redirect | Implementado | Backend CI |
| RF-05 | Gestión de usuarios admin | Implementado | — |
| RF-06 | Notificaciones WhatsApp | Implementado | — |
| RF-07 | Vouchers PDF | Implementado | — |
| RF-08 | Pago con Stripe | Implementado | — |
| RF-09 | Reserva de vuelos desde paquetes | Fuera de alcance | — |
| RNF-01 | Credenciales en variables de entorno | Implementado | Backend CI (ruff) |
| RNF-02 | Eficiencia (p95 paquetes/destinos < 3s, vuelos < 30s) | Implementado | Performance Tests |
| RNF-03 | Disponibilidad 99.9% (Render) | Implementado | Health check |
| RNF-04 | Usabilidad (estados de carga, error, vacío) | Implementado | Frontend CI (E2E) |
| RNF-05 | Seguridad de pagos (Stripe) | Implementado | — |
| RNF-06 | Mantenibilidad (78 tests, ruff linting) | Implementado | Backend CI |
| RNF-07 | Documentación técnica | Implementado | Reportes en /reportes |
| RNF-08 | Compatibilidad multi-browser | Implementado | Frontend CI |

---

## 5. Línea Base del Proyecto

### 5.1 Backend
- **Framework:** Django 5.x + Django REST Framework
- **Base de datos:** PostgreSQL 17
- **Tests:** 78 unitarios (pasan en 0.063s)
- **Linting:** Ruff (0 errores)
- **Deploy:** Render (gunicorn, workers=2)
- **Endpoints:** 28 (regiones, países, ciudades, aerolíneas, aeropuertos, vuelos, paquetes, destinos, chatbot, booking, seed, health)

### 5.2 Frontend
- **Framework:** Vue.js 3 + Vite
- **Tests E2E:** 19 (Playwright Chromium + Firefox)
- **Linting:** ESLint (0 errores)
- **Deploy:** Render
- **Páginas:** Home, Vuelos, Paquetes, Destinos, Chatbot, Admin

### 5.3 CI/CD
- **Backend CI:** 78 tests + ruff + smoke test + reporte artifact
- **Frontend CI:** 19 tests E2E + eslint + build + smoke test + HTML report
- **Performance CI:** k6 con 3 endpoints + thresholds + seed data real

---

## 6. Aprobaciones

| Rol | Nombre | Firma | Fecha |
|-----|--------|-------|-------|
| Desarrollador Backend | Milton Dávila | _________________ | ____/____/______ |
| Desarrollador Frontend | Gabriel M. | _________________ | ____/____/______ |
| Tutor/Tribunal | | _________________ | ____/____/______ |
| Tutor/Tribunal | | _________________ | ____/____/______ |

---

**Documento generado el:** 12 de julio de 2026
**Última revisión:** 12 de julio de 2026
**Próxima revisión programada:** N/A (fin del proyecto)
