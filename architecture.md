---
title: "Sistema de Gestión \"EcoRoute\" con Metodología SPEC"
version: "0.1"
date: "12/05/2026"
---
# architecture.md - Arquitectura técnica #

## 1. Visión general
Organización del código. Arquitectura limpia y persistencia (JSON o SQLite).

## 2. Estructura de directorios

```
ecoroute/
|
├── main.py                 # Punto de entrada de la aplicación
|
├── cli/                    # Capa de presentación
|   ├── __init__.py
|   ├── menu.py             # Menú principal y submenús
|   └── formatters.py       # Formateo de tablas y mensajes.
|
├── logic/
|   ├── __init__.py
│   ├── models.py           # Definición de la entidad `Vehiculo`.
|   ├── services.py         # Orquesta los casos de uso. Es el único punto de contacto entre CLI y repositorio.
│   └── validators.py       # Validación de campos
|
├── db/
|   ├── __init__.py
│   ├── connection.py       # Clase encargada de la conexión (Singleton) y cierre de sesión.
│   └── contact_repo.py     # Repositorio de acceso a datos. Funciones específicas para sentencias SQL (INSERT, SELECT, UPDATE, DELETE).
|    
├── exceptions.py           # Excepciones personalizadas del proyecto 
|
└── tests/
    ├── __init__.py
    └── test_validators.py

```

## 3. Descripción de módulos
### 3.1 `main.py`
Punto de entrada. Inicializa la conexión a la base de datos, instancia los componentes de las capas y lanza el bucle principal del menu CLI.
### 3.2 `cli/menu.py`
Controla el flujo de navegación de la interfaz usuario.
**Funciones principales**
| Función        | Descripción                |
|----------------|----------------------------|
|
---

## 4. Dependencias externas 
| Paquete                    | Versión  | Uso                            |
|----------------------------|----------|--------------------------------|
| `python-dotenv`            | >=1.0.0  | Configuración                  |
| `pytest`                   | >=8.0.0  | Framework de testing           |
| `pytest-cov`               | >=5.0.0  | Cobertura de tests             |
| `Geopy`                    | >=2.4.1  | Cálculos geográficos           |
| `rich`                     |          | Interfaz de usuario            |
| `pydantic`                 | >=2.11   | Validación de datos            |

---
