# Proyecto FIA Chirimoyos
## Universidad de Chile 
### Autor: Marcelo Toro M. - Ing. Agrónomo
### Prefesional encargado: Nicolas Cortés - Ing. Agrónomo

---

## 📋 Descripción

Este repositorio contiene el pipeline **ETL (Extract · Transform · Load)** desarrollado para el Proyecto FIA de investigación en cultivo de chirimoyos en **Agrícola HC**, en el marco de una iniciativa de digitalización agrícola financiada por la Fundación para la Innovación Agraria (FIA).

El objetivo central es **centralizar, validar y estructurar** los datos climáticos y de riego generados por sensores en campo (Campo e Invernadero) en una base de datos relacional SQL Server, habilitando el análisis estadístico, la visualización temporal y el desarrollo futuro de modelos predictivos fenológicos.

---

## 🗂️ Estructura del Repositorio

```
Chirimoyo/
│
├── 📓 ETL_Agricola_HC.ipynb           ← Pipeline ETL ejecutable (7 celdas)
├── 📓 Informe_ETL_AgricolaHC_FIA.ipynb ← Informe técnico consolidado (v2.0)
│
├── 📊 Dep_FIA_climas_riegos_sondas.xlsx ← Datos fuente de sensores
│                                          1.860 registros · 15 variables
│                                          Período: 2023-11-11 → 2026-05-28
│
└── 📄 README.md
```
---
## ⚙️ Pipeline ETL · Resumen

```
SENSORES EN CAMPO
      ↓
EXCEL (.xlsx)  ←  Dep FIA climas riegos sondas.xlsx
      ↓               1.860 filas × 15 columnas
                       
   ── FASE E · EXTRACCIÓN ──────────────────────────
      pd.read_excel() + renombrado de columnas
      
   ── FASE V · VALIDACIÓN ──────────────────────────
      8 reglas de negocio agronómicas
      
   ── FASE T · TRANSFORMACIÓN ──────────────────────
      Estandarización · flags de auditoría · NaN→NULL
      
   ── FASE L · CARGA ───────────────────────────────
      INSERT por lotes (200 filas) · rollback por lote
      
BASE DE DATOS SQL SERVER
      ↓
  CH_UCHILE · MedicionesClimaticas
  1.860 registros · 100% de coincidencia verificada
```
---

### 8 Reglas de Validación Implementadas

| # | Regla | Variables | Criterio |
|---|-------|-----------|----------|
| R1 | Nulos obligatorios | `fecha`, `tratamiento`, `temp_*`, `hr_*` | NOT NULL |
| R2 | Duplicados | `fecha + tratamiento` | Clave candidata única |
| R3 | Coherencia temperatura | `min ≤ media ≤ max` | Consistencia física |
| R4 | Coherencia HR | `min ≤ media ≤ max` | Consistencia física |
| R5 | Rango temperatura | `−5°C ≤ T ≤ 45°C` | Rango fisiológico chirimoya |
| R6 | Rango humedad relativa | `0% ≤ HR ≤ 100%` | Límites del sensor |
| R7 | Precipitaciones | `precip ≥ 0 mm` | Magnitud física |
| R8 | Radiación / ILD | `Rad ≥ 0, ILD ≥ 0` | Energía solar no negativa |

---

## 🗄️ Modelo de Base de Datos

### Tabla `MedicionesClimaticas` · Fact Table

| Campo | Tipo SQL | Nullable | Unidad |
|-------|----------|----------|--------|
| `id` | INT IDENTITY (PK) | ✗ | — |
| `fecha` | DATE | ✗ | YYYY-MM-DD |
| `tratamiento` | NVARCHAR(50) | ✗ | Campo / Invernadero |
| `temp_min` / `temp_media` / `temp_max` | FLOAT | ✓ | °C |
| `hr_min` / `hr_media` / `hr_max` | FLOAT | ✓ | % |
| `precipitaciones_mm` | FLOAT | ✓ | mm |
| `piranometro_rg` | FLOAT | ✓ | W/m² |
| `rad_diaria_mj` | FLOAT | ✓ | MJ/m²/día |
| `ild_mol` | FLOAT | ✓ | mol/m²/día |
| `dpv_kpa` | FLOAT | ✓ | kPa |
| `horas_frio` / `hf_acumuladas` | FLOAT | ✓ | h |
| `flag_revision` | BIT | ✗ | 0=OK · 1=revisar |
| `fecha_carga` | DATETIME | ✗ | Timestamp ingesta |

### Tabla `Catalogo` · Dimension Table

Almacena los parámetros del sistema de riego por tratamiento: superficie, densidad de plantas, caudal por gotero, presión de operación y número de emisores por hectárea.

**Relación:** `Catalogo.tratamiento (1) → MedicionesClimaticas.tratamiento (N)`
---
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
