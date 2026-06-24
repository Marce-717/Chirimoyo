# Proyecto FIA Chirimoyos
## Universidad de Chile 
### Autor: Marcelo Toro

Diseñar y crear una base de datos para un sistema agricola de cultivo de chirimoya. Esta base de datos debe contener toda la información climática de recopilada de estaciones metereológicas en 2 condiciones distintas, cultivo al aire libre y bajo invernadero.

Las variables recolectadas provienen de datos recolectados por el software de riego CDTEC y cuyos datos son recolectados decargando los planillas directamente. 

### Herramientas tecnologicas
Herramientas tcnologicas:
Mocrosotf SQL Server Manegment
Phyton: 

## Flujo de Trabajo ETL {flujo-etl}

### ¿Qué es ETL?

**E**xtract → **T**ransform → **L**oad

```
SENSOR EN CAMPO (temperatura, humedad, etc.)
        ↓
    EXCEL/CSV (archivo con datos crudos)
        ↓
    [PYTHON] - EXTRACCIÓN
        - Leer .xlsx
        - Limpiar nombres de columnas
        - Validar tipos de datos
        ↓
    [PYTHON] - TRANSFORMACIÓN
        - Normalizar valores
        - Generar CSVs con formato chileno
        - Reportar anomalías
        ↓
    CSV CON FORMATO CHILENO
    (;  UTF-8 BOM, CRLF)
        ↓
    [SQL SERVER] - CARGA
        - BULK INSERT a stg.mediciones_raw
        - Validación adicional
        - Transformación a fact.*
        ↓
    BASE DE DATOS LISTA PARA ANÁLISIS

```
