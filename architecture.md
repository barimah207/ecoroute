---
title: "Sistema de Gestión \"EcoRoute\" con Metodología SPEC"
version: "0.1"
date: "12/05/2026"
---
# architecture.md - Arquitectura técnica #

## 1. Visión general
Organización del código. Arquitectura limpia y persistencia (JSON o SQLite).
La aplicación sigue una **arquitectura en capas** (Layered Architecture) para separar la lógica de presentación de la persistencia de datos con tres niveles bien diferenciados. La comunicación entre capas es siempre descendente: la capa superior llama a la inferior; nunca al revés.

```
┌─────────────────────────────────────────────────────────┐
│                   CAPA DE PRESENTACIÓN                  │
│                   cli/  (cli_agent)                     │
│   main.py  ·  menu.py  ·  formatters.py                 │
└───────────────────────┬─────────────────────────────────┘
                        │  llama a
┌───────────────────────▼─────────────────────────────────┐
│                 CAPA DE LÓGICA DE NEGOCIO               │
│                logic/  (logic_agent)                    │
│ /models . services.py . validators.py . calculations.py |
|                                                         |
└───────────────────────┬─────────────────────────────────┘
                        │  llama a
┌───────────────────────▼─────────────────────────────────┐
│                   CAPA DE DATOS                         │
│                   db/  (db_agent)                       │
│         connection.py  ·  contact_repo.py               │
└───────────────────────┬─────────────────────────────────┘
                        │
                    ┌───▼───┐
                    │ MySQL │
                    └───────┘
```

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
│   ├── models/                 # entidades del dominio
│   │   ├── __init__.py
│   │   ├── vehicle.py
│   │   ├── delivery.py
│   │   └── route.py
│   │
│   ├── services.py            # Orquestación de casos de uso
│   ├── validators.py          # Validaciones de negocio
│   └── calculations.py        # Lógica de batería y consumo
|
├── db/
|   ├── __init__.py
│   ├── connection.py       # Clase encargada de la conexión (Singleton) y cierre de sesión.
│   └── contact_repo.py     # Repositorio de acceso a datos. Funciones específicas para sentencias SQL (INSERT, SELECT, UPDATE, DELETE).
|    
├── exceptions.py           # Excepciones personalizadas del proyecto 
|
└── tests/
    ├── test_vehicle.py
    ├── test_delivery.py
    └── test_route.py

```

## 3. Descripción de módulos
### 3.1 `main.py`
Punto de entrada. Inicializa la conexión a la base de datos, instancia los componentes de las capas y lanza el bucle principal del menu CLI.
### 3.2 `cli/menu.py`
Controla el flujo de navegación de la interfaz usuario.
**Funciones principales**
| Función                | Descripción                                          |
|------------------------|------------------------------------------------------|
| `run()`                | Bucle principal del menú                             |

### 3.3 cli/formatters.py
---
### 3.4 logic/models/vehicle.py
Define la flota de furgonetas electricas.
```python

@dataclass
class Vehicle:
    id_vehiculo: str
    modelo: str
    capacidad_bateria_total: float
    nivel_bateria_actual: int/floar
    autonomia_maxima_km: int
    estado: enum('Disponible','En ruta', 'Cargando', 'Mantenimiento')
```
---
### 3.5 logic/models/delivery.py
Representa un pedido o entrega individual
```python

@dataclass
class Delivery:
    id_entrega: str
    destino_coordenadas: tuple[float, float]
    peso_kg: Optional[float] = None
    prioridad: int
    ventana_horaria: str
```
---
### 3.5 logic/models/route.py
```python

@dataclass
class Route:
    id_ruta: str
    vehiculo_asignado: fk
    lista_entregas: list
    distancia_total_estimada: float
    consumo_estimado_bateria: float
```
---
### 3.6 logic/services.py
Orquesta los casos de uso.

###
**Métodos:**

| Método                              | Caso de uso      |
|-------------------------------------|------------------|
| `registrar_vehiculo()`              | CU-01            |
| `gestionar_entregas()`              | CU-02            |
| `logica_seguridad()`                | CU-03            |

---
### 3.7 logic/validators.py
Funciones de validación sin efectos secundarios. Devuelven `True` o lanzan `ValidationError` con el código de error correspondiente.

**Funciones:**

| Función                             | Regla aplicada                                 |
|-------------------------------------|------------------------------------------------|
| `validar_id_vehiculo(id_vehiculo)`  | No vacío,  Identificador único (ej: "VAN-001") |
| `validar_id_entrega(id_entrega)`    | Identificador único del paquete.               |

---
### 3.8 logic/calculations.py
Modulo responsable de los calculos del sistema, logica de bateria y consumo.

| Función                             | Regla aplicada                                 |
|-------------------------------------|------------------------------------------------|
| `validar_id_vehiculo(id_vehiculo)`  | No vacío,  Identificador único (ej: "VAN-001") |
| `validar_id_entrega(id_entrega)`    | Identificador único del paquete.               |

---
### 3.8 db/conecction.py
Gestiona la conexión a MySQL. Carga la configuración desde variables de entorno: DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASSWORD.

---
### 3.9 db/contact_repo.py
Repositorio de acceso a datos. Todas las consultas usan parámetros (%s).

**Métodos:**

| Método                       | SQL generado                            |
|------------------------------|-----------------------------------------|

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
