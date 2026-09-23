# DESIGN.md — Sistema de Diseño ADORA Luxury Boutique

> Este documento describe el proyecto Stitch que es la fuente de verdad del diseño.
> El agente puede consultarlo directamente vía Stitch MCP — no es una copia manual.

---

## 📋 Información del Proyecto Stitch

- **Nombre**: ADORA Luxury Boutique E-Commerce
- **ID del Proyecto**: `12740064253152359896`
- **Tipo**: Desktop (prioridad principal)
- **Visibilidad**: Pública
- **Rol en proyecto**: Owner
- **Última actualización**: 23 de septiembre de 2026
- **URL**: https://stitch.withgoogle.com/projects/12740064253152359896

---

## 🎨 Sistema de Diseño: "Haute Showroom & Digital Atelier"

### Tipografía

| Uso | Familia | Características |
|---|---|---|
| Títulos / Branding | **Playfair Display** | Serif elegante, lujo, categoría premium |
| Cuerpo / UI | **Plus Jakarta Sans** | Sans-serif moderno, legible, contemporáneo |

### Paleta de Colores

El sistema completo está definido en Stitch. El agente puede consultar:
- Colores primarios (oro, negro, blanco)
- Colores secundarios (acentos)
- Estados (hover, active, disabled)
- Modo claro/oscuro (si aplica)

**Nota**: No listar códigos hex aquí — consultarlos directo del Stitch MCP cuando sea necesario.

---

## 📱 Pantallas Diseñadas

El proyecto Stitch contiene pantallas prediseñadas para:

1. **Catálogo/Showroom** — Listado de productos con filtros premium
2. **Ficha de Producto** — Detalles, variantes (talla, color), imágenes del producto
3. **Carrito** — Visualización de artículos, cantidades, totales
4. **Checkout** — Datos de envío, selección de método de pago, confirmación
5. **Dashboard Principal** — Panel administrativo para gestionar productos/pedidos

Cada pantalla tiene:
- Componentes reutilizables (botones, cards, inputs)
- Espaciados y grillas consistentes
- Estados visuales (hover, active, disabled, loading)

---

## 🔗 Cómo el Agente Usa Este Diseño

### Fase 5 (Diseño y Animaciones)

El agente recibe una instrucción como:

```
Lee el proyecto Stitch (ID: 12740064253152359896) vía MCP.
Extrae:
- Tokens de color (primario, secundario, neutros)
- Tipografía (Playfair Display para títulos, Plus Jakarta Sans para body)
- Espaciados
- Componentes base (botones, cards, inputs, etc.)

Construye todos los componentes de Nuxt usando exactamente 
esos tokens. Las animaciones deben ser premium con GSAP, 
complementando el elegance del design system.

Si encuentras pantallas no necesarias para el MVP (v1), 
quítalas del código SIN dañar el sistema de diseño base.
```

### Sin Exportar Manualmente

- ✅ No necesitas exportar código de Stitch
- ✅ No copias/pegas hex codes manualmente
- ✅ El agente consulta Stitch en tiempo real
- ✅ Cualquier cambio en Stitch se refleja automáticamente en el código

---

## 📝 Decisiones de Diseño

### Componentes a Priorizar en v1

- [ ] Botones (primario, secundario, outlined, disabled)
- [ ] Cards de producto
- [ ] Input de búsqueda/filtros
- [ ] Navegación (header, menú)
- [ ] Iconos (carrito, usuario, favoritos, etc.)
- [ ] Formularios (login, checkout)
- [ ] Estados visuales (loading, error, success)

### Componentes Opcionales (puede haber en Stitch pero no son críticos para MVP)

- Galerías avanzadas
- Sliders / carruseles complejos (si no son necesarios para el flujo de compra)
- Funcionalidades "nice to have" sin valor crítico

---

## 🎬 Animaciones Premium (Fase 5)

El sistema de animaciones debe:

1. **Estar documentado en un composable de Nuxt** (`useAnimations.ts` o similar)
2. **Usar GSAP para efectos complejos**: scroll reveals, parallax, stagger
3. **Usar transiciones nativas de Vue** para cambios de estado simples
4. **Respetar el "tempo" del diseño**: elegante, no frenético

Ejemplos:
- Hover suave en cards de producto (escala + sombra)
- Transición del carrito (slide in/out)
- Reveal de elementos en scroll (fade + slide)
- Feedback visual en botones (ripple, pulse, etc.)

---

## ✅ Checklist: Agente Antes de Codear

Cuando el agente reciba la Fase 5, debe verificar:

- [ ] ¿Leí el Stitch project vía MCP?
- [ ] ¿Extraje correctamente los tokens de color?
- [ ] ¿Identifiqué las tipografías (Playfair Display vs Plus Jakarta Sans)?
- [ ] ¿Creé un archivo de tokens en Nuxt** (`theme/tokens.ts` o `tokens/colors.ts`)?
- [ ] ¿Cada componente usa esos tokens, no valores hardcodeados?
- [ ] ¿Las animaciones usan GSAP solo si es complejo, Vue para transiciones simples?

---

## 📞 Referencia Rápida

Si el agente necesita algo del diseño:

- **Colores**: "Consulta el Stitch MCP, pantalla X, componente Y"
- **Tipografía**: "Playfair Display para títulos (h1-h3), Plus Jakarta Sans para body"
- **Espaciados**: "Múltiplos de 4px o 8px, según Stitch"
- **Componentes faltantes**: "Si no está en Stitch, créalo siguiendo el sistema, no inventes"
