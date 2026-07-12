# CorpoDG Trip593 — Plataforma de Viajes

Sistema web para la agencia de viajes **CorpoDG Trip593** que integra un catálogo de destinos turísticos, paquetes vacacionales, reservas con Sabre GDS, pagos con Stripe y un asistente IA (Cory) vía Groq.

## Estructura

| Repositorio | Stack | URL |
|-------------|-------|-----|
| `Proyecto_CorpoDG_Backend` | Django REST + PostgreSQL + Sabre GDS + Stripe | [Backend repo](https://github.com/Gabriel2146/Proyecto_CorpoDG_Backend) |
| `Proyecto_CorpoDG_Frontend` | Vue 3 + Vite + Playwright | [Frontend repo](https://github.com/Gabriel2146/Proyecto_CorpoDG_Frontend) |

## Quick Start

### Backend

```bash
cd Proyecto_CorpoDG_Backend
pip install -r requirements.txt
python manage.py migrate
python manage.py seed_data
python manage.py runserver
```

### Frontend

```bash
cd Proyecto_CorpoDG_Frontend
npm install
npm run dev
```

## Características principales

- Catálogo de destinos y paquetes turísticos con imágenes Unsplash
- Búsqueda y reserva de vuelos en tiempo real vía Sabre GDS
- Pagos con Stripe
- Asistente virtual Cory con IA (Groq, function calling, WhatsApp)
- Panel admin Django para gestión de contenido
- CI/CD con GitHub Actions + Playwright E2E + k6 performance tests
- Despliegue en Render (backend + PostgreSQL)

## Despliegue

El proyecto se despliega automáticamente en Render desde la rama `staging`:
- **Backend**: `corpodg-backend-staging` (Gunicorn)
- **Frontend**: `proyecto-corpodg-frontend` (Nginx static)

Ver `render.yaml` para configuración completa.
