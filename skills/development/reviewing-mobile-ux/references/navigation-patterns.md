# Patrones de navegación mobile

Referencia para decidir qué patrón de navegación conviene y cuándo. Usar al evaluar o recomendar cambios de navegación en apps web mobile-first o PWA.

---

## 1. Bottom Navigation (Tab Bar fija)

**Qué es:** barra inferior persistente con 3-5 íconos + labels. Siempre visible al hacer scroll.

**Cuándo usar:**
- 3-5 secciones principales de igual peso
- El usuario necesita cambiar de sección frecuentemente
- La app tiene flujos paralelos e independientes entre sí

**Cuándo evitar:**
- Más de 5 secciones (genera crowding táctil)
- Flujo lineal o de una sola dirección (wizard, onboarding)
- Pantallas de contenido completo que requieren scroll horizontal

**Antipatrón frecuente:** tab bar que desaparece al scroll — el usuario pierde orientación. Usar posición fija (`position: fixed`).

---

## 2. Top Navigation Bar

**Qué es:** barra superior con título de pantalla, ícono de back y acciones contextuales.

**Cuándo usar:**
- Navegación jerárquica (drill-down)
- Pantallas de detalle donde el back es la acción principal
- Combinada con bottom nav para la jerarquía secundaria

**Sticky vs. no sticky:**
- **Sticky:** si la pantalla tiene acciones frecuentes arriba (búsqueda, filtros) o si el nombre de sección es importante para orientar al usuario
- **No sticky:** si el contenido necesita el espacio vertical; evaluar si el usuario pierde orientación sin el header

**Antipatrón frecuente:** header con más de 3 elementos de acción — divide la atención y satura el espacio premium de la pantalla.

---

## 3. Hamburger Menu (Drawer)

**Qué es:** ícono de 3 líneas que abre un panel lateral o modal con la navegación completa.

**Cuándo usar:**
- Más de 5 secciones de distinta frecuencia de uso
- Jerarquía de navegación profunda (más de 2 niveles)
- Secciones de acceso poco frecuente (configuración, perfil, ayuda)

**Cuándo evitar:**
- Si hay 3-4 secciones principales — usar bottom nav en su lugar
- Si las secciones son frecuentes — el hamburger aumenta el número de taps para navegar

**Regla de oro:** si se usa hamburger, agregar siempre el label "Menú" junto al ícono — mejora la discoverability, especialmente en usuarios nuevos.

---

## 4. Tabs Horizontales (Top Tabs / Segmented Control)

**Qué es:** fila de pestañas horizontales debajo del header, para alternar entre vistas relacionadas del mismo contexto.

**Cuándo usar:**
- 2-4 vistas del mismo contenido (ej: Por mes / Por semana / Por día)
- El usuario necesita comparar o alternar entre vistas frecuentemente
- Las vistas son paralelas, no jerárquicas

**Cuándo evitar:**
- Labels largos que se cortan o no caben en pantalla
- Más de 5 tabs (genera overflow o labels illegibles)
- Tabs que deberían ser secciones independientes (→ usar bottom nav)

---

## 5. Overflow Tabs (Scroll horizontal de tabs)

**Qué es:** fila de tabs que supera el ancho de pantalla y permite scroll horizontal.

**Cuándo usar:**
- 5+ categorías de igual relevancia donde el usuario elige su contexto
- Contenido tipo feed filtrado por categoría (ej: noticias, productos)

**Requisito:** indicador visual de overflow — fade o shadow en el borde derecho. Sin él, el usuario no descubre que hay más tabs fuera de la pantalla.

**Antipatrón frecuente:** tabs con scroll horizontal sin indicador visual → el usuario cree que las opciones visibles son las únicas.

---

## 6. Back Navigation (Navegación jerárquica)

**Qué es:** botón o gesto de retroceso para navegar hacia la pantalla anterior en un flujo jerárquico.

**Cuándo usar:**
- Flujos drill-down: lista → detalle → subnivel
- Flujos de tarea secuencial con posibilidad de retroceso

**Reglas clave:**
- El back siempre debe volver al mismo estado que tenía la pantalla anterior (scroll, filtros) — no al inicio de la pantalla
- En PWA, el back del browser y el back de la app deben comportarse igual — una fuente frecuente de fricción

---

## Árbol de decisión rápido

```
¿Cuántas secciones principales tiene la app?
│
├─ 1-2 → Tab bar o flujo lineal sin nav principal
├─ 3-5 → Bottom Navigation (tab bar fija)  
├─ 4-5 con pesos distintos → Bottom nav (frecuentes) + Hamburger (secundarias)
└─ 6+ → Hamburger / Drawer como nav principal

¿Las vistas son del mismo contexto o secciones diferentes?
│
├─ Mismo contexto, vista alternativa → Top Tabs
└─ Secciones independientes → Bottom Nav o Hamburger

¿El usuario navega principalmente hacia abajo (drill-down)?
└─ Sí → Top Navigation Bar + Back
```

---

## Combinaciones comunes en apps web mobile

| Patrón | Ejemplo de uso |
|--------|---------------|
| Bottom Nav + Top Bar | App de e-commerce: secciones (home, catálogo, carrito, perfil) + header contextual en cada sección |
| Top Bar + Tabs | Dashboard con vistas por período: Semana / Mes / Año |
| Bottom Nav + Hamburger | App compleja: acciones frecuentes abajo, secciones secundarias en drawer |
| Solo Top Bar | Flujo de checkout o wizard paso a paso |
