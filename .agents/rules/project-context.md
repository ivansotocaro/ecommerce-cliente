# Reglas del proyecto — Ecommerce Premium

> Va en `.agents/rules/project-context.md`. Se carga siempre, en cada conversación con el agente. Contiene lo que NUNCA debe cambiar sin discutirlo primero.

---

## Stack tecnológico (fijo)

| Capa | Tecnología | Notas |
|---|---|---|
| Backend / Admin | **Strapi 5** (≥5.37.0) | Node.js. Nunca usar una versión anterior a 5.37.0 (vulnerabilidad de seguridad conocida, CVE-2026-27886). |
| Frontend | **Nuxt.js (Vue)** | Todo el diseño y animaciones viven aquí. |
| Base de datos | **MySQL** | Se usa en TODOS los entornos, incluido desarrollo local — no usar SQLite ni siquiera para pruebas rápidas, para evitar diferencias de comportamiento entre entornos. |
| Imágenes | **Cloudinary**, desde el día uno | No usar almacenamiento local ni en desarrollo ni en producción — evita migraciones posteriores y el problema de archivos que se borran en cada deploy. Las imágenes en Cloudinary NO se borran con los deploys de código. Usar carpetas separadas por entorno dentro de Cloudinary (`CLOUDINARY_FOLDER=dev` vs `CLOUDINARY_FOLDER=produccion`) para que las pruebas nunca se mezclen con el catálogo real. |
| Pagos | **Wompi** (Colombia) | Módulo aislado en `backend/src/api/payment/`. Cuenta comercial la crea la clienta, no el desarrollador. |
| Envío | Cálculo por zona/ciudad | Módulo aislado en `backend/src/api/shipping/`. Tabla de tarifas configurable, no hardcodeada. |
| Node.js | v22, v24 o v26 (LTS/par) | Nunca instalar versiones impares (23, 25) — no son compatibles con Strapi. |
| Deploy | Railway o Render | Ambos manejan Node + MySQL administrados. |
| Monitoreo de errores | Sentry (backend y frontend) | Activar desde la primera fase de deploy en staging, no solo al final. |

---

## Estructura de carpetas (no modificar sin razón fuerte)

```
ecommerce-cliente/
├── .agents/rules/project-context.md
├── backend/            (Strapi)
│   ├── config/
│   ├── src/api/{product, category, order, payment, shipping}/
│   └── src/extensions/users-permissions/
├── frontend/           (Nuxt)
│   ├── components/{product, cart, checkout, auth, ui}/
│   ├── composables/
│   ├── pages/
│   └── stores/
├── docs/{SPEC.md, decisiones.md}
└── README.md
```

Cada módulo dentro de `api/` y cada carpeta de `components/` debe ser autocontenida: su propia lógica, sin depender de forma frágil de otro módulo. Si `payment/` falla, no debe romper `product/`.

## Reglas de arquitectura

1. **Nunca confiar en precios/totales enviados desde el frontend.** Se recalculan siempre en el backend antes de cualquier cobro.
2. La confirmación de pago **solo** se acepta vía webhook verificado de Wompi.
3. Módulos de pago, envío y almacenamiento de imágenes deben quedar aislados y ser reemplazables sin tocar el resto del sistema.
4. No agregar librerías fuera de las ya acordadas (ver tabla de stack + sección de librerías) sin preguntar primero. Preferir siempre paquetes oficiales o ampliamente mantenidos, nunca paquetes con pocas descargas o sin actualizaciones recientes.
5. No crear funcionalidades fuera de lo definido en `docs/SPEC.md`. Si algo no está definido, preguntar antes de asumir — especialmente en dinero, pagos, stock o datos personales.
6. Todas las credenciales van en variables de entorno (`.env`), nunca hardcodeadas. Mantener `.env.example` actualizado en `backend/` y `frontend/` por separado.
7. El modelo de "Orden" incluye desde el inicio los estados: `pendiente`, `pagado`, `en preparación`, `enviado`, `entregado`, `cancelado`, `en devolución`, `reembolsado`.
8. En v1, el reembolso del dinero es manual (la clienta lo hace desde su panel de Wompi); el sistema solo registra el cambio de estado.

## Librerías permitidas (ampliar solo con aprobación)

- Backend: `@strapi/provider-upload-cloudinary`, `@sentry/node`. Para llamadas HTTP a Wompi, usar `fetch` nativo de Node, sin librerías adicionales.
- Frontend: `pinia` / `@pinia/nuxt`, `gsap`, `@sentry/nuxt`, opcionalmente `@nuxtjs/tailwindcss`.

## Entornos

- **Desarrollo local**: Node LTS instalado, Strapi + Nuxt corriendo en `localhost`, conectados a **MySQL** (local o instancia en la nube, nunca SQLite), a Cloudinary real (no simulado) y a Wompi en modo sandbox.
- **Staging**: desplegado en Railway/Render con URL temporal gratuita, mismas integraciones que producción pero con credenciales de prueba (Wompi sandbox).
- **Producción**: dominio propio (se agrega cuando esté listo, no bloquea el desarrollo), credenciales reales de Wompi, Sentry activo, backups de base de datos MySQL activados.

## Reglas de trabajo con el agente

- Trabajar por fases según `docs/SPEC.md`. No avanzar de fase sin validar la anterior funcionando.
- Antes de instalar dependencias nuevas, crear modelos de datos, o tocar lógica de pagos/envío, mostrar el plan primero y esperar aprobación.
- Pedir explícitamente casos de prueba (qué pasa si el pago falla, si el stock llega a cero durante el pago, si alguien intenta comprar sin sesión) al cerrar cada fase — no asumir que "funciona" sin probarlo.
- Cualquier decisión de negocio nueva se agrega primero aquí antes de implementarla.
