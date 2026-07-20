# Arquitectura de la Solución, Metodología Aplicada y Recursos Técnicos

---

## 1. Arquitectura de la Solución

El sistema CorpoDG Trip593 se implementó bajo una **arquitectura de tres capas (three-tier)** con separación clara entre presentación, lógica de negocio y datos. Adopta el patrón **SPA (Single Page Application)** con backend RESTful y frontend desacoplado, lo que permite escalar y mantener cada componente de forma independiente.

### 1.1 Diagrama de Capas

```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENTE (Navegador)                       │
│  ┌───────────────────────────────────────────────────────┐  │
│  │              Frontend — Vue 3 SPA                      │  │
│  │  Vue Router · Axios/Fetch · Playwright E2E Tests       │  │
│  │  Deploy: Render (https://proyecto-corpodg-frontend...) │  │
│  └───────────────────────┬───────────────────────────────┘  │
│                          │ HTTP/JSON                        │
└──────────────────────────┼──────────────────────────────────┘
                           │
┌──────────────────────────┼──────────────────────────────────┐
│                    CAPA DE API REST                          │
│  ┌───────────────────────────────────────────────────────┐  │
│  │          Backend — Django 4.2 + DRF 3.14               │  │
│  │  28 endpoints REST · 4 ViewSets CRUD · 9 APIViews      │  │
│  │  Autenticación: SessionAuthentication + AllowAny       │  │
│  │  Deploy: Render (https://corpodg-backend-staging...)   │  │
│  └───────────┬──────────────┬──────────────┬────────────┘  │
│              │              │              │                │
│              ▼              ▼              ▼                │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────┐  │
│  │PostgreSQL│ │ Sabre    │ │ Groq     │ │ Stripe       │  │
│  │ 16       │ │ GDS      │ │ LLaMA 3.3│ │ Pasarela     │  │
│  │ Render   │ │ Sandbox  │ │ Chatbot  │ │ Pago (RF-09) │  │
│  └──────────┘ └──────────┘ └──────────┘ └──────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 Frontend (Capa de Presentación)

| Aspecto | Detalle |
|---------|---------|
| **Framework** | Vue 3 (Composition API) con Vite 7 como bundler |
| **Enrutamiento** | Vue Router 4 con historial HTML5 (`createWebHistory`) |
| **Peticiones HTTP** | `fetch` nativo encapsulado en `src/services/api.js` |
| **Estilos** | CSS vanilla organizado en `src/styles/` (aprox. 17,800 líneas totales) |
| **Pruebas** | Playwright 1.61 — 19 tests E2E (5 chatbot, 9 responsive, 5 estados) |
| **Despliegue** | Render, con rewrite rules para SPA routing |

**Vistas principales (14 vistas):**
- `Home.vue` — Página principal con destacados
- `Destinos.vue` — Catálogo de destinos turísticos
- `PaquetesLayout.vue`, `Paquetes.vue`, `PaquetesTipo.vue`, `PaquetesTemporada.vue` — Catálogo de paquetes con filtros
- `DetallePaquete.vue` — Detalle de paquete turístico
- `ResultadosVuelos.vue` — Búsqueda de vuelos en tiempo real vía Sabre
- `Boletos.vue` — Visualización de boletos y reservas
- `ReservaConfirmada.vue`, `ReservaCancelada.vue` — Estados post-pago
- `Renta_Autos.vue` — Módulo informativo de renta de autos

**Componentes reutilizables (11):**
`Navbar.vue`, `Footer_Info.vue`, `ChatBot.vue`, `BuscadorVuelos.vue`, `CarruselPaquetes.vue`, `CarruselVuelos.vue`, `ModalCheckoutVuelo.vue`, `ModalReservaVuelo.vue`, `ModalReservaPaquete.vue`, `ModalContacto.vue`, `ModalPdfViewer.vue`

### 1.3 Backend (Capa de Lógica de Negocio)

| Aspecto | Detalle |
|---------|---------|
| **Framework** | Django 4.2 + Django REST Framework 3.14 |
| **Base de datos** | PostgreSQL 16 (Render Free Tier) |
| **ORM** | Django ORM con 18 modelos de datos |
| **Admin** | django-jazzmin con tema darkly y branding CorpoDG |
| **Pruebas** | unittest — 78 tests (modelos, servicios Sabre, chatbot, viewsets) |
| **Linting** | Ruff |
| **Despliegue** | Render con Gunicorn |

**Modelos de datos (18):**
`Cliente`, `Solicitud`, `Destino`, `Vuelo`, `Region`, `PaisRegion`, `Ciudad`, `Aerolinea`, `Aeropuerto`, `TipoPaquete`, `Temporada`, `TipoViaje`, `PaqueteTuristico`, `ConfiguracionDestacados`, `OrdenVueloDestacado`, `OrdenPaqueteDestacado`, `OrdenDestinoDestacado`, `GoogleDrivePDFMixin`

**Endpoints REST (28):**
- **CRUD (ModelViewSet):** `/api/clientes/`, `/api/solicitudes/`, `/api/destinos/`, `/api/vuelos/`
- **Solo lectura (ReadOnlyModelViewSet):** `/api/regiones/`, `/api/paises-region/`, `/api/ciudades/`, `/api/aerolineas/`, `/api/aeropuertos/`, `/api/paquetes/`, `/api/tipos-paquete/`, `/api/temporadas/`
- **APIViews de negocio:** `/api/buscar-vuelos-live/`, `/api/revalidar-vuelo/`, `/api/seatmap/`, `/api/chatbot/`, `/api/contacto/`, `/api/health/`
- **Booking (Stripe):** `/api/booking/checkout/`, `/api/booking/confirm/`, `/api/booking/webhook/`, `/api/booking/voucher/`
- **Booking Paquetes:** `/api/paquetes/booking/checkout/`, `/api/paquetes/booking/confirm/`, `/api/paquetes/booking/voucher/`
- **AJAX Admin:** 4 endpoints en cascada (región → país → ciudad → aeropuerto)

**Módulos de servicio:**
  - `searchFlights.py` — Integración con Sabre REST API para búsqueda de vuelos (con retry automático ante 401)
  - `revalidateFlight.py` — Revalidación de tarifas contra Sabre
  - `seatMapFlight.py` — Mapa de asientos (sandbox simulado / Sabre real)
  - `bookingFlight.py` — Checkout Stripe + confirmación Sabre + voucher PDF
  - `bookingPaquete.py` — Reserva de paquetes con Stripe
  - `chatbot.py` — Function Calling con Groq (LLaMA 3.3-70B), system prompt con restricción de dominio
  - `notifications.py` — Notificaciones WhatsApp Business API
  - `LlamadosAPIS/` — Scripts de consulta a APIs externas (Sabre auth, API-Ninjas, AirLabs)

### 1.4 Integraciones Externas

| Sistema | Propósito | Modo |
|---------|-----------|------|
| **Sabre GDS** | Búsqueda de vuelos, revalidación de tarifas, mapa de asientos | Sandbox (`api.cert.platform.sabre.com`) |
| **Groq** | Motor LLM del chatbot Cory (LLaMA 3.3-70B) | Producción con API key |
| **Stripe** | Pasarela de pago (checkout, webhook, confirmación) | Sandbox/Producción (RF-09, excluido de entregables) |
| **API-Ninjas** | Consulta de aerolíneas y aeropuertos | API key |
| **AirLabs** | Consulta de ciudades | API key |
| **WhatsApp Cloud API** | Notificaciones de contacto | Configurable vía .env |

### 1.5 Infraestructura y Despliegue

```
                    ┌──────────────────┐
                    │   GitHub Actions │
                    │   (CI/CD)        │
                    └────────┬─────────┘
                             │ push staging
                             ▼
┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐
│  Render      │    │  Render          │    │  Render          │
│  Frontend    │◄──►│  Backend         │◄──►│  PostgreSQL 16   │
│  (Vue 3 SPA) │    │  (Django + DRF)  │    │  (Free Tier)     │
└──────────────┘    └──────────────────┘    └──────────────────┘
```

- **Frontend:** Render Static Site con SPA rewrite rules
- **Backend:** Render Web Service con Gunicorn + variables de entorno vía `python-decouple`
- **Base de datos:** Render PostgreSQL 16 (free tier, 1 GB)
- **CI/CD:** 3 pipelines en GitHub Actions (Backend CI, Frontend CI, Performance Tests)
- **Branches:** `staging` → Render (autodeploy), `feat/gabriel/chatbot_v2` (desarrollo)

### 1.6 Patrones Arquitectónicos

1. **REST API** — Comunicación frontend-backend mediante JSON sobre HTTP, siguiendo principios RESTful con routers de DRF.
2. **Repository Pattern** — Los módulos de servicio (`searchFlights.py`, `chatbot.py`) encapsulan la lógica de integración con APIs externas, aislando la complejidad de los ViewSets.
3. **Strategy Pattern** — SeatMap alterna entre sandbox simulado y Sabre real según configuración (`SEATMAP_SANDBOX`).
4. **Circuit Breaker** — Retry automático con backoff ante errores 401 de Sabre (CP-07).
5. **SPA + Shell** — Vue Router maneja la navegación cliente-side; el backend sirve solo datos JSON.

### 1.7 Justificación de Decisiones Tecnológicas

| Decisión | Alternativas consideradas | Justificación |
|----------|---------------------------|----------------|
| **Vue 3 (Composition API) + Vite** como framework frontend | React, Angular | Curva de aprendizaje más corta para un equipo de un solo desarrollador, mejor rendimiento en desarrollo gracias al HMR de Vite, y menor tamaño de bundle final frente a Angular. Vue Router cubre el enrutamiento SPA sin necesidad de librerías adicionales de gestión de estado, ya que el proyecto no requería un store global complejo. |
| **Django 4.2 + Django REST Framework** como backend | Node.js/Express, FastAPI | Framework "baterías incluidas": ORM maduro, sistema de autenticación y, sobre todo, panel de administración nativo — clave porque el proyecto depende fuertemente del **admin de Jazzmin** para que el personal de CorpoDG gestione destinos, paquetes y vuelos sin tocar código. Con 18 modelos y 28 endpoints, el tiempo de desarrollo con DRF fue menor que construir lo equivalente en Express o FastAPI, que habrían requerido admin panel y ORM por separado. |
| **PostgreSQL 16** en producción (SQLite en desarrollo) | MySQL, solo SQLite | PostgreSQL es soportado de forma nativa y gratuita en Render, ofrece mejor cumplimiento ACID y tipos de datos más ricos (JSON, arrays) que MySQL. SQLite se mantuvo únicamente en local por su cero-configuración, evitando dependencias de servicios externos durante el desarrollo diario. |
| **Render** como plataforma de despliegue | Heroku, AWS (EC2/Elastic Beanstalk), Railway | Render ofrece despliegue continuo directo desde GitHub (autoDeploy), free tier con base de datos PostgreSQL incluida, y configuración declarativa vía `render.yaml`. Heroku eliminó su plan gratuito y AWS implicaba una complejidad de infraestructura (VPC, balanceadores, IAM) injustificada para un proyecto Capstone de un solo desarrollador. |
| **Groq (LLaMA 3.3-70B)** como motor LLM del chatbot | OpenAI GPT-4, Anthropic Claude API | Groq ofrece inferencia de muy baja latencia (LPU) y un tier gratuito/económico suficiente para el volumen de un proyecto académico, frente al costo por token de OpenAI/Anthropic que habría limitado las pruebas iterativas del chatbot durante el desarrollo. |
| **Sabre GDS (sandbox de certificación)** para datos de vuelos | Amadeus API, datos simulados/mock | Sabre es uno de los Global Distribution Systems estándar de la industria de viajes, lo que da al sistema datos de vuelos realistas (aerolíneas, tarifas, disponibilidad) en lugar de mocks estáticos, acercando la demo a un caso de uso real de agencia de viajes. |
| **Stripe** como pasarela de pago | PayPal, pasarelas locales (PlaceToPay, PayPhone) | Documentación y SDK de Python maduros, modo sandbox robusto para pruebas sin mover dinero real, y manejo de webhooks estandarizado. Su implementación completa quedó fuera del alcance final de entregables (RF-09), pero la integración base (checkout, confirmación) se mantiene documentada. |
| **k6** para pruebas de carga/performance | JMeter, Locust (solo) | Se evaluó Locust primero (usado para una prueba puntual de 100 usuarios concurrentes), pero se adoptó k6 como estándar en el pipeline de CI por su naturaleza *CLI-first* y scripts en JavaScript, lo que facilitó integrarlo directamente en GitHub Actions sin necesidad de una UI o un proceso Python adicional corriendo en el runner. |
| **GitHub Actions** como CI/CD | Jenkins, CircleCI, GitLab CI | Integración nativa con el repositorio (sin configurar webhooks ni un servidor de CI aparte), minutos gratuitos suficientes para un repo privado académico, y sintaxis YAML declarativa que permitió versionar los 3 pipelines (Backend CI, Frontend CI, Performance) junto al código. |
| **Git submodules** para separar Backend y Frontend | Monorepo único, repos totalmente independientes | Permite versionar y desplegar cada capa de forma independiente (cada una con su propio pipeline de CI) mientras se conserva un repositorio "padre" que referencia versiones específicas de cada submódulo — útil para que el repositorio de entrega al tribunal quede autocontenido con snapshots exactos de ambos componentes. |

---

## 2. Metodología Aplicada

Se empleó **Scrum** como marco de trabajo ágil para la gestión del proyecto, adaptado al contexto académico de un proyecto Capstone con un equipo de desarrollo reducido.

### 2.1 Roles

| Rol | Responsable |
|-----|-------------|
| **Product Owner** | Gabriel Padilla — Define prioridades del producto y criterios de aceptación |
| **Scrum Master** | Gabriel Padilla — Facilita el proceso y elimina blockers |
| **Development Team** | Gabriel Padilla — Único desarrollador, implementa todos los sprints |

### 2.2 Planificación de Sprints

El proyecto se organizó en **5 sprints** de duración variable, alineados con los entregables del Capstone:

| Sprint | Objetivo | Duración | Entregables |
|:------:|----------|:--------:|-------------|
| **1** | Configuración del proyecto y CI/CD | 2 semanas | Backend Django + DRF configurado, Frontend Vue 3 + Vite, pipelines CI básicos, despliegue en Render |
| **2** | Catálogo de destinos y paquetes | 2 semanas | Modelos de datos (`Destino`, `PaqueteTuristico`, etc.), CRUD API, seed de datos Ecuador, admin jazzmin |
| **3** | Integración Sabre y búsqueda de vuelos | 2 semanas | Búsqueda en tiempo real, revalidación de tarifas, mapa de asientos, retry automático 401 |
| **4** | Chatbot Cory y experiencia de usuario | 2 semanas | Chatbot con Groq + function calling, restricción de dominio, diseño responsive, estados de carga/error |
| **5** | Pruebas, performance y cierre | 1 semana | 78 tests unitarios, 19 tests E2E, pipeline k6, reportes de resultados |

### 2.3 Artefactos Scrum

| Artefacto | Descripción |
|-----------|-------------|
| **Product Backlog** | Issues en GitHub Projects con labels `feature`, `testing`, `infra` |
| **Sprint Backlog** | Issues asignadas a cada sprint con milestone |
| **Increment** | Código funcional desplegado en Render al final de cada sprint |
| **Definition of Done** | Código en `staging`, CI verde, tests pasando, desplegado en Render |

Cada incremento incluye:
- Código fuente commiteado en la rama `feat/gabriel/chatbot_v2`
- Pipelines CI/CD verdes (Backend CI + Frontend CI)
- Despliegue automático en staging vía Render
- Pruebas unitarias y E2E correspondientes

### 2.4 Eventos Scrum

| Evento | Frecuencia | Propósito |
|--------|-----------|-----------|
| **Sprint Planning** | Al inicio de cada sprint | Definir objetivo del sprint y seleccionar items del backlog |
| **Daily Scrum** | Diario (auto-gestión) | Revisar avance y blockers (adaptado a equipo unipersonal) |
| **Sprint Review** | Fin de cada sprint | Demostrar funcionalidad desplegada en staging |
| **Sprint Retrospective** | Fin de cada sprint | Identificar mejoras para el siguiente sprint |

### 2.5 Herramientas de Gestión

| Herramienta | Uso |
|-------------|-----|
| **GitHub Projects** | Tablero Scrum con columnas To-do / In Progress / Done |
| **GitHub Issues** | Gestión de requisitos con labels (feature, testing, infra, bug) |
| **GitHub Actions** | Integración continua y validación automatizada |
| **Git** | Control de versiones con ramas por funcionalidad |

---

## 3. Recursos Técnicos Utilizados

### 3.1 Hardware

| Recurso | Especificación | Uso |
|---------|---------------|-----|
| **PC de Desarrollo** | Windows 11, 16 GB RAM, Intel Core i5 | Desarrollo de código, pruebas locales |
| **Servidor de Producción** | Render Cloud (Free Tier) | Despliegue continuo de frontend y backend |
| **Base de Datos** | Render PostgreSQL 16 (Free Tier, 1 GB) | Almacenamiento persistente |

### 3.2 Lenguajes de Programación

| Lenguaje | Versión | Componente |
|----------|---------|------------|
| Python | 3.13 | Backend Django |
| JavaScript | ES2022 | Frontend Vue 3 |
| SQL | PostgreSQL 16 | Consultas a base de datos |

### 3.3 Frameworks y Librerías — Backend

| Librería | Versión | Propósito |
|----------|---------|-----------|
| Django | 4.2.x | Framework web principal |
| djangorestframework | 3.14 | API REST |
| django-jazzmin | 3.0 | Admin dashboard con tema darkly |
| django-cors-headers | 4.3 | Configuración CORS |
| python-decouple | 3.8 | Variables de entorno |
| groq | 1.2 | Cliente Groq para chatbot LLM |
| stripe | 7.0 | Pasarela de pago |
| requests | 2.31 | Cliente HTTP para APIs externas (Sabre, API-Ninjas, AirLabs) |
| xhtml2pdf | 0.2.11 | Generación de vouchers PDF |
| psycopg2-binary | 2.9 | Conexión PostgreSQL |
| gunicorn | 23.0 | Servidor WSGI de producción |
| ruff | 0.9 | Linter y formateador |

### 3.4 Frameworks y Librerías — Frontend

| Librería | Versión | Propósito |
|----------|---------|-----------|
| Vue | 3.5.x | Framework frontend reactivo |
| Vue Router | 4.6.x | Enrutamiento SPA |
| Vite | 7.2.x | Bundler y dev server |
| Axios | 1.13.x | Cliente HTTP (integrado con fetch nativo) |
| @playwright/test | 1.61 | Tests E2E multi-browser |
| ESLint | 9.39 | Linting JavaScript |

### 3.5 Infraestructura Cloud y Servicios

| Servicio | Tipo | Propósito |
|----------|------|-----------|
| **Render** | PaaS (Platform as a Service) | Despliegue de frontend (Static Site), backend (Web Service) y PostgreSQL |
| **GitHub** | SaaS | Control de versiones, CI/CD (GitHub Actions), gestión de issues |
| **Sabre GDS** | API REST | Búsqueda de vuelos en tiempo real (sandbox de certificación) |
| **Groq** | API LLM | Inferencia del modelo LLaMA 3.3-70B para chatbot |
| **Stripe** | API de pagos | Pasarela de pago (checkout, webhooks, confirmación) |
| **API-Ninjas** | API REST | Consulta de datos de aerolíneas y aeropuertos |
| **AirLabs** | API REST | Consulta de datos de ciudades |
| **WhatsApp Cloud API** | API REST | Notificaciones de formulario de contacto |

### 3.6 Herramientas de Desarrollo

| Herramienta | Propósito |
|-------------|-----------|
| **Visual Studio Code** | Editor de código principal |
| **Git** | Control de versiones distribuido |
| **GitHub Desktop** | Interfaz gráfica para Git |
| **Node.js 20** | Entorno de ejecución para frontend |
| **pip** | Gestor de paquetes Python |
| **npm** | Gestor de paquetes Node.js |

### 3.7 Herramientas de Prueba

| Herramienta | Propósito | Resultados |
|-------------|-----------|------------|
| **Django TestCase** | Tests unitarios backend | 78 tests (0.083s) |
| **Playwright** | Tests E2E frontend multi-browser | 19 tests (Chromium + Firefox) |
| **k6** | Pruebas de performance y carga | Pipeline CI con thresholds RNF-02 |
| **Locust** | Prueba de carga 100 usuarios concurrentes | 1,795 requests, 0% fallos |
| **Ruff** | Linting Python | Integrado en CI |

### 3.8 APIs del Sistema (Endpoints)

| Endpoint | Método | Propósito |
|----------|--------|-----------|
| `GET /api/destinos/` | GET | Catálogo de destinos (con filtros) |
| `GET /api/paquetes/` | GET | Catálogo de paquetes turísticos |
| `GET /api/vuelos/` | GET | Listado de vuelos |
| `POST /api/buscar-vuelos-live/` | POST | Búsqueda en tiempo real vía Sabre |
| `POST /api/revalidar-vuelo/` | POST | Revalidación de tarifa |
| `POST /api/seatmap/` | POST | Mapa de asientos |
| `POST /api/chatbot/` | POST | Conversación con chatbot Cory |
| `POST /api/contacto/` | POST | Formulario de contacto |
| `GET /api/health/` | GET | Health check del servidor |
| `GET /api/regiones/` | GET | Regiones geográficas |
| `GET /api/paises-region/` | GET | Países por región |
| `GET /api/ciudades/` | GET | Ciudades con filtros |
| `GET /api/aerolineas/` | GET | Aerolíneas |
| `GET /api/aeropuertos/` | GET | Aeropuertos (con autocomplete) |
| `GET /api/paquetes/booking/voucher/` | GET | Descarga de voucher PDF |
| `POST /api/booking/webhook/` | POST | Webhook de Stripe |

(Endpoints de booking/pago excluidos de entregables según limitaciones del proyecto)

---

## 4. Cumplimiento de Requisitos No Funcionales

| RNF | Descripción | Estado | Herramienta/Evidencia |
|:---:|-------------|:------:|-----------------------|
| RNF-01 | Seguridad de credenciales | ✅ | `python-decouple` + `.env`, placeholders en código |
| RNF-02 | Eficiencia de desempeño | ✅ | Pipeline k6 + Locust 100 usuarios |
| RNF-03 | Tolerancia a fallos | ✅ | Retry automático 401 Sabre (CP-07) |
| RNF-04 | Usabilidad (estados) | ✅ | Tests E2E con cobertura de carga, error y vacío |
| RNF-05 | Mantenibilidad | ✅ | CI/CD automatizado, 78 tests backend, 19 E2E frontend |
| RNF-06 | Seguridad de pagos | ❌ | Excluido de entregables (RF-09) |
| RNF-07 | Compatibilidad navegadores | ✅ | Chromium (CI) + Firefox (responsive) |
| RNF-08 | Protección de datos | ✅ | Datos de Ecuador en seed, sin datos sensibles expuestos |
