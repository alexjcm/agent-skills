# Dark patterns en apps mobile

Catálogo de patrones de diseño engañoso para detectar durante auditorías UX/UI. También llamados "deceptive design patterns". Leer cuando el análisis incluya evaluación de flujos de conversión, suscripciones, permisos, notificaciones o cualquier punto donde los intereses del negocio y del usuario puedan estar en conflicto.

---

## Patrones más comunes

### 1. Confirmshaming
**Qué es:** el botón de rechazo usa lenguaje que culpabiliza o avergüenza al usuario para presionarlo a aceptar.

**Ejemplos:**
- "No gracias, prefiero pagar más"
- "No, no me interesan los descuentos"
- "Prefiero perder dinero"

**Cómo detectarlo en código:** texto de los botones secundarios/de cierre en modales, popups y banners de notificación.

**Por qué es un problema:** manipula emocionalmente en lugar de informar — daña la confianza y puede generar rechazo hacia la marca.

---

### 2. Nagging
**Qué es:** interrupciones repetidas y persistentes para permisos, notificaciones, ratings o suscripciones — hasta que el usuario cede por agotamiento.

**Ejemplos:**
- Modal de permisos de notificaciones que aparece en cada sesión
- Banner de "instala la app" que no se puede cerrar permanentemente
- Rating prompt que aparece en cada apertura

**Cómo detectarlo en código:** lógica de cuándo mostrar modales/alerts, ausencia de condición de "ya rechazado".

---

### 3. False urgency / False scarcity
**Qué es:** crear presión artificial de tiempo o escasez que no refleja la realidad.

**Ejemplos:**
- Contador regresivo que se reinicia
- "¡Solo quedan 2 disponibles!" cuando es falso
- "Oferta expira en 10 min" aplicada a todos los usuarios en todo momento

**Cómo detectarlo en código:** lógica de contadores/timers, datos de stock hardcodeados o siempre bajos.

---

### 4. Misdirection
**Qué es:** el diseño dirige la atención del usuario hacia la acción que favorece al negocio, minimizando visualmente la opción que el usuario realmente quiere.

**Ejemplos:**
- Botón "Aceptar" grande y colorido vs. "Rechazar" en texto gris pequeño
- La opción de plan anual preseleccionada cuando el usuario quería el mensual
- "Continuar" que hace avanzar en un flujo que el usuario no eligió intencionalmente

**Cómo detectarlo en código:** asimetría visual en elementos de igual peso funcional, estados preseleccionados.

---

### 5. Roach Motel
**Qué es:** suscribirse o activar algo es fácil, pero cancelarlo es deliberadamente difícil.

**Ejemplos:**
- Suscripción en 2 pasos; cancelación require llamar por teléfono
- "Eliminar cuenta" enterrado en 4 niveles de configuración
- Opción de baja solo disponible una vez al mes

**Cómo detectarlo en código:** comparar profundidad de rutas de activación vs. desactivación de features.

---

### 6. Trick questions
**Qué es:** preguntas o checkboxes redactados de forma confusa, con doble negación o lenguaje ambiguo, para obtener consentimiento que el usuario no pretendia dar.

**Ejemplos:**
- "Desmarca esta casilla si no quieres dejar de recibir comunicaciones"
- Checkbox pre-marcado para suscripción al newsletter
- "¿No deseas no recibir ofertas?" (doble negación)

**Cómo detectarlo en código:** texto de labels de checkboxes, estados por defecto (`checked` en HTML).

---

### 7. Sneak into basket (Cargo oculto)
**Qué es:** agregar items, cargos o servicios al carrito/checkout sin el consentimiento explícito del usuario.

**Ejemplos:**
- Seguro de viaje preseleccionado en checkout
- "Donación sugerida" añadida automáticamente
- Cargo de procesamiento revelado solo en el último paso

**Cómo detectarlo en código:** items preseleccionados en checkout, cargos que aparecen sin interacción del usuario.

---

### 8. Disguised ads
**Qué es:** contenido publicitario diseñado para parecerse a contenido orgánico o a elementos de navegación.

**Ejemplos:**
- Resultado patrocinado que se ve igual al resultado orgánico
- Botón "Continuar" que es un anuncio
- "Artículos recomendados" que son anuncios sin etiqueta clara

**Cómo detectarlo en código:** diferencias de estilo entre contenido orgánico y patrocinado, presencia de etiqueta "Anuncio" o "Patrocinado".

---

## Cómo reportar dark patterns

Al identificar un dark pattern:
1. Nombrar el patrón específico (ej: "confirmshaming")
2. Indicar dónde aparece (pantalla, componente)
3. Explicar por qué es problemático (impacto en el usuario, riesgo legal si aplica)
4. Proponer la alternativa ética: misma función, sin manipulación

**Priorización:** los dark patterns son siempre **Crítico** o **Importante** — nunca Opcional — porque afectan directamente la confianza y potencialmente la legalidad (GDPR, leyes de protección al consumidor).

---

## Nota sobre contexto

No todo elemento que parece deceptivo lo es. Antes de reportar:
- Verificar si hay una razón funcional legítima (ej: preselección para reducir fricción genuina)
- Distinguir entre diseño descuidado y manipulación intencional
- El impacto en el usuario es el criterio principal — no la intención del equipo de diseño
