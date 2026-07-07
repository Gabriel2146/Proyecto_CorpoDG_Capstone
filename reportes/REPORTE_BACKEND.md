# Reporte de Resultados — Backend

**Proyecto:** CorpoDG Trip593 (Sistema Web Responsive B2C)
**Componente:** Backend (Django/DRF) — ~7,900 líneas
**Repositorio:** `Gabriel2146/Proyecto_CorpoDG_Backend`
**Rama:** `feat/gabriel/chatbot_v2` / `staging`
**Deploy:** `https://corpodg-backend-staging.onrender.com`

---

## 1. Tests Unitarios — 78 pruebas

**Comando:** `python manage.py test servicios --verbosity=2`
**Duración:** 0.083s
**Resultado:** ✅ **78/78 PASS**

| Módulo | Tests | Estado |
|--------|------:|:------:|
| Modelos — validadores | 18 | ✅ |
| `searchFlights` — IDs, duración, ordenamiento, retry 401 | 11 | ✅ |
| `revalidateFlight` — payload, segmentos, normalización | 7 | ✅ |
| `seatMapFlight` — payload, segmentos, sandbox | 7 | ✅ |
| ViewSets — filtros Destino/Paquete/Vuelo | 9 | ✅ |
| Chatbot — tools, accion, redirect | ~12 | ✅ |
| Paquete Turístico — vencimiento, vigencia | 5 | ✅ |
| ConfigDestacados | 2 | ✅ |
| Sandbox — activo/inactivo | 3 | ✅ |
| BuildAccion — redirect vuelos/paquetes | 6 | ✅ |
| **Total** | **78** | **✅** |

### Resultado completo
```
Found 78 test(s).
...
Ran 78 tests in 0.083s
OK
```

---

## 2. Pruebas de Sistema — Integración Sabre (Sandbox)

### CP-01: Búsqueda de vuelos en tiempo real ✅

| Campo | Resultado |
|-------|-----------|
| Ruta | UIO → GYE |
| Fecha | 2026-08-15 |
| Pasajeros | 1 adulto |
| Vuelos encontrados | 20 |
| Ordenamiento | Correcto (escalas → duración → precio) |
| Precio más bajo | $104.93 USD (LATAM, directo, 53 min) |

Evidencia: `cp01_resultados.json`

### CP-05a: Revalidación de tarifa ✅

| Campo | Resultado |
|-------|-----------|
| Disponible | ✅ True |
| Precio confirmado | $104.93 USD |
| Aerolínea validadora | LA (LATAM) |

### CP-05b: Mapa de asientos ✅

| Campo | Resultado |
|-------|-----------|
| Vuelo | LA1419 UIO→GYE |
| Cabina | Economy (Y) |
| Asientos | 120 en 20 filas |

Evidencia: `cp05_resultados.json`

### CP-07: Reintento automático ante 401 ✅
Cubierto por 2 tests unitarios:
- `test_buscar_vuelos_retry_401` — force-refresca token y reintenta
- `test_buscar_vuelos_doble_401` — dos 401 consecutivos retornan error

---

## 3. CI/CD Pipeline

| Etapa | Duración | Estado |
|-------|:--------:|:------:|
| Set up Python 3.13 | — | ✅ |
| Instalar dependencias | — | ✅ |
| Django system check | — | ✅ |
| Migraciones (PostgreSQL 17) | — | ✅ |
| 78 tests unitarios | — | ✅ |
| **Total** | **~57s** | **✅** |

El pipeline sube el reporte de tests como artifact (`backend-test-report`) disponible por 30 días.

---

## 4. Deploy en Render

| Item | Estado |
|------|--------|
| URL | `https://corpodg-backend-staging.onrender.com` |
| Health check | ✅ `GET /api/health/` |
| Base de datos | PostgreSQL 16 (free tier) |
| CORS | ✅ Configurado para frontend |
| Seed ejecutado | ✅ Admin `admin` / `Admin123!` + datos Ecuador |

---

## 5. Cobertura vs Requisitos

| RF | Descripción | Estado | Evidencia |
|:--:|-------------|:------:|-----------|
| RF-01 | Búsqueda de vuelos en tiempo real | ✅ | CP-01, 11 tests searchFlights |
| RF-03 | Sistema de administración | ✅ | Admin 727 líneas, seed |
| RF-05 | Restricción de dominio chatbot | ✅ | Tests chatbot |
| RF-06 | Configuración segura LLM | ✅ | Fail-fast GROQ_API_KEY |
| RF-07 | Endpoint único chatbot | ✅ | ChatbotView + tests |
| RF-08 | Revalidación tarifa / seatmap | ✅ | CP-05, tests específicos |
| RNF-01 | Seguridad credenciales | ✅ | .env + placeholders |
| RNF-03 | Eficiencia desempeño | ✅ | Prueba carga 100 usuarios |
| RNF-04 | Fiabilidad | ✅ | CP-07 retry 401 |
| RNF-06 | Mantenibilidad | ✅ | 78 tests + CI/CD |
