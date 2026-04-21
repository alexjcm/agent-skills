# Patrones de formularios mobile

Criterios detallados para evaluar formularios en apps web mobile-first. Leer cuando el análisis incluya formularios, flujos de registro, login, checkout u otros flujos de captura de datos.

---

## 1. Tipos de input y teclado

El tipo incorrecto de teclado es uno de los antipatrones más comunes y más fáciles de corregir.

| Campo | Atributo recomendado |
|-------|---------------------|
| Email | `type="email"` |
| Teléfono | `type="tel"` |
| Número (sin spinner) | `inputmode="numeric"` |
| Número decimal | `inputmode="decimal"` |
| Búsqueda | `type="search"` |
| URL | `type="url"` |
| Contraseña | `type="password"` |
| PIN / código | `inputmode="numeric" pattern="[0-9]*"` |

**Antipatrón frecuente:** usar `type="text"` para todos los campos — el usuario recibe teclado alfabético para campos numéricos.

---

## 2. Autocompletado (`autocomplete`)

Habilitar `autocomplete` reduce la fricción y el tiempo de completado en móvil.

| Campo | Valor recomendado |
|-------|------------------|
| Nombre completo | `autocomplete="name"` |
| Nombre | `autocomplete="given-name"` |
| Apellido | `autocomplete="family-name"` |
| Email | `autocomplete="email"` |
| Teléfono | `autocomplete="tel"` |
| Dirección | `autocomplete="street-address"` |
| Ciudad | `autocomplete="address-level2"` |
| Código postal | `autocomplete="postal-code"` |
| Tarjeta de crédito | `autocomplete="cc-number"` |
| Nueva contraseña | `autocomplete="new-password"` |
| Contraseña actual | `autocomplete="current-password"` |

**Antipatrón:** `autocomplete="off"` en campos donde el autofill sería útil — solo justificado en OTPs o campos de seguridad específicos.

---

## 3. Validación

### Cuándo validar
- **Inline (on blur o on input):** preferible para campos críticos — el usuario ve el error antes de enviar
- **Al enviar:** aceptable para formularios cortos simples
- **Mezcla:** validar al perder foco; re-validar al enviar

### Cómo mostrar errores
- Mensaje específico junto al campo afectado (no solo "Error en el formulario")
- El mensaje persiste hasta que el campo sea corregido
- No desaparecer el error al focus — desaparece solo cuando el dato es válido
- Color + ícono para el estado de error — no solo color (considera daltonismo)
- Scroll automático al primer campo inválido después de intentar enviar

### Antipatrones frecuentes
- Mensaje de error genérico al tope del formulario sin indicar qué campo
- Error que desaparece al hacer focus en el campo
- Validación que solo ocurre al enviar en formularios largos
- Requerir formato específico sin indicarlo antes (ej: contraseña con caracteres especiales)

---

## 4. Visibilidad del campo activo

En mobile, el teclado puede cubrir hasta el 40% de la pantalla.

**Verificar:**
- ¿Hay `scroll` automático para mantener el campo activo visible sobre el teclado?
- ¿En flujos con campos al pie de la pantalla, el layout se ajusta cuando el teclado aparece?
- ¿Los botones de acción (enviar, siguiente) siguen siendo accesibles con el teclado activo?

**Antipatrón:** campo activo cubierto por el teclado sin scroll — el usuario no ve lo que escribe.

---

## 5. Campos requeridos

- Indicar requerido con más que solo `*` — muchos usuarios no saben qué significa
- Considerar: indicar solo los opcionales (si la mayoría son requeridos, es menos ruido)
- El atributo `required` en HTML mejora la accesibilidad con screen readers
- No depender solo del color rojo para indicar requerido

---

## 6. Flujos multistep

- Mostrar progreso (paso 2 de 4, barra de progreso) — el usuario necesita saber cuánto queda
- Permitir retroceder sin perder datos ingresados
- Auto-save progresivo (localStorage) para formularios largos — resiliente a interrupciones (llamadas, cierre accidental)
- El botón "Siguiente" debe validar solo el paso actual, no todo el formulario
- No pedir información que no sea necesaria en ese paso — reducir longitud percibida

---

## 7. Labels y placeholders

| Elemento | Uso correcto |
|----------|-------------|
| `<label>` | Siempre visible, encima o a la izquierda del campo — nunca solo placeholder |
| `placeholder` | Ejemplo de formato, no sustituto del label — desaparece al escribir |
| Floating label | Aceptable si se implementa correctamente (label sube al focus/blur) |

**Antipatrón:** usar solo `placeholder` como label — desaparece cuando el usuario empieza a escribir y pierde la referencia.

---

## 8. Accesibilidad de formularios

- Cada `<input>` debe tener `<label>` asociado o `aria-label`
- Botón de envío con texto descriptivo: "Crear cuenta" mejor que "Enviar"
- Iconos en campos (ej: ícono de ojo en contraseña) con `aria-label` o `title`
- El orden de `tabindex` debe seguir el flujo visual del formulario
- Mensajes de error asociados al campo con `aria-describedby`
