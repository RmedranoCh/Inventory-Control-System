# Sistema de Gestión de Inventario y Kárdex Contable

Una aplicación de escritorio para administrar inventarios, controlar ventas y llevar un kárdex contable automatizado. Está desarrollada en **Python** con **CustomTkinter** y usa **SQLite** como base de datos local. Funciona enteramente en una sola computadora, sin necesidad de conexión a internet ni servidores externos.

---

## ¿Qué hace este programa?

- **Control de inventario** — Podés dar de alta productos, actualizar cantidades, precios y stock mínimo. La tabla principal te muestra en tiempo real el estado de tu inventario.
- **Alertas visuales de stock bajo** — Cuando un producto llega a su límite mínimo, el sistema lo marca automáticamente en rojo para que lo veas de un vistazo.
- **Registro de ventas** — Seleccionás un producto, indicás cantidad y precio pactado, y el sistema descuenta del stock automáticamente. Cada venta queda registrada en una bitácora de operaciones.
- **Kárdex contable automatizado** — Cada movimiento (alta, modificación, venta) se registra con fecha, tipo y descripción. Esa información se puede exportar a Excel con fórmulas y sumatorias incluidas.
- **Exportación a Excel** — Con un clic generás un libro contable con dos hojas: una con el historial completo de movimientos y otra con el inventario disponible actual.
- **Seguridad básica** — Las contraseñas se guardan con SHA-256 (irreversible). Los detalles de los productos se cifran con Fernet (AES) usando una clave derivada del usuario del sistema.
- **Cambio de credenciales** — Podés cambiar el usuario y la contraseña desde la misma interfaz, siempre que ingreses la contraseña actual.

---

## Estructura del proyecto

```text
inventory-control-system/
│
├── src/
│   ├── main.py                  # Arranque de la aplicación, control de pantallas
│   ├── database/
│   │   ├── conexion.py          # Conexión a SQLite (la base se guarda en %AppData%)
│   │   └── tablas.py            # Creación de tablas y triggers SQL
│   ├── views/
│   │   ├── login_view.py        # Pantalla de inicio de sesión
│   │   ├── dashboard_view.py    # Menú lateral con las secciones principales
│   │   ├── inventario_view.py   # CRUD de productos y tabla de monitoreo
│   │   ├── ventas_view.py       # Registro de ventas e historial de movimientos
│   │   └── config_view.py       # Cambio de usuario y contraseña
│   └── utils/
│       ├── seguridad.py         # Hash SHA-256 y cifrado Fernet (AES)
│       └── excel_exporter.py    # Generación del libro contable en Excel
│
├── requirements.txt             # Librerías necesarias para ejecutar el proyecto
└── README.md                    # Este archivo
```

---

## Cómo instalarlo y ejecutarlo

### 1. Cloná el repositorio

```bash
git clone https://github.com/RmedranoCh/Inventory-Control-System.git
cd Inventory-Control-System
```

### 2. Instalá las dependencias

```bash
pip install -r requirements.txt
```

### 3. Ejecutá la aplicación

```bash
python src/main.py
```

---

### Credenciales por defecto

La primera vez que se ejecuta el programa, se crea automáticamente un usuario administrador con estos datos:

- **Usuario:** `admin`
- **Contraseña:** `admin123`

Podés cambiarlos después desde la sección **Seguridad / Claves** del menú lateral.

---

## Cómo compilarlo a un `.exe`

Si querés generar un ejecutable independiente para distribuir o usar sin instalar Python:

```bash
pyinstaller --clean --noconsole --onefile src/main.py
```

Esto crea un archivo `main.exe` dentro de la carpeta `dist/`. Podés renombrarlo como quieras (ej. `SistemaContable.exe`) y moverlo al Escritorio. Las carpetas `build/` y el archivo `main.spec` se pueden eliminar después de la compilación.

---

## Detalles técnicos

| Aspecto | Descripción |
|---|---|
| **Lenguaje** | Python 3 |
| **Interfaz gráfica** | CustomTkinter (tema oscuro/claro según el sistema) |
| **Base de datos** | SQLite, guardada en `%AppData%\.sistema_inventario_pro\secure_data.db` |
| **Hash de contraseñas** | SHA-256 |
| **Cifrado de datos sensibles** | Fernet (AES), clave derivada del nombre de usuario del SO |
| **Exportación** | openpyxl — genera un archivo .xlsx con dos hojas |
| **Triggers SQL** | 3 disparadores que registran automáticamente creación, modificación y venta de productos en el historial |

### Tablas de la base de datos

- **usuarios** — Almacena usuario y contraseña con hash.
- **productos** — Cada producto con nombre único, cantidad, precio de costo, stock mínimo y detalles cifrados.
- **ventas** — Registro de cada venta con producto, cantidad, precio, total, fecha y notas.
- **historial_movimientos** — Bitácora donde los triggers insertan automáticamente cada operación.

### Triggers

- `log_nuevo_producto` — Se activa al insertar un producto.
- `log_actualizacion_producto` — Se activa al modificar cantidad o precio; si el stock queda en o por debajo del mínimo, marca el evento como "BAJO STOCK".
- `log_nueva_venta` — Se activa al registrar una venta.

Tanto ventas como historial usan `ON DELETE CASCADE`, así que si eliminás un producto, sus registros asociados se borran automáticamente para mantener la consistencia.

---

---

# Inventory Management & Accounting Kardex System

A desktop application to manage inventory, control sales, and keep an automated accounting kardex. Built with **Python** using **CustomTkinter** and **SQLite** as the local database. It runs entirely on a single computer with no internet connection or external servers required.

---

## What this program does

- **Inventory control** — Add products, update quantities, prices, and minimum stock. The main table shows your inventory in real time.
- **Low stock visual alerts** — When a product reaches its minimum limit, the system automatically highlights it in red so you can spot it at a glance.
- **Sales registration** — Pick a product, enter the quantity and agreed price, and the system deducts from stock automatically. Every sale is recorded in an operations log.
- **Automated accounting kardex** — Every movement (creation, update, sale) is logged with date, type, and description. This data can be exported to Excel with built-in formulas and totals.
- **Excel export** — One click generates an accounting workbook with two sheets: a full movement history and the current available inventory.
- **Basic security** — Passwords are stored using SHA-256 (one-way hash). Product details are encrypted with Fernet (AES) using a key derived from the system username.
- **Credential change** — You can change the username and password from the interface itself, as long as you provide the current password.

---

## Project structure

```text
inventory-control-system/
│
├── src/
│   ├── main.py                  # Application entry point, screen control
│   ├── database/
│   │   ├── conexion.py          # SQLite connection (database stored in %AppData%)
│   │   └── tablas.py            # Table creation and SQL triggers
│   ├── views/
│   │   ├── login_view.py        # Login screen
│   │   ├── dashboard_view.py    # Sidebar menu with main sections
│   │   ├── inventario_view.py   # Product CRUD and monitoring table
│   │   ├── ventas_view.py       # Sales registration and movement history
│   │   └── config_view.py       # Username and password change
│   └── utils/
│       ├── seguridad.py         # SHA-256 hashing and Fernet (AES) encryption
│       └── excel_exporter.py    # Excel accounting workbook generation
│
├── requirements.txt             # Required libraries
└── README.md                    # This file
```

---

## Installation and execution

### 1. Clone the repository

```bash
git clone https://github.com/RmedranoCh/Inventory-Control-System.git
cd Inventory-Control-System
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the application

```bash
python src/main.py
```

---

### Default credentials

The first time the program runs, an admin user is created automatically with:

- **Username:** `admin`
- **Password:** `admin123`

You can change these later from the **Security / Keys** section in the sidebar menu.

---

## How to compile to a `.exe`

If you want a standalone executable to distribute or use without installing Python:

```bash
pyinstaller --clean --noconsole --onefile src/main.py
```

This creates a `main.exe` file inside the `dist/` folder. You can rename it (e.g., `SistemaContable.exe`) and move it to your Desktop. The `build/` folder and `main.spec` file can be deleted after compilation.

---

## Technical details

| Aspect | Description |
|---|---|
| **Language** | Python 3 |
| **GUI framework** | CustomTkinter (dark/light theme follows system settings) |
| **Database** | SQLite, stored at `%AppData%\.sistema_inventario_pro\secure_data.db` |
| **Password hashing** | SHA-256 |
| **Sensitive data encryption** | Fernet (AES), key derived from the OS username |
| **Export** | openpyxl — generates a .xlsx file with two sheets |
| **SQL triggers** | 3 triggers that automatically log creation, modification, and sale of products |

### Database tables

- **usuarios** — Stores username and hashed password.
- **productos** — Each product with a unique name, quantity, cost price, minimum stock, and encrypted details.
- **ventas** — Records each sale with product, quantity, price, total, date, and notes.
- **historial_movimientos** — Log where triggers automatically insert every operation.

### Triggers

- `log_nuevo_producto` — Fires on product insert.
- `log_actualizacion_producto` — Fires on quantity or price update; if stock drops to or below the minimum, it marks the event as "BAJO STOCK" (LOW STOCK).
- `log_nueva_venta` — Fires on sale registration.

Both sales and history use `ON DELETE CASCADE`, so when you delete a product, its related records are removed automatically to maintain consistency.
