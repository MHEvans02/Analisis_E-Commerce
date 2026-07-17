# Análisis E-Commerce UK Retailer

**Portfolio Data Analyst** · Análisis completo del ciclo de datos de un retailer UK de artículos de decoración del hogar.

---

## Descripción del proyecto

Pipeline de datos de punta a punta sobre un dataset de **541.909 transacciones** de un retailer online del Reino Unido. El proyecto abarca desde la exploración del dato crudo hasta un dashboard interactivo, pasando por limpieza, modelado relacional y análisis de hipótesis de negocio.

**Dataset:** [Kaggle — E-Commerce Data (carrie1)](https://www.kaggle.com/datasets/carrie1/ecommerce-data)  
**Período:** Diciembre 2010 — Diciembre 2011

---

## Resultados clave

| Métrica | Valor |
|---------|-------|
| Revenue neto total | £9.770.920 |
| Facturas únicas | 23.960 |
| Clientes identificados | 4.334 |
| Productos activos | 3.791 |
| Países activos | 38 |
| Tasa de cancelación media | 14.53% (CV 10.8% — estable) |
| Top 20% clientes → revenue | 74.6% (Pareto confirmado) |
| Franja horaria 10–15h | 74.4% del revenue diario |
| Noviembre 2011 | 1.8× el promedio mensual |
| Envíos / revenue ventas | 2.73% |

---

## Hallazgos principales

- **El dataset mezcla 3 fuentes distintas** — ventas, ajustes de inventario, cargos de envío y conceptos contables conviven en un solo archivo. Analizarlos sin separar produce métricas incorrectas.
- **El segmento Guest gasta ~2× más** por factura que el cliente identificado (£1.101 vs £475 → 2.3×), y **3.0× más que el retail** (£366) — resultado contra-intuitivo que sugiere mayoristas no registrados.
- **6 países operan como mayoristas** con tickets 3.3–7.0× superiores al baseline UK (Netherlands 7.0×, Australia 5.6×, Singapore 5.2×, Japan 4.5×, Lebanon 3.9×, Israel 3.3×).
- **Noviembre (pico de ventas) tiene la tasa de cancelación más baja** del período (12.2%) — los clientes de temporada alta son más decididos.
- **Correlación frecuencia/ticket = −0.005** — existen dos arquetipos de cliente completamente independientes.

---

## Stack tecnológico

| Categoría | Tecnología |
|-----------|-----------|
| Lenguaje | Python 3.11+ |
| Manipulación de datos | pandas · numpy |
| Visualización estática | matplotlib · seaborn |
| Dashboard interactivo | Plotly · Dash |
| Validación SQL | SQLite (sqlite3) |
| Exportación | openpyxl |
| Modelo relacional | DBML · dbdiagram.io |

---

## Estructura del repositorio

```
ecommerce-uk/
│
├── notebooks/
│   ├── 01_eda.ipynb                    # Análisis exploratorio (EDA)
│   ├── 02_analisis_estructural.ipynb   # Separación en 9 tablas
│   ├── 03_etl.ipynb                    # Pipeline ETL completo
│   ├── 04_modelado_relacional.ipynb    # Star Schema híbrido
│   ├── 05_analisis_kpis.ipynb          # 11 hipótesis + validación SQL
│   └── 06_dashboard_dash.ipynb         # Dashboard Plotly Dash
│
├── assets/                             # 27 gráficos generados por las notebooks
│
├── docs/
│   ├── documentacion_proyecto.docx     # Documentación técnica + hipótesis (completa)
│   ├── documentacion_proyecto.pdf
│   ├── Modelado.pdf                    # Modelo relacional (diagrama)
│   ├── diagrama de flujo.pdf           # Flujo del pipeline
│   └── dashboard_ecommerce_uk.pdf      # Snapshot del dashboard
│
├── modelo_relacional.dbml              # Schema para dbdiagram.io
├── .gitignore
├── requirements.txt
└── README.md
```

> **Nota:** La carpeta `data/` no está incluida en el repositorio (ver [Instalación](#instalación)).  
> Los CSVs se generan automáticamente ejecutando las notebooks en orden.

---

## Pipeline de datos

```
data/data.csv
     │
     ▼
NB1 — EDA ──────────────────────────── 13 hallazgos · 12 assets
     │
     ▼
NB2 — Análisis Estructural ─────────── 9 tablas identificadas · 5 assets
     │
     ▼
NB3 — ETL ──────────────────────────── data/clean/ (9 CSVs + Excel)
     │
     ▼
NB4 — Modelado Relacional ──────────── data/model/ (14 tablas · 27 relaciones)
     │
     ▼
NB5 — Análisis y KPIs ──────────────── 11 hipótesis · validación SQL · data/kpis/
     │
     ▼
NB6 — Dashboard Dash ───────────────── App web en localhost:8050
```

**El orden de ejecución es estricto** — cada notebook consume los outputs de la anterior.

---

## Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/MHEvans02/ecommerce-uk.git
cd ecommerce-uk
```

### 2. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 3. Descargar el dataset

El dataset original no está incluido en el repositorio por su tamaño.

1. Ir a [https://www.kaggle.com/datasets/carrie1/ecommerce-data](https://www.kaggle.com/datasets/carrie1/ecommerce-data)
2. Descargar `data.csv`
3. Crear la carpeta `data/` en la raíz del proyecto y colocar el archivo ahí:

```
ecommerce-uk/
└── data/
    └── data.csv    ← colocar aquí
```

### 4. Ejecutar las notebooks en orden

```bash
jupyter notebook
```

Abrir y ejecutar en este orden:

1. `notebooks/01_eda.ipynb`
2. `notebooks/02_analisis_estructural.ipynb`
3. `notebooks/03_etl.ipynb`
4. `notebooks/04_modelado_relacional.ipynb`
5. `notebooks/05_analisis_kpis.ipynb`
6. `notebooks/06_dashboard_dash.ipynb`

Después de ejecutar NB3, NB4 y NB5 las carpetas `data/clean/`, `data/model/` y `data/kpis/` se generan automáticamente.

---

## Las 6 notebooks

### NB1 — Análisis Exploratorio (EDA)
Exploración estadística completa del dataset crudo: completitud, distribuciones, outliers, correlaciones y análisis temporal. Documenta 13 hallazgos con nivel de severidad.

**Output:** 12 gráficos en `assets/`

---

### NB2 — Análisis Estructural
Demuestra con evidencia cuantitativa que el dataset mezcla 3 fuentes con lógicas de negocio distintas. Define los 2 filtros de clasificación y separa en 9 tablas independientes.

**Output:** 5 gráficos en `assets/`

---

### NB3 — ETL
Pipeline completo de limpieza y transformación: corrección de tipos, estandarización de texto, deduplicación (−5.268 duplicados exactos), tratamiento de nulos, outliers y columnas calculadas (`Revenue`, `IsCancellation`, `IsGuest`, `Year`, `Month`, `DayOfWeek`, `Hour`).

**Output:** 9 CSVs en `data/clean/` + Excel con 11 hojas

---

### NB4 — Modelado Relacional
Construye un Star Schema híbrido sobre las 9 tablas del ETL: 2 fact tables + 5 dimensiones + 7 tablas operativas normalizadas. Verifica las 27 relaciones FK con 0 huérfanas.

| Tabla | Filas | Tipo |
|-------|-------|------|
| fact_ventas | 532.328 | Fact table principal |
| fact_ajustes_inventario | 1.324 | Fact table secundaria |
| dim_producto | 3.926 | Dimensión |
| dim_cliente | 4.364 | Dimensión (incluye CID=−1 para Guest) |
| dim_fecha | 396 | Dimensión — generada con pd.date_range |
| dim_pais | 38 | Dimensión — PK natural Country |
| dim_concepto | 32 | Lookup de StockCodes admin |

**Output:** 14 CSVs en `data/model/` + 2 gráficos en `assets/`

---

### NB5 — Análisis y KPIs
Análisis de 11 hipótesis de negocio con 15 KPIs documentados (fórmula, unidad, granularidad). Cada KPI se calcula en pandas y se verifica con la query SQL equivalente en SQLite — diferencia 0.00 en los 5 KPIs de validación cruzada.

| Hipótesis | Resultado | Veredicto |
|-----------|-----------|-----------|
| H-01: Q4 > 40% revenue | Q4 = 28.9% / Nov = 1.8× promedio | ✗ No confirmada |
| H-02: Franja 10–15h > 70% | 74.4% del revenue | ✓ Confirmada |
| H-03: Pareto 20/74 | Top 20% → 74.6% | ✓ Confirmada |
| H-04: Frecuencia no predice ticket | Pearson r = −0.005 | ✓ Confirmada |
| H-05: Ticket Guest ≈ Identificado | Guest £1.101 = 2.3× identificado (3.0× retail) | ⚠ Inesperada |
| H-06: Top 50 SC > 20% | Top 50 = 22.1% | ✓ Confirmada |
| H-07: Bajas en período concentrado | 72.1% en 3 meses | ✓ Confirmada |
| H-08: NL y AU son mayoristas | 6 países mayoristas (3.3–7.0×) | ✓ Confirmada |
| H-09: Clientes intl más valiosos | 1.9× per capita | ✓ Confirmada |
| H-10: Cancelación estable | CV = 10.8% | ✓ Confirmada |
| H-11: Envíos < 3% revenue | 2.73% | ✓ Confirmada |

**Output:** Excel KPIs con 6 hojas + 6 gráficos en `assets/`

---

### NB6 — Dashboard Plotly Dash
App web interactiva conectada a los datos reales del modelo relacional. Filtros reactivos que recalculan los gráficos en tiempo real via callbacks Python.

**4 páginas:**
- **Resumen Ejecutivo** — KPIs globales, revenue mensual, distribución horaria, composición del revenue, cancelación mensual
- **Clientes** — Pareto acumulado, ticket por segmento, frecuencia, top 10 (filtro Mayorista/Retail)
- **Productos** — concentración del catálogo, top SKUs, tabla detallada, bajas de inventario
- **Geografía y Operaciones** — países por revenue, ratios vs UK, cancelaciones (filtro por tipo de mercado)

**Para correr:**
```bash
# Ejecutar todas las celdas de notebooks/06_dashboard_dash.ipynb
# Abrir http://localhost:8050 en el browser
```

---

## Modelo relacional

Arquitectura **Star Schema híbrido** — 2 fact tables + 5 dimensiones + 7 tablas operativas + 27 relaciones FK verificadas.

Decisiones de diseño clave:
- PKs subrogadas en 13 de 14 tablas (excepción: `dim_pais` usa `Country` como PK natural)
- `CustomerID = −1` para ventas anónimas — garantiza 0 NULLs en la FK de `fact_ventas`
- `dim_fecha` generada con `pd.date_range` — incluye los días sin actividad del período
- Normalización hasta 3FN con excepciones conscientes documentadas en cada tabla

---

## Documentación

La carpeta `docs/` contiene la documentación técnica completa en PDF y Word (`documentacion_proyecto`) con:

- Resumen ejecutivo y hallazgos principales
- Descripción del pipeline y arquitectura del proyecto
- Modelo de datos — las 9 tablas ETL y el Star Schema híbrido
- Análisis completo de las 11 hipótesis con implicancias de negocio
- Guía de instalación y uso

Además incluye el modelo relacional (`Modelado.pdf`), el diagrama de flujo del pipeline (`diagrama de flujo.pdf`) y un snapshot del dashboard (`dashboard_ecommerce_uk.pdf`).

---

## Autor

**Michael Hans Evans**  
Data Analyst ·
GitHub: [MHEvans02](https://github.com/MHEvans02)
