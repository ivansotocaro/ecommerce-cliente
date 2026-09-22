# Ecommerce Premium — Proyecto Completo

> Ecommerce moderno y escalable para una marca premium colombiana. Stack: Strapi + Nuxt + MySQL + Cloudinary + Wompi.

---

## 📋 Documentación principal

Antes de tocar código, lee estos archivos en orden:

1. **`SPEC.md`** — Qué construye la v1 (alcance del MVP, reglas de negocio)
2. **`.agents/rules/project-context.md`** — Reglas técnicas, stack fijo, estructura de carpetas, librerías permitidas
3. **`.env.example`** — Variables de entorno que necesitas configurar

---

## 🛠️ Stack tecnológico

| Capa | Tecnología |
|---|---|
| Backend / Admin | **Strapi 5** (Node.js) |
| Frontend | **Nuxt.js** (Vue) |
| Base de datos | **MySQL** (todos los entornos) |
| Imágenes | **Cloudinary** (dev/produccion separados) |
| Pagos | **Wompi** (Colombia) |
| Animaciones | **GSAP** |
| Estado (frontend) | **Pinia** |
| Monitoreo | **Sentry** |

---

## 📁 Estructura del proyecto

```
ecommerce-cliente/
├── .agents/rules/
│   └── project-context.md         ← Reglas técnicas (se carga automáticamente)
├── backend/                       ← Strapi
│   ├── src/api/
│   │   ├── product/
│   │   ├── category/
│   │   ├── order/
│   │   ├── payment/               ← Integración con Wompi
│   │   └── shipping/              ← Cálculo de envío por zona
│   └── .env.example
├── frontend/                      ← Nuxt.js
│   ├── components/
│   ├── pages/
│   ├── stores/                    ← Pinia
│   └── .env.example
├── docs/
│   ├── SPEC.md                    ← Alcance del MVP
│   └── decisiones.md              ← Log de decisiones importantes
├── SPEC.md                        ← (Alcance: qué construye la v1)
├── .env.example                   ← Variables de entorno
└── README.md                      ← Este archivo
```

---

## 🚀 Fases de desarrollo

El proyecto se divide en 8 fases. Cada fase tiene un entregable que se puede probar sin depender de las siguientes:

1. **Fase 0** — Setup base: Strapi + Nuxt + MySQL corriendo localmente ✅
2. **Fase 1** — Catálogo público (productos, categorías, variantes)
3. **Fase 2** — Autenticación de clientes (login/registro)
4. **Fase 3** — Carrito de compras
5. **Fase 4** — Checkout y pagos (integración Wompi)
6. **Fase 5** — Diseño y animaciones premium (en paralelo con 1-4)
7. **Fase 6** — Panel de administración para la clienta
8. **Fase 7** — Optimización y QA
9. **Fase 8** — Lanzamiento a producción

---

## 🔧 Antes de empezar

### Requisitos instalados en tu computador:
- Node.js v22, v24 o v26 (versiones LTS/pares)
- MySQL 8.0+
- Git

### Cuentas/servicios a crear (gratis):
- **Cloudinary**: cloudinary.com (25GB gratis, imágenes)
- **Railway o Render**: para hosting (planes gratuitos disponibles)
- **Sentry**: sentry.io (monitoreo de errores)
- **Wompi**: wompi.co (la clienta crea la cuenta comercial)

### Archivos a completar antes de la Fase 0:
- Copia `.env.example` a `.env` en cada carpeta (`backend/` y `frontend/`)
- Completa las variables con tus credenciales reales

---

## 🎯 Reglas importantes

1. **Nunca confiar en precios/totales del frontend** — siempre se recalculan en el backend antes de cobrar.
2. **Confirmación de pago solo por webhook de Wompi**, no por respuesta del navegador.
3. **MySQL en todos los entornos** — desarrollo, staging y producción.
4. **Cloudinary desde el día uno** — imágenes nunca se pierden con deploys.
5. **Módulos aislados** — pagos, envío, imágenes: cada uno es reemplazable sin romper el resto.
6. No agregar librerías fuera del stack definido sin preguntar.

---

## 📖 Flujo de trabajo con el Agente

1. Lee los documentos (SPEC.md, project-context.md)
2. Abre Antigravity Agent
3. Dale un encargo de una sola fase (ej: "Haz la Fase 0")
4. El agente muestra el plan antes de ejecutar
5. Revisa el plan, aprueba si es correcto
6. El agente crea archivos/carpetas
7. Tú revisa el código en el IDE
8. Una vez validado, pasa a la siguiente fase

---

## ❓ Preguntas pendientes con la clienta

Estas necesitan respuesta para desbloquear ciertos módulos:

1. Tabla exacta de zonas de envío y tarifas por zona
2. ¿Facturación electrónica DIAN es obligatoria?
3. ¿Métodos de pago adicionales a tarjeta (PSE, Nequi)?

Mientras, el desarrollo avanza con datos de placeholder.

---

## 🆘 Soporte

Si algo falla o no está claro, los documentos son la fuente de la verdad:
- Reglas técnicas → `.agents/rules/project-context.md`
- Alcance del producto → `SPEC.md`
- Variables de entorno → `.env.example`
