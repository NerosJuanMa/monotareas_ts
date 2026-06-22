# MonoTareas

Monorepo educativo basado en **pnpm workspaces** que demuestra cómo reutilizar una misma aplicación web para múltiples plataformas:

* Backend REST con **Express + MariaDB**
* Cliente web con **HTML + CSS + TypeScript + esbuild**
* Aplicación Android mediante **Capacitor**
* Aplicación de escritorio mediante **Tauri**
* Infraestructura de desarrollo mediante **Podman/Docker**

---

# Estructura del proyecto

```text
monotareas-jm/
│
├── infra/
│   ├── backend/
│   ├── web/
│   └── compose.yml
│
├── packages/
│   ├── backend/
│   ├── web/
│   ├── capacitor-app/
│   └── tauri-app/
│
├── package.json
├── pnpm-workspace.yaml
├── pnpm-lock.yaml
└── tsconfig.base.json
```

---

# Requisitos

## Opción A (Recomendada): Ejecutar todo mediante contenedores

Solo necesitas:

* Git
* Podman Desktop (Windows/macOS)
* Podman + Podman Compose (Linux)

No es necesario instalar:

* Node.js
* pnpm
* TypeScript
* MariaDB

Todo se instala dentro de los contenedores.

---

## Opción B: Desarrollo local

Necesitas:

### Node.js

Versión recomendada:

```bash
Node.js >= 24
```

Comprobar versión:

```bash
node --version
```

### pnpm

Instalar:

```bash
npm install -g pnpm
```

Comprobar:

```bash
pnpm --version
```

### MariaDB

Si vas a ejecutar el backend fuera de Docker:

Instalar MariaDB Server 12.x o superior.

---

# Clonar el proyecto

```bash
git clone <URL_DEL_REPOSITORIO>
cd monotareas-jm
```

---

# Instalación de dependencias

Desde la raíz:

```bash
pnpm install
```

Esto instalará todas las dependencias de:

* backend
* web
* capacitor-app
* tauri-app

---

# Ejecución mediante Podman / Docker

## Windows

### 1. Instalar Podman Desktop

https://podman-desktop.io

Durante la instalación:

* Activar WSL2 si se solicita.
* Reiniciar el equipo si es necesario.

---

### 2. Iniciar la máquina Podman

Abrir Podman Desktop.

La primera vez:

```bash
podman machine init
podman machine start
```

---

### 3. Levantar el entorno completo

Desde la raíz del proyecto:

```bash
podman compose -f infra/compose.yml up -d --build
```

Si tu instalación utiliza el ejecutable clásico:

```bash
podman-compose -f infra/compose.yml up -d --build
```

---

## Linux

Instalar:

```bash
sudo apt install podman podman-compose
```

o equivalente según distribución.

Inicializar:

```bash
podman machine init
podman machine start
```

Levantar:

```bash
podman compose -f infra/compose.yml up -d --build
```

---

## macOS

Instalar:

```bash
brew install podman podman-compose
```

Iniciar:

```bash
podman machine init
podman machine start
```

Levantar:

```bash
podman compose -f infra/compose.yml up -d --build
```

---

# Servicios disponibles

Una vez arrancado el stack:

| Servicio   | URL                   |
| ---------- | --------------------- |
| Web        | http://localhost:8080 |
| API        | http://localhost:3000 |
| phpMyAdmin | http://localhost:8000 |
| MariaDB    | localhost:3306        |

---

# Credenciales de base de datos

Configuradas en `infra/compose.yml`.

## Base de datos

```text
tareas-db
```

## Usuario

```text
tareas-user
```

## Contraseña

```text
tareas-secret
```

## Root

```text
root
```

Contraseña:

```text
secret
```

---

# Parar el entorno

```bash
podman compose -f infra/compose.yml down
```

---

# Eliminar contenedores y base de datos

```bash
podman compose -f infra/compose.yml down -v
```

---

# Desarrollo local (sin contenedores)

## Instalar dependencias

```bash
pnpm install
```

---

# Backend

Ubicación:

```text
packages/backend
```

## Compilar

```bash
pnpm --filter @monotareas/backend build
```

Generará:

```text
packages/backend/dist
```

---

## Ejecutar en modo desarrollo

```bash
pnpm --filter @monotareas/backend dev
```

Utiliza:

```text
tsx watch
```

por lo que recompila automáticamente.

---

## Ejecutar compilado

```bash
pnpm --filter @monotareas/backend build
pnpm --filter @monotareas/backend start
```

---

# Web

Ubicación:

```text
packages/web
```

---

## Compilar

```bash
pnpm --filter @monotareas/web build
```

Salida:

```text
packages/web/dist
```

---

## Modo observación

```bash
pnpm --filter @monotareas/web dev
```

Reconstruye automáticamente cuando cambian los archivos.

---

# Comandos globales del monorepo

Backend desarrollo:

```bash
pnpm dev:backend
```

Backend build:

```bash
pnpm build:backend
```

Web build:

```bash
pnpm build:web
```

---

# Android (Capacitor)

Ubicación:

```text
packages/capacitor-app
```

---

## Requisitos

Instalar:

* Android Studio
* Android SDK
* Java JDK 17 o superior

Comprobar:

```bash
java --version
```

---

## Instalar dependencias

Desde la raíz:

```bash
pnpm install
```

---

## Generar versión web

```bash
pnpm --filter @monotareas/web build
```

---

## Sincronizar con Capacitor

```bash
pnpm --filter @monotareas/capacitor-app sync
```

---

## Crear proyecto Android (solo la primera vez)

```bash
npx cap add android
```

---

## Abrir Android Studio

```bash
pnpm --filter @monotareas/capacitor-app open:android
```

---

## Importante

Dentro del emulador Android:

```text
localhost != tu ordenador
```

Debes configurar la URL del backend para apuntar a:

```text
http://IP_DE_TU_PC:3000
```

en lugar de:

```text
http://localhost:3000
```

---

# Escritorio (Tauri)

Ubicación:

```text
packages/tauri-app
```

---

## Requisitos

### Rust

Instalar:

https://rustup.rs

Comprobar:

```bash
rustc --version
cargo --version
```

---

## Dependencias del sistema

### Windows

Instalar:

* Microsoft Visual Studio Build Tools

---

### Linux

Ejemplo Ubuntu:

```bash
sudo apt install \
build-essential \
pkg-config \
libgtk-3-dev \
webkit2gtk-4.1-dev
```

---

### macOS

Instalar Xcode Command Line Tools:

```bash
xcode-select --install
```

---

## Instalar dependencias

```bash
pnpm install
```

---

## Inicializar Tauri (solo la primera vez)

```bash
npx @tauri-apps/cli init
```

---

## Ejecutar en desarrollo

```bash
pnpm --filter @monotareas/tauri-app dev
```

---

## Generar ejecutable

```bash
pnpm --filter @monotareas/tauri-app build
```

Los binarios se generarán dentro de:

```text
src-tauri/target/
```

---

# Solución de problemas

## Error de pnpm

Instalar:

```bash
npm install -g pnpm
```

---

## Error de Node.js

Comprobar:

```bash
node --version
```

Debe ser:

```text
24 o superior
```

---

## Error de MariaDB

Verificar que el puerto:

```text
3306
```

no esté ocupado.

---

## Error de API inaccesible

Comprobar:

```bash
curl http://localhost:3000
```

---

## Error de Web inaccesible

Comprobar:

```bash
http://localhost:8080
```

---

# Tecnologías utilizadas

* TypeScript
* Express
* MariaDB
* pnpm Workspaces
* esbuild
* Capacitor
* Tauri
* Podman
* Docker Compose
* phpMyAdmin

---

# Flujo recomendado para alumnos

1. Clonar repositorio.
2. Ejecutar Podman.
3. Lanzar:

```bash
podman compose -f infra/compose.yml up -d --build
```

4. Abrir:

```text
http://localhost:8080
```

5. Acceder a phpMyAdmin:

```text
http://localhost:8000
```

6. Probar la API:

```text
http://localhost:3000
```

Con esto todo el entorno queda operativo sin instalar Node.js ni MariaDB en el equipo anfitrión.

# ##############################################
# Arquitectura Interna

## Diagrama de componentes

```text
┌─────────────────────┐
│     Navegador       │
│  Capacitor / Tauri  │
└──────────┬──────────┘
           │ HTTP
           ▼
┌─────────────────────┐
│   Frontend Web      │
│ TypeScript + ESBuild│
└──────────┬──────────┘
           │ REST API
           ▼
┌─────────────────────┐
│ Express Backend     │
│ Puerto 3000         │
└──────────┬──────────┘
           │ MariaDB Driver
           ▼
┌─────────────────────┐
│ MariaDB             │
│ Puerto 3306         │
└─────────────────────┘
```

---

# API REST

Base URL:

```text
http://localhost:3000
```

---

## Obtener todas las tareas

### Request

```http
GET /tareas
```

### Respuesta

```json
[
  {
    "id": 1,
    "titulo": "Comprar leche",
    "completada": false
  }
]
```

---

## Obtener una tarea

### Request

```http
GET /tareas/1
```

### Respuesta

```json
[
  {
    "id": 1,
    "titulo": "Comprar leche",
    "completada": false
  }
]
```

---

## Crear tarea

### Request

```http
POST /tareas
Content-Type: application/json
```

```json
{
  "titulo": "Nueva tarea"
}
```

### Respuesta

```json
[
  {
    "affectedRows": 1,
    "insertId": 10
  }
]
```

---

## Marcar tarea como completada

### Request

```http
PATCH /tareas/10
Content-Type: application/json
```

```json
{
  "completada": true
}
```

---

## Borrar tarea

### Request

```http
DELETE /tareas/10
```

---

## Restaurar datos iniciales

### Request

```http
POST /reset
```

---

# Variables de entorno utilizadas

Actualmente el backend utiliza las siguientes:

| Variable         | Descripción             |
| ---------------- | ----------------------- |
| DB_HOST          | Servidor MariaDB        |
| MARIADB_DATABASE | Nombre de base de datos |
| MARIADB_USER     | Usuario                 |
| MARIADB_PASSWORD | Contraseña              |
| NODE_ENV         | development/production  |

Configuración usada por Docker:

```env
DB_HOST=mariadb-service
MARIADB_DATABASE=tareas-db
MARIADB_USER=tareas-user
MARIADB_PASSWORD=tareas-secret
NODE_ENV=development
```

---

# Desarrollo Backend

## Compilación

```bash
pnpm --filter @monotareas/backend build
```

Genera:

```text
packages/backend/dist
```

---

## Desarrollo con recarga automática

```bash
pnpm --filter @monotareas/backend dev
```

Internamente ejecuta:

```bash
tsx watch src/server.ts
```

---

# Desarrollo Frontend

## Compilación

```bash
pnpm --filter @monotareas/web build
```

Internamente:

1. Ejecuta TypeScript.
2. Ejecuta ESBuild.
3. Copia recursos públicos.
4. Genera carpeta `dist`.

---

## Modo observación

```bash
pnpm --filter @monotareas/web dev
```

---

# Configuración para Capacitor

Por defecto el frontend usa:

```javascript
http://localhost:3000
```

Esto NO funciona dentro de Android.

Antes de cargar la aplicación debe definirse:

```javascript
window.__API_BASE__ = "http://192.168.1.100:3000";
```

donde la IP corresponde al ordenador que ejecuta el backend.

---

# Configuración para Tauri

Igualmente puede redefinirse:

```javascript
window.__API_BASE__ = "http://localhost:3000";
```

o cualquier URL remota.

---

# Base de Datos

## Tabla principal

```sql
CREATE TABLE tareas (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(255) NOT NULL,
    completada BOOLEAN DEFAULT FALSE
);
```

---

# Comprobaciones rápidas

## Backend

```bash
curl http://localhost:3000/tareas
```

Debe devolver JSON.

---

## Base de datos

```bash
mysql -h localhost -u tareas-user -p
```

Contraseña:

```text
tareas-secret
```

---

## phpMyAdmin

Abrir:

```text
http://localhost:8000
```

Servidor:

```text
mariadb-service
```

Usuario:

```text
tareas-user
```

Contraseña:

```text
tareas-secret
```

---

# Flujo completo para evaluación

```bash
git clone <repo>
cd monotareas-jm
pnpm install
podman compose -f infra/compose.yml up -d --build
```

Comprobar:

```text
http://localhost:8080
```

Crear una tarea.

Abrir:

```text
http://localhost:8000
```

Verificar que aparece en MariaDB.

Realizar:

```bash
curl http://localhost:3000/tareas
```

Verificar que la API devuelve la misma información.

Con estas tres comprobaciones queda validado el funcionamiento completo del sistema.
