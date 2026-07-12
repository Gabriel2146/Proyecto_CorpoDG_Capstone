# CorpoDG Trip593 — Plataforma de Viajes

Monorepo contenedor del sistema web de **CorpoDG Trip593**, agencia ecuatoriana de viajes. Integra catálogo de destinos y paquetes turísticos, reservas de vuelos en vivo con Sabre GDS, pagos con Stripe, y asistente IA (Cory).

| Submódulo | Stack | URL |
|-----------|-------|-----|
| `Proyecto_CorpoDG_Backend` | Django 6.0 + DRF + PostgreSQL | [github.com/Gabriel2146/Proyecto_CorpoDG_Backend](https://github.com/Gabriel2146/Proyecto_CorpoDG_Backend) |
| `Proyecto_CorpoDG_Frontend` | Vue 3 + Vite + Playwright | [github.com/Gabriel2146/Proyecto_CorpoDG_Frontend](https://github.com/Gabriel2146/Proyecto_CorpoDG_Frontend) |

## Inicio rápido

```bash
git clone --recurse-submodules https://github.com/Gabriel2146/Proyecto_CorpoDG_Capstone.git
cd Proyecto_CorpoDG_Capstone

# Backend
cd Proyecto_CorpoDG_Backend
pip install -r requirements.txt
python manage.py migrate
python manage.py seed_data
python manage.py runserver

# Frontend (otra terminal)
cd Proyecto_CorpoDG_Frontend
npm install
npm run dev
```

## Funcionalidades principales

- **Catálogo turístico** — 10 regiones, destinos, paquetes con imágenes Unsplash
- **Vuelos en vivo** — búsqueda y reserva vía Sabre GDS (API certificada)
- **Pagos** — checkout con Stripe + webhooks + vouchers PDF
- **Asistente Cory** — chatbot con IA (Groq, function calling, WhatsApp)
- **Admin Django** — panel con jazzmin para gestión de contenido
- **CI/CD** — GitHub Actions + Playwright E2E + k6 performance tests
- **Despliegue** — Render (backend Gunicorn + PostgreSQL, frontend Nginx)

## Arquitectura

El frontend Vue 3 consume la API REST del backend Django. El backend orquesta llamadas a Sabre GDS (vuelos), Stripe (pagos), Groq (chatbot IA) y WhatsApp (notificaciones). La base de datos PostgreSQL contiene catálogo, clientes y reservas.
