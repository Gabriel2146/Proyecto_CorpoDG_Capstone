# Carta de Verificación de Pruebas de Usuario Final

**Proyecto:** CorpoDG Trip593 — Sistema Web B2C de Reservas Turísticas
**Fecha de Emisión:** 12 de julio de 2026

---

Por medio de la presente, los abajo firmantes, usuarios finales designados para la
evaluación del sistema **CorpoDG Trip593**, declaramos que hemos realizado las pruebas
funcionales correspondientes sobre el sistema desplegado en el entorno de staging
(https://corpodg-backend-staging.onrender.com — https://proyecto-corpodg-frontend.onrender.com)
y certificamos lo siguiente:

---

## 1. Alcance de las Pruebas Realizadas

| # | Módulo | Funcionalidad Probada | Resultado |
|---|--------|----------------------|-----------|
| 1 | Home | Visualización de interfaz principal, promociones y destinos destacados | ✅ |
| 2 | Vuelos | Búsqueda de vuelos por origen, destino y fecha | ✅ |
| 3 | Vuelos | Visualización de resultados con filtros | ✅ |
| 4 | Vuelos | Flujo de selección de vuelo y pago | ✅ |
| 5 | Paquetes | Navegación por regiones y países | ✅ |
| 6 | Paquetes | Visualización de detalle de paquete turístico | ✅ |
| 7 | Paquetes | Información completa (incluye, no incluye, requisitos, políticas) | ✅ |
| 8 | Destinos | Exploración de destinos turísticos | ✅ |
| 9 | Chatbot Cory | Interacción con el asistente virtual sobre vuelos | ✅ |
| 10 | Chatbot Cory | Interacción sobre paquetes turísticos | ✅ |
| 11 | Chatbot Cory | Redirección a páginas de detalle desde el chat | ✅ |
| 12 | Responsive | Visualización correcta en pantallas de 320px (móvil) | ✅ |
| 13 | Responsive | Visualización correcta en pantallas de 768px (tablet) | ✅ |
| 14 | Responsive | Visualización correcta en pantallas de 1280px (escritorio) | ✅ |
| 15 | Estados | Visualización de indicadores de carga mientras se obtienen datos | ✅ |
| 16 | Estados | Visualización de mensajes de error cuando un servicio falla | ✅ |
| 17 | Estados | Visualización de mensaje cuando no hay resultados | ✅ |
| 18 | Navegación | Transición entre todas las secciones del sitio | ✅ |
| 19 | Rendimiento | Tiempo de carga aceptable en listado de paquetes | ✅ |
| 20 | Administración | Acceso al panel de administración (Django Admin) | ✅ |

---

## 2. Entorno de Prueba

| Recurso | Detalle |
|---------|---------|
| URL Frontend | https://proyecto-corpodg-frontend.onrender.com |
| URL Backend | https://corpodg-backend-staging.onrender.com |
| URL Admin | https://corpodg-backend-staging.onrender.com/admin/ |
| Navegadores probados | Google Chrome, Mozilla Firefox, Microsoft Edge |
| Dispositivos probados | Desktop (1920×1080), Laptop (1366×768), Tablet (768×900), Móvil (320×700) |
| Fechas de prueba | 11–12 de julio de 2026 |

---

## 3. Observaciones

Durante las pruebas realizadas, se identificaron y corrigieron los siguientes hallazgos:

1. **Imágenes de paquetes**: Se actualizaron las URLs de imágenes de Unsplash que estaban
   ausentes en la versión inicial del seed de datos.
2. **Filtro por región**: Se corrigió la asignación de regiones a los paquetes turísticos
   para que el filtrado funcione correctamente.
3. **Estados de carga**: Se verificó que los indicadores de carga (`spinner`) se muestran
   durante la obtención de datos y desaparecen al completarse la operación.
4. **Chatbot**: Se confirmó que el asistente Cory responde correctamente y redirige a las
   páginas de detalle según la consulta del usuario.

No se encontraron errores críticos o bloqueantes al momento de la firma de este documento.

---

## 4. Conformidad

Declaramos que el sistema **CorpoDG Trip593** cumple con los requerimientos funcionales
y no funcionales establecidos, y que su comportamiento es el esperado para un entorno
de producción.

---

## 5. Firmas

| Nombre | Rol | Firma | Fecha |
|--------|-----|-------|-------|
| | Usuario final / Tester | _________________ | ____/____/______ |
| | Usuario final / Tester | _________________ | ____/____/______ |
| | Usuario final / Tester | _________________ | ____/____/______ |
| Milton Dávila | Desarrollador Backend | _________________ | ____/____/______ |
| Gabriel M. | Desarrollador Frontend | _________________ | ____/____/______ |

---

**Documento generado el:** 12 de julio de 2026
**Formato:** Carta de verificación para defensa de tesis
