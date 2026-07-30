# Documentación Completa de Variables de Entorno y Accesos — FarmaExpress

Este archivo contiene la documentación centralizada de todas las variables de entorno (`.env`), configuraciones de despliegue (Neon + Render) y credenciales de acceso para los proyectos **FarmaExpress** y **MaraPlus**.

---

## 1. Repositorios de GitHub

- **FarmaExpress Repo**: `https://github.com/jesusenrique22/farmaexpreess.git` (Rama `main`)
- **MaraPlus Repo**: `https://github.com/jesusenrique22/maraapp` (Rama `main`)

---

## 2. Variables de Entorno del Backend (`backend-nestjs/.env`)

### Entorno Local (Docker PostgreSQL)
Ubicación del archivo: [`backend-nestjs/.env`](file:///Users/smart/Documents/GitHub/farmaexpreess/backend-nestjs/.env)

```env
# Conexión a Base de Datos PostgreSQL local en Docker (Puerto 5433)
DATABASE_URL="postgresql://maraplus:maraplus_dev@localhost:5433/maraplus?schema=public"
DIRECT_URL="postgresql://maraplus:maraplus_dev@localhost:5433/maraplus?schema=public"

# Puerto donde escucha la API NestJS
PORT=3010

# URL pública de la API
PUBLIC_API_URL="http://127.0.0.1:3010"

# Seguridad y Tokens JWT
JWT_SECRET="farmaexpress-dev-secret-cambiar-en-produccion"
JWT_EXPIRES_IN="7d"

# Credenciales por defecto del Super Administrador
ADMIN_EMAIL="admin@farmaexpress.com"
ADMIN_PASSWORD="Admin123!"

# Integración con Google Gemini AI (AI Studio)
# Clave principal para el chat / asistente
GEMINI_API_KEY="AQ.Ab8RN6KH3vgCB2GgjYFxOJ89hJ..."
# Clave dedicada para el escáner visual de recetas médicas
GEMINI_SCAN_API_KEY="AQ.Ab8RN6KH3vgCB2GgjYFxOJ89hJ..."
GEMINI_MODEL="gemini-2.0-flash"
GEMINI_SCAN_MODEL="gemini-2.0-flash-lite"

# Clave opcional de RapidAPI para precios reales de bebidas
RAPIDAPI_KEY=""
```

---

## 3. Base de Datos Cloud (Neon PostgreSQL - FarmaExpress)

Ubicación del archivo: [`backend-nestjs/.env.neon`](file:///Users/smart/Documents/GitHub/farmaexpreess/backend-nestjs/.env.neon)

```env
# Proyecto Neon DB: smart-medic (Base de Datos: farmaexpress)
# Pooled (App / Prisma Client):
DATABASE_URL="postgresql://neondb_owner:npg_6otjSrwAy8hs@ep-small-fire-atder57k-pooler.c-9.us-east-1.aws.neon.tech/farmaexpress?sslmode=require"

# Direct (Migraciones / Seed sin Connection Pooler):
DIRECT_URL="postgresql://neondb_owner:npg_6otjSrwAy8hs@ep-small-fire-atder57k.c-9.us-east-1.aws.neon.tech/farmaexpress?sslmode=require"

ADMIN_EMAIL="admin@farmaexpress.com"
ADMIN_PASSWORD="Admin123!"
```

---

## 4. Despliegue en la Nube (Render Blueprint)

Configuración en [`render.yaml`](file:///Users/smart/Documents/GitHub/farmaexpreess/render.yaml):

- **API NestJS (Web Service)**:
  - Service Name: `farmaexpress-api`
  - Runtime: Docker (`backend-nestjs/Dockerfile`)
  - Health check: `/health`
  - Variables requeridas en Render Dashboard: `DATABASE_URL`, `DIRECT_URL`, `ADMIN_PASSWORD`, `GEMINI_API_KEY`.
- **Frontend Flutter Web (Web Service)**:
  - Service Name: `farmaexpress`
  - Runtime: Docker (`deploy/Dockerfile.render-web` + Nginx)
  - Variable inyectada automáticamente: `API_BASE_URL` (enlazado a `farmaexpress-api`).

---

## 5. Tabla de Accesos y Usuarios de Prueba (FarmaExpress)

| Rol | Correo Electrónico | Contraseña | Rutas / Acceso en la App | Descripción |
|---|---|---|---|---|
| **SUPER ADMIN** | `admin@farmaexpress.com` | `Admin123!` | `/login/staff` o `/admin` | Administrador FarmaExpress (Panel de control total: productos, banners, sucursales, médicos, pacientes) |
| **MÉDICO 1** | `doctor@farmaexpress.com` | `Doctor123!` | `/login/staff` | Dr. Juan Pérez (Cardiología y Med. General) |
| **MÉDICO 2** | `doctor2@farmaexpress.com` | `Doctor123!` | `/login/staff` | Dra. María González (Pediatría) |
| **MÉDICO 3** | `doctor3@farmaexpress.com` | `Doctor123!` | `/login/staff` | Dr. Roberto Silva (Dermatología) |
| **PACIENTE / CLIENTE** | `patient@farmaexpress.com` | `Patient123!` | `/login/store` o `/login/medic-plus` | Carlos Mendoza (Cliente/Paciente: Tienda, reservas, puntos y citas) |

---

## 6. Comandos Utilitarios Rápidos

### Alternar a BD Cloud (Neon) en Local:
```bash
cp backend-nestjs/.env.neon backend-nestjs/.env
```

### Alternar a BD Local (Docker):
```bash
cp backend-nestjs/.env.example backend-nestjs/.env
```
