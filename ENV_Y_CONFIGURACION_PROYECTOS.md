# Documentación Completa de Variables de Entorno y Accesos

Este archivo contiene la documentación centralizada de todas las variables de entorno (`.env`), configuraciones de despliegue (Neon + Render) y credenciales de acceso para los proyectos **MaraPlus** y **FarmaExpress**.

---

## 1. Repositorios de GitHub

- **FarmaExpress Repo**: `https://github.com/jesusenrique22/farmaexpreess.git` (Rama `main`)
- **MaraPlus Repo**: `https://github.com/jesusenrique22/maraapp` (Rama `main`)

---

## 2. Variables de Entorno del Backend (`backend-nestjs/.env`)

### Entorno Local (Docker PostgreSQL)
Ubicación del archivo: [`backend-nestjs/.env`](file:///Users/smart/mara-app/backend-nestjs/.env)

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
GEMINI_API_KEY=""
# Clave dedicada para el escáner visual de recetas médicas
GEMINI_SCAN_API_KEY=""
GEMINI_MODEL="gemini-2.0-flash"
GEMINI_SCAN_MODEL="gemini-2.0-flash-lite"

# Clave opcional de RapidAPI para precios reales de bebidas
RAPIDAPI_KEY=""
```

---

## 3. Base de Datos Cloud (Neon PostgreSQL)

Ubicación del archivo: [`backend-nestjs/.env.neon`](file:///Users/smart/mara-app/backend-nestjs/.env.neon)

```env
# Proyecto Neon DB: smart-medic
DATABASE_URL="postgresql://neondb_owner:npg_6otjSrwAy8hs@ep-small-fire-atder57k.c-9.us-east-1.aws.neon.tech/maraplus?sslmode=require"
DIRECT_URL="postgresql://neondb_owner:npg_6otjSrwAy8hs@ep-small-fire-atder57k.c-9.us-east-1.aws.neon.tech/maraplus?sslmode=require"

ADMIN_EMAIL="admin@farmaexpress.com"
ADMIN_PASSWORD="Admin123!"
```

---

## 4. Despliegue en la Nube (Render Blueprint)

Configuración en [`render.yaml`](file:///Users/smart/mara-app/render.yaml):

- **API NestJS (Web Service)**:
  - Runtime: Docker (`backend-nestjs/Dockerfile`)
  - Health check: `/health`
  - Variables requeridas en Render Dashboard: `DATABASE_URL`, `DIRECT_URL`, `ADMIN_PASSWORD`, `GEMINI_API_KEY`.
- **Frontend Flutter Web (Web Service)**:
  - Runtime: Docker (`deploy/Dockerfile.render-web` + Nginx)
  - Variable inyectada automáticamente: `API_BASE_URL` (enlazado a `maraplus-api`).

---

## 5. Tabla de Accesos y Usuarios de Prueba

| Rol | Correo Electrónico | Contraseña | Rutas / Acceso en la App | Descripción |
|---|---|---|---|---|
| **SUPER ADMIN** | `admin@farmaexpress.com`<br>*(o admin@maraplus.com)* | `Admin123!` | `/login/staff` o `/admin` | Panel de control total: productos, banners, sucursales, médicos, pacientes |
| **MÉDICO 1** | `doctor@maraplus.com` | `Doctor123!` | `/login/staff` | Dr. Juan Pérez (Cardiología y Med. General) |
| **MÉDICO 2** | `doctor2@maraplus.com` | `Doctor123!` | `/login/staff` | Dra. María González (Pediatría) |
| **MÉDICO 3** | `doctor3@maraplus.com` | `Doctor123!` | `/login/staff` | Dr. Roberto Silva (Dermatología) |
| **PACIENTE / CLIENTE** | `patient@maraplus.com` | `Patient123!` | `/login/store` o `/login/medic-plus` | Tienda, reservas de Pádel, MaraPuntos y Citas Médicas |

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
