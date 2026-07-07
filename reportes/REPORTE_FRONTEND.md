# Reporte de Resultados — Frontend

**Proyecto:** CorpoDG Trip593 (Sistema Web Responsive B2C)
**Componente:** Frontend (Vue 3 + Vite) — ~17,800 líneas
**Repositorio:** `Gabriel2146/Proyecto_CorpoDG_Frontend`
**Rama:** `feat/gabriel/chatbot_v2` / `staging`
**Deploy:** `https://proyecto-corpodg-frontend.onrender.com`

---

## 1. Tests E2E (Playwright) — 14 pruebas

**Servidor:** Vite dev server en `http://localhost:5173`
**Duración:** 55.7s
**Resultado:** ✅ **14/14 PASS**

### 1.1 Chatbot Cory (5 tests)
| # | Test | Estado |
|:--:|------|:------:|
| 1 | Burbuja del chat visible en homepage | ✅ |
| 2 | Abre y cierra el chat al hacer clic en burbuja | ✅ |
| 3 | Muestra mensaje de bienvenida de Cory al abrir | ✅ |
| 4 | Botón de redirect aparece cuando chatbot devuelve acción | ✅ |
| 5 | Botón de redirect navega a `/vuelos/resultados` con query params | ✅ |

### 1.2 Diseño Responsive (9 tests)
Screenshots automáticos en 3 viewports × 3 páginas:

| Página | 320px (Mobile) | 768px (Tablet) | 1280px (Desktop) |
|--------|:---:|:---:|:---:|
| Homepage | ✅ | ✅ | ✅ |
| Destinos | ✅ | ✅ | ✅ |
| Paquetes | ✅ | ✅ | ✅ |

### Reporte HTML Interactivo
`Proyecto_CorpoDG_Frontend/playwright-report/index.html`

---

## 2. Capturas del Sistema

Las siguientes capturas muestran el sistema funcionando con datos reales (seed Ecuador).

Ubicación: `reportes/capturas/frontend/`

### Homepage
| Mobile (320px) | Tablet (768px) | Desktop (1280px) |
|:---:|:---:|:---:|
| `homepage-mobile-320.png` | `homepage-tablet-768.png` | `homepage-desktop-1280.png` |

### Destinos
| Mobile (320px) | Tablet (768px) | Desktop (1280px) |
|:---:|:---:|:---:|
| `destinos-mobile-320.png` | `destinos-tablet-768.png` | `destinos-desktop-1280.png` |

### Paquetes
| Mobile (320px) | Tablet (768px) | Desktop (1280px) |
|:---:|:---:|:---:|
| `paquetes-mobile-320.png` | `paquetes-tablet-768.png` | `paquetes-desktop-1280.png` |

---

## 3. CI/CD Pipeline

| Etapa | Duración | Estado |
|-------|:--------:|:------:|
| Set up Node 20 | — | ✅ |
| `npm ci` + build | — | ✅ |
| Install Playwright browsers | — | ✅ |
| 14 tests E2E | — | ✅ |
| **Total** | **~1m13s** | **✅** |

El pipeline sube el reporte HTML de Playwright como artifact (`playwright-report`) disponible por 30 días.

---

## 4. Deploy en Render

| Item | Estado |
|------|--------|
| URL | `https://proyecto-corpodg-frontend.onrender.com` |
| SPA routing | ✅ Rewrite rules `/**` → `/index.html` |
| API conectada | ✅ `VITE_API_URL` → backend staging |

---

## 5. Cobertura vs Requisitos

| RF | Descripción | Estado | Evidencia |
|:--:|-------------|:------:|-----------|
| RF-02 | Catálogo paquetes/destinos/vuelos | ✅ | Tests ViewSets, seed data |
| RF-04 | Interfaz adaptable | ✅ | 9 screenshots responsive |
| RF-05 | Chatbot en UI | ✅ | 5 tests E2E |
| RF-07 | Integración chatbot frontend | ✅ | ChatBot.vue + api.js |
| RNF-05 | Usabilidad | ✅ | Estados carga/error implementados |
| RNF-07 | Compatibilidad navegadores | ⏳ | Pendiente corrida manual |
