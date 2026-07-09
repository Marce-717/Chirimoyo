# Proyecto FIA Chirimoyos
## Universidad de Chile 
### Autor: Marcelo Toro M. - Ing. Agrónomo
### Prefesional encargado: Nicolas Cortés - Ing. Agrónomo

---

## 📋 Descripción

Este repositorio contiene el pipeline **ETL (Extract · Transform · Load)** desarrollado para el Proyecto FIA de investigación en cultivo de chirimoyos en **Agrícola HC**, en el marco de una iniciativa de digitalización agrícola financiada por la Fundación para la Innovación Agraria (FIA).

El objetivo central es **centralizar, validar y estructurar** los datos climáticos generados por sensores en campo (Campo e Invernadero) en una base de datos relacional SQL Server, habilitando el análisis estadístico, la visualización temporal y el desarrollo futuro de modelos predictivos fenológicos.

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

---
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
        - BULK INSERT a MedicionesClimaticas
        - Validación adicional
        - Transformación a fact.*
        ↓
    BASE DE DATOS LISTA PARA ANÁLISIS

```

### Herramientas tecnologicas
Herramientas tcnologicas:
Mocrosotf SQL Server Manegment
Phyton: 
