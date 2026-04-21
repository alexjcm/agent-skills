---
name: reviewing-mobile-ux
description: Analiza el código fuente de apps web mobile-first y PWA para evaluar si cumplen estándares y buenas prácticas de UX/UI, identifica antipatrones y entrega recomendaciones priorizadas por impacto. Activar cuando el usuario pida revisar, auditar o analizar el código de una app web mobile, o mencione querer validar su UX/UI, navegación, formularios, accesibilidad, ergonomía táctil, dark patterns, consistencia o patrones de diseño mobile.
---

# Mobile Web UX Reviewer

Analiza el código fuente de una app web mobile-first o PWA para evaluar si cumple estándares y buenas prácticas de UX/UI, identifica antipatrones y propone mejoras priorizadas por impacto. Las fuentes en `references/sources.md` son la fuente de verdad del skill.

**Entrada principal:** código fuente de la app (componentes, HTML, CSS, rutas, etc.). Si el análisis requiere contexto visual que no puede inferirse del código, solicitar capturas de pantalla o descripción del flujo específico.

**Alcance:** apps web mobile-first y PWA. No aplica a lógica back-end, apps nativas o diseño desktop-first.

---

## Protocolo de análisis

### Orden de inspección desde código fuente
1. **Navegación** — rutas, componentes de nav, jerarquía de pantallas, estado activo
2. **Componentes de UI** — acción principal por pantalla, peso visual implícito en el markup
3. **Formularios** — tipos de `input`, `inputmode`, `autocomplete`, validación, mensajes de error
4. **Accesibilidad** — `aria-label`, `role`, `alt`, `tabindex`, semántica HTML (`button` vs `div` clickeable)
5. **Responsive y ergonomía** — breakpoints, unidades CSS, tamaños de elementos interactivos
6. **Estados y feedback** — loaders, skeleton screens, empty states, manejo de errores de red
7. **Dark patterns** — wording de botones de rechazo, salidas de flujos, frecuencia de interrupciones

### Cuándo pedir contexto visual
Solicitar capturas o descripción del flujo cuando:
- El comportamiento visual no puede inferirse del código (lógica dinámica, datos en runtime)
- Se sospecha un problema de jerarquía visual que requiere ver el resultado renderizado
- El usuario menciona una pantalla específica sin señalarla en el código
- Hay ambigüedad sobre si un elemento es visible en el flujo principal

**Código parcial o sin flujo especificado:** evaluar lo disponible, indicar qué no pudo analizarse y qué contexto adicional sería útil.

---

## Criterio y fuentes

Aplica principios documentados en las fuentes de referencia — son la fuente de verdad para criterios de evaluación y antipatrones. Ver detalle en [references/sources.md](references/sources.md).

Prioriza: claridad, jerarquía visual, navegación predecible, legibilidad, accesibilidad, facilidad táctil, consistencia y reducción de fricción.

Cita fuentes cuando la recomendación sea no obvia, contraste con la intuición del equipo o requiera respaldo técnico — omite para hallazgos ampliamente conocidos.

---

## Sesgo recomendado

**Favorece:** navegación visible y persistente, headers compactos, labels cortos y consistentes, una acción principal clara por pantalla, layouts limpios, empty states diseñados, feedback visible en estados de carga, mejoras incrementales antes que rediseños radicales.

**Desconfía de:** headers sobrecargados, hamburger menus innecesarios con pocas secciones, tabs apretadas, acciones secundarias con demasiado protagonismo, mezcla de idiomas, labels largos que se cortan, diseños que parecen desktop comprimido, dependencia exclusiva del color para estados, elementos visibles que no responden al tap (view-tap asymmetry), pantallas sin estado vacío ni feedback de carga.

---

## Qué evaluar

Evalúa solo los aspectos relevantes para el caso. Omite los que no apliquen.

### 1. Navegación
- Claridad, ubicación y previsibilidad del patrón
- Facilidad para cambiar de sección y visibilidad del estado activo
- La tab bar debe ser persistente (siempre visible al hacer scroll). Una nav bar estándar desaparece al scroll — evaluar si conviene versión sticky
- Si los links no caben en pantalla: preferir overflow horizontal con indicador visual antes de recurrir al hamburger
- Si se usa hamburger: debe acompañarse de label de texto ("Menú"), no solo el ícono — mejora la discoverability

> Para criterios detallados de selección de patrón (bottom nav, hamburger, tabs, overflow) y árbol de decisión, leer [references/navigation-patterns.md](references/navigation-patterns.md).

### 2. Jerarquía visual
- Qué se ve primero, si la acción principal destaca
- Elementos compitiendo en exceso o header consumiendo demasiado espacio
- El 79% de usuarios escanea, no lee — la jerarquía visual debe guiar la atención sin depender del texto

### 3. Claridad de uso
- Si la pantalla se entiende rápido
- Labels claros, acciones bien agrupadas, usuario sabe qué hacer primero

### 4. Ergonomía mobile
- Tamaño de controles: mínimo 48×48dp (≈9mm físicos en densidad estándar, equivalente a Material Design y Apple HIG) con separación mínima de 8px entre elementos — el crowding causa errores de tap incluso con targets bien dimensionados
- View-tap asymmetry: elementos visualmente presentes que no responden al tap
- Comodidad de uso con una mano, contenido útil visible temprano
- Persistencia de estado: en mobile el usuario recibe interrupciones (llamadas, notificaciones). La interfaz debe preservar estado — auto-save en formularios, recuperación de sesión al volver

### 5. Textos, consistencia y theming
- Labels breves, idioma consistente, wording claro de acciones y estados
- Máximo 30–40 caracteres por línea para legibilidad óptima en mobile
- Consistencia de interacción: filtros, búsquedas y cambios de vista deben comportarse igual en toda la app
- Dark mode: si existe, verificar que contraste, íconos y jerarquía se mantengan

### 6. Datos y contenido complejo
- Tablas en mobile — elegir estrategia según densidad de datos: (1) simplificar columnas si con ≤3 basta para la tarea; (2) scroll horizontal con fade/shadow en borde si se necesitan 4-6 columnas y los valores exactos importan; (3) card expandible por fila si hay jerarquía entre celdas o la pantalla es muy estrecha
- Considerar reemplazar tablas por visualizaciones cuando el usuario necesita tendencias, no valores exactos
- Empty states: evaluar qué muestra la pantalla cuando no hay datos — su ausencia es un antipatrón frecuente
- Estados de carga: debe haber feedback visible mientras se obtiene información

### 7. Accesibilidad básica
- Contraste mínimo WCAG AA: 4.5:1 texto normal, 3:1 texto grande y elementos UI
- Estados y datos que no dependan solo del color — combinar siempre con ícono, patrón o etiqueta (considera daltonismo)
- Controles principales alcanzables por teclado y con tamaños de texto aumentados

### 8. Riesgos de UX
- Fricción innecesaria, pérdida de contexto, acciones poco visibles, sobrecarga visual, errores probables

### 9. Formularios mobile
- Tipo de teclado según dato: `type="email"`, `type="tel"`, `inputmode="numeric"` — el teclado incorrecto es un antipatrón frecuente
- Validación inline (mientras el usuario completa) preferible a solo validar al enviar — reduce frustración en campos críticos
- Mensajes de error: específicos, junto al campo afectado y persistentes hasta que el error se corrija
- Campos requeridos con indicador explícito más allá del asterisco
- `autocomplete` configurado en campos de nombre, email, teléfono — el autofill bloqueado innecesariamente genera fricción
- Verificar que el teclado no cubra el campo activo sin scroll automático de compensación
- Flujos multistep: mostrar progreso, permitir retroceder, preservar datos al volver

> Ver criterios detallados en [references/form-patterns.md](references/form-patterns.md).

### 10. Dark patterns
- **Confirmshaming:** botón de rechazo con lenguaje culpabilizante ("No, prefiero pagar más")
- **Nagging:** interrupciones repetidas para permisos, notificaciones o suscripciones
- **False urgency / scarcity:** contadores o alertas de escasez que no reflejan la realidad
- **Misdirection:** la acción favorita del negocio visualmente dominante; la del usuario, minimizada
- **Roach motel:** suscribirse es fácil, cancelar está enterrado en configuración o requiere contacto
- **Trick questions:** checkboxes con doble negación o lenguaje ambiguo para obtener consentimiento

> Ver lista completa con ejemplos en [references/dark-patterns.md](references/dark-patterns.md).

---

## Formato de salida

Usa solo las secciones que aporten valor real. Omite sin mencionar las que no tengan hallazgos relevantes.

1. **Diagnóstico general** — abrir con el hallazgo más crítico en la primera oración; el lector debe identificar el problema principal sin necesidad de leer el informe completo
2. **Hallazgos principales** — aciertos y problemas relevantes
3. **Recomendaciones priorizadas** — Crítico / Importante / Opcional. Cada una: qué cambiar, por qué, beneficio esperado
4. **Propuestas concretas** — cambios específicos aplicables; incluir wireframe textual cuando la recomendación implique reorganización de layout, jerarquía o navegación — omitir para ajustes de texto, color o tamaño
5. **Antipatrones detectados** — qué patrón es problemático y por qué no conviene en ese caso
6. **Riesgos de UX** — qué puede pasar si el diseño sigue igual

Responde como consultor UX/UI: crítico pero útil, concreto, justificado, orientado a decisiones prácticas. No señales problemas por preferencia estilística — prioriza lo que afecte claridad, uso, navegación, ergonomía o accesibilidad.

---

## Reglas de decisión

- **Pocas secciones principales** → considerar bottom navigation si la nav actual genera fricción
- **Nav que desaparece al scroll** → evaluar si sticky nav o tab bar persistente mejoraría la orientación
- **Hamburger sin label de texto** → recomendar agregar "Menú" junto al ícono
- **Header con demasiadas funciones** → separar branding, navegación y acciones secundarias
- **Acción claramente principal** → darle prioridad visual, reducir competencia
- **Labels largos o mezcla de idiomas** → simplificar wording, unificar idioma
- **Mucho ruido arriba del fold** → reducir header, mostrar contenido útil antes
- **Tabla compleja en mobile** → ≤3 columnas esenciales: simplificar; 4-6 columnas con valores exactos: scroll horizontal con indicador visual; datos con jerarquía entre celdas: card expandible; usuario necesita tendencias: reemplazar con gráfico
- **Pantalla sin empty state** → recomendar diseñar el estado vacío con mensaje claro y acción sugerida
- **Sin feedback de carga** → recomendar skeleton screen o indicador visible — la ausencia genera percepción de error
- **Formularios o flujos multistep** → verificar auto-save y recuperación de estado ante interrupciones
- **Bastan cambios pequeños** → preferir mejoras incrementales antes que rediseño
- **Patrón dudoso** → no solo señalarlo: explicar cuándo sí conviene y cuándo no
- **Acciones frecuentes vs ocasionales compitiendo** → priorizar facilidad de las frecuentes

---

## Ejemplos de recomendaciones

- "Mover la navegación principal a una barra inferior fija puede mejorar la ergonomía y liberar espacio vertical si solo hay 4 secciones principales."
- "El botón de cerrar sesión no debería competir con la navegación principal si es una acción secundaria y poco frecuente."
- "Un header más compacto permitiría mostrar contenido útil antes y reduciría la sensación de pantalla saturada."
- "Revisar los nombres de sección — si mezclan términos técnicos con lenguaje cotidiano, unificar el criterio mejora la consistencia y reduce la carga cognitiva."
- "Evita usar tabs superiores apretadas cuando los labels son largos y el ancho disponible es reducido."
- "Agregar un empty state con mensaje de ayuda evita que el usuario crea que la pantalla está rota cuando no hay datos todavía."
- "El ícono de hamburger sin label dificulta que nuevos usuarios descubran la navegación — agregar 'Menú' como texto reduce esa fricción."
