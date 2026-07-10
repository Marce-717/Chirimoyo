# Proyecto FIA Chirimoyos
## Universidad de Chile 
### Autor: Marcelo Toro M. - Ing. Agrónomo
### Prefesional encargado: Nicolas Cortés - Ing. Agrónomo

---

## 📋 Descripción

Este repositorio contiene el pipeline **ETL (Extract · Transform · Load)** desarrollado para el Proyecto FIA de investigación en cultivo de chirimoyos en **Agrícola HC**, en el marco de una iniciativa de digitalización agrícola financiada por la Fundación para la Innovación Agraria (FIA).

El objetivo central es **estructurar, centralizar y validar** los datos climáticos generados por sensores en campo (Campo e Invernadero) en una base de datos relacional SQL Server, habilitando el análisis estadístico, la visualización temporal y el desarrollo futuro de modelos predictivos fenológicos.

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

---

## 🛠️ Stack Tecnológico

### Python

| Librería | Uso |
|---|---|
| `pandas` | Lectura, limpieza y transformación de datos |
| `numpy` | Operaciones numéricas |
| `pyodbc` / `sqlalchemy` | Conexión a SQL Server Express |
| `matplotlib` | Visualización de series temporales |
| `jupyter` | Entorno interactivo de análisis |

### Base de datos

| Componente | Detalle |
|---|---|
| Motor | Microsoft SQL Server Express |
| Instancia local | `LAPTOP-PUELCHE\SQLEXPRESS` |
| Base de datos | `FIA` |
| Tabla principal | `MedicionesClimaticas` |
| Gestión | SQL Server Management Studio (SSMS) |

---

## 🚀 Cómo ejecutar el pipeline

### Prerrequisitos

```bash
pip install pandas numpy sqlalchemy pyodbc openpyxl matplotlib
```

> Se requiere tener instalado **SQL Server Express** con la instancia `LAPTOP-PUELCHE\SQLEXPRESS` activa y la base de datos `FIA` creada.

### Pasos

1. Clonar el repositorio
   ```bash
   git clone https://github.com/Marce-717/Chirimoyo.git
   cd Chirimoyo
   ```

2. Abrir `ETL_Agricola_HC.ipynb` en Jupyter Lab o Jupyter Notebook

3. Ejecutar las celdas en orden (7 celdas secuenciales):
   - Celda 1 → Importación de librerías y conexión a SQL Server
   - Celda 2 → Lectura y exploración del `.xlsx`
   - Celda 3 → Limpieza y validación de datos
   - Celda 4 → Transformación al formato SQL
   - Celda 5 → TRUNCATE de seguridad (opcional)
   - Celda 6 → Carga por lotes con rollback automático
   - Celda 7 → Consultas analíticas y visualizaciones

---

## 📊 Análisis incluidos

- ✅ Análisis Exploratorio de Datos (EDA)
- ✅ Series temporales de temperatura, humedad y DPV
- ✅ Acumulación de horas frío por temporada
- ✅ Estadísticos descriptivos por tratamiento (Campo vs. Invernadero)
- 🔄 Modelado predictivo fenológico *(en desarrollo)*

---

## 📌 Contexto del Proyecto

Este proyecto busca caracterizar las condiciones agroclimáticas que determinan la fenología del cultivo, aportando evidencia científica para la toma de decisiones productivas y la adaptación al cambio climático.

---

## 📄 Licencia

Este repositorio se desarrolla en el contexto de un proyecto de investigación financiado por **FIA** (Fundación para la Innovación Agraria). Documento para ser usado por Agricola HC.

---

*Universidad de Chile · Facultad de Ciencias Agronómicas*
