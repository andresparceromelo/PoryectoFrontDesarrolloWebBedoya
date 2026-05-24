# 🎟️ Mi Boleta

> **Práctica de Desarrollo Web** · Andres Bedoya Cano · 2026-1

Aplicación web full-stack para la gestión de boletas de juegos. Los usuarios pueden registrar, visualizar y administrar sus boletas; los administradores tienen acceso a un panel de control con vistas extendidas.

---

## 📑 Tabla de Contenidos

- [Vista General](#-vista-general)
- [Stack Tecnológico](#-stack-tecnológico)
- [Arquitectura del Proyecto](#-arquitectura-del-proyecto)
- [Requisitos Previos](#-requisitos-previos)
- [Instalación y Configuración](#-instalación-y-configuración)
- [Variables de Entorno](#-variables-de-entorno)
- [Scripts Disponibles](#-scripts-disponibles)
- [Rutas de la Aplicación](#-rutas-de-la-aplicación)
- [Modelo de Datos](#-modelo-de-datos)
- [API](#-api)
- [Roles y Permisos](#-roles-y-permisos)

---

## 🌐 Vista General

**Mi Boleta** es una SPA (Single Page Application) con dark mode premium que permite a los usuarios:

- Registrarse e iniciar sesión de forma segura
- Crear y gestionar boletas de juegos (loterías, chances, etc.)
- Visualizar un dashboard con resumen de sus boletas
- Filtrar boletas por estado, tipo de juego y fecha
- *(Rol admin)* Acceder al panel de administración con vista global

---

## 🛠️ Stack Tecnológico

### Frontend

| Tecnología | Versión | Uso |
|---|---|---|
| [React](https://react.dev/) | 19 | Framework UI |
| [Vite](https://vitejs.dev/) | 8 | Build tool y dev server |
| [TypeScript](https://www.typescriptlang.org/) | 6 | Tipado estricto |
| [Tailwind CSS](https://tailwindcss.com/) | 4 | Estilos utilitarios |
| [shadcn/ui](https://ui.shadcn.com/) | — | Componentes base accesibles |
| [TanStack Query](https://tanstack.com/query) | 5 | Fetching, cache y sincronización de datos |
| [Framer Motion](https://www.framer.com/motion/) | 12 | Animaciones declarativas |
| [React Router DOM](https://reactrouter.com/) | 7 | Enrutamiento cliente |
| [React Hook Form](https://react-hook-form.com/) | 7 | Gestión de formularios |
| [Zod](https://zod.dev/) | 4 | Validación de esquemas |
| [Sonner](https://sonner.emilkowal.ski/) | 2 | Notificaciones toast |
| [Lucide React](https://lucide.dev/) | — | Iconografía |

### Backend *(referencia — API externa)*

| Tecnología | Uso |
|---|---|
| Node.js + TypeScript | Runtime y lenguaje |
| Prisma ORM | Acceso a base de datos |
| Supabase (PostgreSQL) | Base de datos relacional |
| Clean Architecture | Organización del código (domain / application / infrastructure / interface) |

---

## 🏗️ Arquitectura del Proyecto

```
practica-frontend/
├── frontend/                   # SPA React + Vite
│   ├── src/
│   │   ├── components/
│   │   │   ├── ui/             # Componentes shadcn/ui
│   │   │   └── shared/         # Layout, navegación y guards globales
│   │   ├── features/           # Módulos por dominio
│   │   │   ├── auth/           # Login y Registro
│   │   │   ├── dashboard/      # Vista principal del usuario
│   │   │   ├── tickets/        # CRUD de boletas
│   │   │   └── admin/          # Panel de administración
│   │   ├── hooks/              # Hooks globales reutilizables
│   │   ├── services/           # Cliente API (fetch/axios) y funciones core
│   │   ├── types/              # Contratos TypeScript alineados al backend
│   │   └── lib/                # Utilidades, constantes y configuración
│   ├── .env.example
│   └── package.json
│
└── backend/                    # Referencia del backend (API externa)
    ├── src/                    # Clean Architecture
    │   ├── domain/             # Entidades y contratos
    │   ├── application/        # Casos de uso y DTOs
    │   ├── infrastructure/     # Repositorios, Prisma, DB
    │   └── interface/          # Controladores y rutas HTTP
    ├── prisma/                 # Schema y migraciones
    └── database/
        └── schema.sql          # Schema SQL para inicialización manual en Supabase
```

---

## ✅ Requisitos Previos

- **Node.js** ≥ 20
- **npm** ≥ 10
- Acceso a internet (la API corre en [Render](https://render.com/))

---

## 🚀 Instalación y Configuración

### 1. Clonar el repositorio

```bash
git clone https://github.com/andresparceromelo/PoryectoFrontDesarrolloWebBedoya.git
cd PoryectoFrontDesarrolloWebBedoya
```

### 2. Instalar dependencias del frontend

```bash
cd frontend
npm install
```

### 3. Configurar variables de entorno

```bash
cp .env.example .env
```

Editar `.env` con los valores correspondientes (ver sección [Variables de Entorno](#-variables-de-entorno)).

### 4. Iniciar servidor de desarrollo

```bash
npm run dev
```

La aplicación estará disponible en **http://localhost:5173**

---

## 🔐 Variables de Entorno

Crear el archivo `frontend/.env` a partir de `frontend/.env.example`:

```env
# URL base de la API REST
VITE_API_BASE_URL=https://mi-boleta-api-y9dv.onrender.com/api/v1
```

> **Nota:** Todas las variables del frontend deben comenzar con `VITE_` para que Vite las exponga al cliente.

---

## 📜 Scripts Disponibles

Ejecutar desde la carpeta `frontend/`:

| Comando | Descripción |
|---|---|
| `npm run dev` | Inicia el servidor de desarrollo con HMR |
| `npm run build` | Compila TypeScript y genera el bundle de producción en `dist/` |
| `npm run preview` | Sirve localmente el bundle de producción |
| `npm run lint` | Ejecuta ESLint sobre todo el código fuente |

---

## 🗺️ Rutas de la Aplicación

| Ruta | Acceso | Descripción |
|---|---|---|
| `/login` | Público | Inicio de sesión |
| `/register` | Público | Registro de nuevo usuario |
| `/` | 🔒 Autenticado | Dashboard con resumen de boletas |
| `/tickets` | 🔒 Autenticado | Listado y gestión de boletas |
| `/admin` | 🔒 Solo `admin` | Panel de administración |

Las rutas privadas están protegidas por `<PrivateRoute />` y el panel de admin por `<AdminRoute />`.

---

## 🗄️ Modelo de Datos

### `users`

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | `uuid` | Identificador único |
| `name` | `text` | Nombre completo |
| `email` | `text` | Email único |
| `password_hash` | `text` | Hash de la contraseña |
| `role` | `text` | `'user'` ó `'admin'` |
| `created_at` | `timestamptz` | Fecha de creación |

### `tickets`

| Campo | Tipo | Descripción |
|---|---|---|
| `id` | `uuid` | Identificador único |
| `user_id` | `uuid` | Referencia al usuario dueño |
| `title` | `text` | Título o nombre de la boleta |
| `game_type` | `text` | Tipo de juego (lotería, chance, etc.) |
| `game_number` | `text` | Número de la boleta |
| `game_date` | `timestamptz` | Fecha del sorteo |
| `amount` | `numeric(12,2)` | Valor apostado |
| `place` | `text` | Lugar de compra |
| `status` | `text` | Estado actual de la boleta |
| `notes` | `text` | Notas adicionales |
| `created_at` | `timestamptz` | Fecha de registro |
| `updated_at` | `timestamptz` | Última actualización |

---

## 🌍 API

La API REST está desplegada en:

```
https://mi-boleta-api-y9dv.onrender.com/api/v1
```

La colección completa de endpoints está disponible en [`backend/docs/api-mi-boleta.postman_collection.json`](./backend/docs/api-mi-boleta.postman_collection.json) (importar en Postman).

---

## 👥 Roles y Permisos

| Rol | Dashboard | Tickets | Admin |
|---|:---:|:---:|:---:|
| `user` | ✅ | ✅ | ❌ |
| `admin` | ✅ | ✅ | ✅ |

---

## 👨‍💻 Autor

**Andres Bedoya Cano** · Práctica Desarrollo Web 2026-1

---

<p align="center">Hecho con ❤️ usando React + Vite + TypeScript</p>
