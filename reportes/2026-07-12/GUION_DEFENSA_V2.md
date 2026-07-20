# Guión Defensa V2 — CorpoDG Trip593

**Contexto:** presentación + demo local (127.0.0.1). El guión NO repite lo que ya está escrito en las diapositivas — expande, conecta y narra.

---

## Preparación local

Antes de entrar, tener abierto y cargado:

- [ ] Frontend B2C: `http://127.0.0.1:5173/`
- [ ] Admin panel: `http://127.0.0.1:8000/admin/`
- [ ] GitHub Actions: `https://github.com/anomalyco/Proyecto_Capstone/actions` (pipelines verdes)
- [ ] Diagrama draw.io/png abierto para señalar

---

## Estructura (14 min total = ~7:30 hablado + ~6:30 demo)

### Slide 1 — Portada (0:15)

> Buenos días, soy Gabriel Padilla y presento CorpoDG Trip593, un sistema web de cotización turística en tiempo real.

Solo nombre + título. No leer nada más.

---

### Slide 2 — El problema (0:30)

**NO leer los bullets.** Cuenta el contexto:

> La agencia CorpoDG cotiza vuelos y paquetes turísticos. Hoy, ese proceso es manual: el cliente llama, el asesor busca en varias pantallas, tarda minutos en responder. No hay un lugar unificado donde el cliente pueda ver opciones de vuelo, comparar paquetes, explorar destinos y contactar a la agencia — todo desde el celular.

Idea fuerza: *"La agencia necesita estar donde el cliente está: online, en cualquier dispositivo, en tiempo real."*

---

### Slide 3 — La propuesta (0:45)

NO enumerar los 4 puntos. Explica la lógica de diseño:

> Construí una plataforma B2C con 4 capacidades que se potencian entre sí: un catálogo que la agencia misma actualiza sin saber código; búsqueda de vuelos conectada al sistema que usa la industria real; un chatbot que entiende de viajes y puede consultar vuelos en vivo; y todo funciona en cualquier pantalla sin perder calidad — probado automáticamente en 3 tamaños.

---

### Slide 4 — Arquitectura (2:30)

**Apóyate en el diagrama.** Señala mientras hablas, no leas la slide:

> El sistema sigue 3 capas independientes. Frontend Vue 3 con Vite — 14 vistas, 11 componentes. Backend Django REST Framework — 28 endpoints, 18 modelos. PostgreSQL 16. Todo conectado por HTTP/JSON.
>
> [Señalar Sabre en el diagrama] La búsqueda de vuelos no es simulada: va contra el GDS Sabre en su sandbox de certificación. [Señalar Groq] El chatbot Cory usa LLaMA 3.3 de 70B con function calling — solo puede invocar herramientas que definí, no navega libre.
>
> [Señalar Render + GitHub Actions] El despliegue es continuo: cada push a staging corre 3 pipelines automáticos y si pasa todo, Render despliega solo.

---

### Slide 5 — Integraciones externas (1:00)

La slide ya lista las 3 integraciones. No las repitas. Añade el detalle que NO está:

> La integración con Sabre fue la más compleja del proyecto: autenticación OAuth2 con token rotativo, tres endpoints diferentes (búsqueda, revalidación, seatmap), y un mecanismo de retry automático si el token expira a mitad de una consulta — el sistema lo refresca y reintenta sin que el usuario lo note. Esto está cubierto por pruebas específicas.

---

### Slide 6 — Decisiones técnicas (1:00)

La slide tiene 5 filas con comparaciones. No las leas. Destaca la lógica general:

> Cada tecnología la elegí por una razón concreta ligada a las restricciones del proyecto: equipo de 1 persona, presupuesto cero, necesidad de que la agencia administre contenido sin programar. Django fue la decisión más importante: su admin nativo me permitió darle a CorpoDG un panel completo sin escribir una sola línea de frontend de administración. Render me evitó toda la complejidad de AWS. Y Groq me dejó iterar el chatbot decenas de veces sin costo.

---

### Slide 7 — Metodología (0:30)

Slide visual con flechas. Solo explica cómo se adaptó Scrum:

> Scrum adaptado a equipo unipersonal. 5 sprints de 1-2 semanas. Cada sprint cerraba con algo funcional desplegado en staging. El backlog en GitHub Issues, el tablero en Projects. La Definition of Done era siempre: CI en verde + tests pasando + deploy en staging.

Corta. Esto no necesita más.

---

### Slide 8 — Calidad y recursos (0:45)

La slide muestra los números grandes. No los leas. Destaca lo más relevante:

> 78 tests en backend, 19 end-to-end con Playwright en Chromium y Firefox, y un pipeline de performance con k6 que corre en cada push. Además hice una prueba de carga con Locust: 100 usuarios concurrentes, casi 1800 peticiones, 0% de error. Los requisitos no funcionales los estructuré bajo ISO/IEC 25010 — cubro eficiencia, fiabilidad, seguridad, usabilidad, mantenibilidad y compatibilidad.

---

### Slide 9 — Demo título (0:10)

> Vamos a verlo funcionando en vivo.

Pausa. Cambiar mentalmente a modo demo.

---

### Slide 10 — Demo roadmap + demo real (6:30)

**No leer la slide.** Es una guía visual para el tribunal. Usa la slide como menú, pero ejecuta la demo mientras hablas:

**1. Homepage + responsive (0:40)**
> [En frontend local: http://127.0.0.1:5173/] Esta es la página principal. Destinos destacados, paquetes. Voy a redimensionar — [arrastrar ventana] — el layout se adapta. Probado en 320, 768 y 1280 píxeles con Playwright automáticamente.

**2. Catálogo de destinos (0:35)**
> [Navegar a Destinos] Cada destino tiene su PDF informativo. TODO el contenido se administra desde el panel Django — la agencia no necesita tocar código.

**3. Paquetes con filtros (0:35)**
> [Navegar a Paquetes, aplicar filtro región/costa] Filtros por región, tipo de viaje, temporada. Precios, duración, aerolíneas.

**4. Búsqueda de vuelos Sabre (1:15)**
> [Ir a Vuelos, seleccionar UIO → GYE] Esta es la funcionalidad principal. Busco Quito a Guayaquil. La consulta viaja en vivo a Sabre — no son datos precargados. [Esperar resultados] Resultados ordenados por precio, escalas y duración. Cada tarifa se puede revalidar — el sistema verifica que el precio sigue vigente.

**5. Chatbot Cory (1:00)**
> [Abrir chat] "¿Qué destinos tienen?" [Esperar respuesta — consulta catálogo]. "Quiero un vuelo de Quito a Guayaquil" — Cory consulta Sabre también. [Fuera de dominio] "Recomiéndame una película" — Cory redirige al tema de viajes.

**6. Panel administración (1:00)**
> [http://127.0.0.1:8000/admin/] Panel Jazzmin — la agencia gestiona destinos, paquetes, vuelos, regiones, configuración de destacados. Todo sin programar. [Entrar a ConfiguracionDestacados] Aquí se define qué vuelos, paquetes y destinos aparecen destacados en la homepage y en qué orden.

**7. CI/CD GitHub Actions (0:30)**
> [Mostrar Actions] Cada push corre 3 pipelines — Backend, Frontend, Performance. Los 3 en verde. Esto garantiza que cualquier cambio futuro no rompa lo que ya funciona.

---

### Slide 11 — Cierre (0:30)

> En resumen, CorpoDG Trip593 es un sistema completo y desplegado que resuelve la cotización turística en tiempo real con una arquitectura desacoplada, integraciones reales a GDS Sabre e IA generativa, calidad alineada a ISO/IEC 25010, y un pipeline de CI/CD automatizado que mantiene el producto estable. Quedo atento a sus preguntas.

---

## Cómo memorizarlo

- **No memorices el texto.** Memoriza la **estructura**: para cada diapositiva, sabes qué IDEA quieres transmitir (no qué frases).
- **Practica con cronómetro** 2-3 veces en voz alta. Ajusta lo que sobre.
- **En la demo** no narres cada clic — deja que la pantalla hable. Solo contextualiza lo que están viendo.
- **Si te trabas**, respira y di "en concreto..." para reenfocar.
- **Si preguntan durante la presentación**, responde breve y retoma: "Eso justo lo cubro en la siguiente sección" o "Buena pregunta, lo respondo ahora".

## Preguntas preempted (no esperes a que pregunten)

Ya quedan respondidas en el discurso:

| Pregunta | Dónde |
|---|---|
| ¿Por qué no hay login? | Slide 3 — sistema B2C |
| ¿Qué pasa si Sabre falla? | Slide 5 — retry automático |
| ¿Cómo restringen al chatbot? | Slide 4/5 — function calling |
| ¿Estándar de calidad? | Slide 8 — ISO/IEC 25010 |
| ¿Carga concurrente? | Slide 8 — Locust 100 usuarios |
| ¿Por qué Django? | Slide 6 — admin nativo |
| ¿Credenciales seguras? | Slide 6 — python-decouple + variables de entorno |
