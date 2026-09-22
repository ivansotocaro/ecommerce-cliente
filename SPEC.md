# SPEC.md — Alcance del MVP (v1)

> Este documento define QUÉ construye la v1 del ecommerce. Todo lo que no esté aquí explícitamente se considera fuera de alcance para el MVP. Si el agente (Antigravity) necesita tomar una decisión no cubierta aquí, debe preguntar antes de asumir.

---

## 1. Catálogo de productos

- Los productos se organizan por **categorías** (ej. estuches de instrumento, zapatillas).
- Cada producto tiene **atributos independientes** (Talla, Color, Tipo, etc. según la categoría) que se combinan en **variantes**. Ejemplo: Producto "Zapatilla X" → atributos Talla (38, 39, 40) y Color (negro, blanco) → variante concreta "Zapatilla X - Talla 39 - Negro", con su propio stock, precio (opcional, puede heredar del producto) y SKU.
- Cada variante tiene su propio **stock numérico**.
- **Ver el catálogo es público**: no requiere inicio de sesión.
- Filtros mínimos: por categoría y por atributo (ej. talla, color).

### Reglas de stock
- **Si una variante no tiene stock (stock = 0)**: se sigue mostrando en la ficha del producto, pero el botón de "Agregar al carrito" para esa variante específica queda **deshabilitado**, con una etiqueta visible tipo "Agotado".
- El stock se descuenta **solo cuando el pago es confirmado** (no al agregar al carrito, para no bloquear inventario con carritos abandonados).
- Si dos personas compran la última unidad casi al mismo tiempo, gana quien complete el pago primero; al segundo se le debe informar que el producto se agotó antes de cobrar (validación de stock justo antes de confirmar el pago).

---

## 2. Carrito de compras

- El carrito **no requiere sesión** para armarse (se puede navegar y agregar productos sin login).
- El carrito se guarda en el navegador (client-side) mientras no haya sesión iniciada.
- Al iniciar sesión o registrarse, el carrito debe conservarse (no perderse).
- Se puede modificar cantidad o quitar productos libremente desde el carrito.
- No hay límite de cantidad por producto en el MVP, salvo el stock disponible de la variante.

---

## 3. Cuentas de usuario (clientes)

- **Requisito**: iniciar sesión es **obligatorio solo para completar el checkout** (pagar). No es necesario para navegar ni para armar el carrito.
- **Datos del registro/checkout** (set completo, estándar de un ecommerce):
  - Nombre completo
  - Correo electrónico
  - Contraseña
  - Teléfono
  - Dirección de envío (calle, ciudad, departamento/zona, referencia adicional)
  - Documento de identidad (cédula/NIT) — se pide desde ahora aunque la facturación electrónica quede para v2, porque es más fácil pedirlo una vez que agregarlo después a cuentas ya creadas.
- Recuperación de contraseña por email: incluida en el MVP (es una función esperada por cualquier usuario y Strapi la trae de fábrica).
- No incluye en v1: login social (Google/Facebook), verificación de email obligatoria antes de comprar (puede agregarse después sin romper nada).

---

## 4. Checkout

- Flujo: Carrito → (si no hay sesión, login/registro) → Dirección de envío → Cálculo de envío → Selección de método de pago → Pago → Confirmación.
- El **total de la orden se recalcula siempre en el backend** antes de enviarlo a la pasarela de pago — nunca se confía en el total calculado en el navegador.
- Se genera una orden en estado `pendiente` antes de redirigir al pago, y cambia a `pagado` o `fallido` según la respuesta de la pasarela (vía webhook, no por respuesta directa del navegador).

### Envío
- El costo de envío se calcula **según ciudad/zona** del cliente (no es tarifa fija única).
- La tabla de zonas/tarifas es configurable (no hardcodeada en el código), para que la clienta pueda ajustarla sin depender del desarrollador. Se define su fuente exacta (tabla en Strapi vs. integración con transportadora) cuando la clienta entregue las zonas y tarifas — **pendiente de definir con ella**, pero el sistema debe construirse desde el inicio esperando este cálculo (no como parche posterior).
- Mientras no haya tarifas reales, se usa una tabla de zonas de ejemplo/placeholder fácilmente editable.

### Métodos de pago
- **v1 lanza con un solo método de pago**: tarjeta de crédito/débito vía Wompi.
- La integración debe diseñarse de forma que **agregar métodos adicionales (PSE, Nequi) en el futuro no requiera rehacer el flujo de checkout** — el método de pago es un módulo intercambiable, no lógica mezclada con el resto del checkout.
- Métodos de pago adicionales y facturación electrónica quedan explícitamente para v2 (ver sección 7).

---

## 5. Pedidos, cancelaciones y devoluciones

- Estados de una orden: `pendiente` → `pagado` → `en preparación` → `enviado` → `entregado`, con posibilidad de pasar a `cancelado` o `en devolución` desde varios puntos.
- **En el MVP**:
  - El cliente puede solicitar cancelación/devolución escribiendo a la clienta (fuera del sistema, por WhatsApp/email — no hay flujo de "solicitar devolución" en la cuenta del cliente en v1).
  - La **clienta**, desde el panel admin de Strapi, puede cambiar el estado del pedido a `cancelado` o `en devolución` / `reembolsado`.
  - El **reembolso del dinero se hace manualmente** por la clienta desde su panel de Wompi — el sistema no ejecuta el reembolso automáticamente en v1.
- El modelo de datos de la orden debe incluir estos estados desde el inicio, para que automatizar el reembolso en v2 no requiera cambiar la estructura de datos.

---

## 6. Panel de administración

- Es el panel nativo de **Strapi** (`/admin`), con su propio sistema de login — separado de las cuentas de clientes.
- En v1, un solo usuario administrador (la clienta). No se definen roles adicionales (empleados con permisos limitados) todavía, pero Strapi lo soporta si se necesita después.
- Desde el panel, la clienta puede:
  - Crear/editar/eliminar productos, categorías, atributos y variantes.
  - Ver y cambiar el estado de los pedidos.
  - Ver el stock por variante y actualizarlo.
- Fuera de alcance en v1: reportes/analítica avanzada de ventas (Strapi ya permite ver listados y filtros básicos, que es suficiente por ahora).

---

## 7. Explícitamente fuera de alcance del MVP (v2 o posterior)

- Métodos de pago adicionales a tarjeta (PSE, Nequi, contraentrega).
- Facturación electrónica (DIAN) — pendiente de confirmar si es obligatoria para el tipo de negocio/volumen de la clienta.
- Reembolsos automáticos vía API de la pasarela.
- Solicitud de devolución self-service desde la cuenta del cliente.
- Roles múltiples de administrador/empleados.
- Login social.
- Multi-idioma o multi-moneda.
- Cupones/descuentos.
- Reseñas de producto o wishlist.

---

## 8. Preguntas aún pendientes con la clienta (bloquean partes específicas, no todo el desarrollo)

1. Tabla real de zonas de envío y sus tarifas.
2. Si el negocio requiere facturación electrónica DIAN (afecta v2, no bloquea el MVP).
3. Cuenta comercial de Wompi ya creada o por crear (la clienta debe hacerlo, no el desarrollador).
