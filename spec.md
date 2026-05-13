---
title: "Agenda básica CLI con Python"
version: "0.1"
date: "29/04/2026"
---
# spec.md - Especificación funcional #
## 1. Descripción general 
La aplicación es una herramienta de línea de comandos (CLI) escrita en lenguaje Python para gestionar una flota de furgonetas eléctricas. A diferencia de un desarrollo tradicional, utilizaremos SPEC-Driven Development (SDD).

## 2. Funcionalidades
- Crear, listar, actualizar y eliminar vehiculos.
- Crear, listar, actualizar y eliminar pedidos.
- Crear, listar, actualizar y eliminar rutas.
- Crear un menú de opciones.
- CLI sea interactiva.
- Almacene información en una base de datos Sqlite3.

## 3. Modelo de datos
### 3.1 Tabla vehiculo en base de datos Sqlite3
| Campo | Tipo | Obligatorio | Descripcion |
|-------|------|-------------|-------------|
| id_vehiculo | CHAR(7) | SI | Identificador único (ej: "VAN-001"). Debe seguir un patrón alfanumérico |
| modelo | VARCHAR(100) | SI | Nombre del modelo (ej: "Tesla Semi", "Renault Kangoo ZE") |
| capacidad_bateria_total | float | SI |  Capacidad máxima en kWh |
| nivel_bateria_actual | int/float | SI | Porcentaje de carga actual (0 a 100) |
| autonomia_maxima_km | int | SI | Cuántos km puede recorrer con el 100% de carga |
| estado | enum | SI |  Disponible, En Ruta, Cargando, Mantenimiento |

---
### 3.2 Tabla pedidos en base de datos Sqlite3
| Campo | Tipo | Obligatorio | Descripcion |
|-------|------|-------------|-------------|
| id_entrega | VARCHAR(100) | Identificador único del paquete |
| destino_coordenadas |   | Ubicación exacta de la entrega |
| peso_kg | float | Influye en el consumo (opcional para aumentar dificultad) |
| prioridad | int | Nivel de urgencia (1-3) |
| ventana_horaria | VARCHAR(20) | Ejemplo: "09:00 – 11:00" |

---
### 3.3 Tabla vehiculo en base de datos Sqlite3
| Campo | Tipo | Obligatorio | Descripcion |
|-------|------|-------------|-------------|
| id_ruta | VARCHAR(20) | ID de la jornada |
| vehiculo_asignado | | Lista ordenada de IDs de entregas |
| distancia_total_estimada | float | Suma de los trayectos en km |
| consumo_estimado_bateria | float | Porcentaje que se retrasará tra completar la ruta |

----

## 4. Casos de uso
### CU-01: 

   
### CU-02: 
### CU-03: 

---
## 5. Reglas de validación
| Campo | Regla|
|-------|------|


## 6. Requisitos no funcionales (opcional)

| ID    | Requisito                                                              |
|-------|------------------------------------------------------------------------|


---

## 7. Mensajes de error estándar (opcional)

| Código  | Mensaje                                              |
|---------|------------------------------------------------------|
| ERR-001 | ""                          |

