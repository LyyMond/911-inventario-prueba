# 🎫 SisTicket — Sistema Institucional de Gestión de Tickets e Inventario

Sistema web institucional construido con **CodeIgniter 4** + **MySQL / MariaDB**. Desarrollado para mantener la trazabilidad de los bienes de una institución a través de un esquema avanzado de mesa de ayuda (tickets) e inventario unitario.

---

## ✨ Características Principales

1. **Gestión de Roles**: Administrador, Técnico y Operador (solo ve sus propios tickets).
2. **Dashboard Dinámico y Timeline Histórico**: Sistema de auditoría integrado que muestra una bitácora detallada (Log de Actividad) de todos los movimientos de tickets, estados y personal interviniente.
3. **Clonación Inteligente de Inventario**: Permite registrar múltiples equipos iguales en la base de datos (Ej: 10 Computadoras) generando códigos unitarios autoincrementables automáticamente, manteniendo su independencia en áreas físicas.
4. **Diseño Visual**: Auténtico diseño Glassmorphism, completamente responsive y en Modo Oscuro.

---

## 📋 Requisitos Previos

| Componente | Versión mínima recomendada |
|---|---|
| PHP | 8.1+ (con extensiones `intl`, `mbstring`, `mysqli`) |
| Base de Datos | MySQL 8.0 / MariaDB 10.6+ |
| Composer | 2.x |

---

## 🚀 Instalación en 4 Pasos

Instalar **SisTicket** es muy sencillo ya que es un proyecto estándar de CodeIgniter 4 completo.

### 1. Clonar el repositorio y descargar dependencias

```bash
git clone https://github.com/TU-USUARIO/ticket-system.git
cd ticket-system
composer install
```

### 2. Configurar el Entorno

En la raíz del proyecto encontrarás un archivo llamado `env`. Renómbralo a `.env`:

```bash
# En Windows (CMD)
copy env .env

# En Linux/GitBash
cp env .env
```

Abre el nuevo archivo `.env` y asegúrate de configurar las credenciales de tu servidor de base de datos local:

```env
CI_ENVIRONMENT = development

database.default.hostname = localhost
database.default.database = ticket_system
database.default.username = root
database.default.password =      # (Tu contraseña de MySQL)
database.default.DBDriver = MySQLi
```

### 3. Base de Datos

1. Abre tu gestor de base de datos (Ej: phpMyAdmin o DBeaver).
2. Crea una base de datos vacía llamada `ticket_system` (o la que configuraste en `.env`).
3. Importa el archivo `ticket_system.sql` incluido en este repositorio. Esto construirá todas las tablas e inyectará los equipos de prueba y configuraciones iniciales.

### 4. Levantando el Servidor

Puedes correr la aplicación de dos maneras:

**A. Vía Servidor Integrado de PHP (Desarrollo rápido):**
En la raíz del proyecto, ejecuta:
```bash
php spark serve
```
La aplicación estará disponible en `http://localhost:8080`

**B. Vía XAMPP / Laragon / WAMP:**
Simplemente ubica la carpeta en tu directorio web (`htdocs` o `www`) y entra a:
`http://localhost/ticket-system/public`

---

## 🔐 Primeros Pasos y Autenticación

El sistema divide la forma de auto-registro por seguridad. 

Para poder crear el **Primer Administrador del Sistema**, debes dirigirte a la sección de registro interno (dentro del Login en la pestaña "Administrador"). Por seguridad, el registro del administrador *requiere* un código de autorización (Admin Code) especial, el cual por defecto es:

> **Admin Code de Instalación:** `0000`

Una vez crees el primer administrador usando el código `0000`, podrás iniciar sesión y visualizar el panel completo. 
*(El código de seguridad puede modificarse a posteriori en el `AuthController.php`)*.

---

## 🔗 Estructura de Rutas y Menú

| URL | Descripción / Funcionalidad |
|---|---|
| `/login` | Pasarela de autenticación (Operadores y Admins). |
| `/dashboard` | Panel principal (Estadísticas y Línea de Tiempo de Actividad). |
| `/tickets` | Listado completo de tickets. |
| `/tickets/historial` | Registro completo de bitácora y auditoría del sistema de tickets. |
| `/inventario` | Registro unitario de equipos (Oculto para operadores). |
| `/usuarios` | Gestión activa de integrantes (Oculto para operadores). |

---

## 🗃️ Estructura de Tabla del Inventario y Auto-Clonación

El sistema utiliza identificadores institucionales estrictos como código de bien (Ej: `3-171-14-46`). Al crear un inventario con `Cantidad > 1`, el sistema iterará internamente desde ese último sufljo numérico insertando entidades independientes una por una. **A nivel de base de datos la cantidad siempre será 1**, promoviendo un inventario atómico e imparcial donde cada bien pueda ser auditado, dañado o reparado de forma individual.
