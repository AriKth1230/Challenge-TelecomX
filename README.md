# Telecom X — Análisis de Evasión de Clientes (Churn)

Este proyecto aborda la **evasión de clientes (churn)** en Telecom X. A partir de datos históricos expuestos como **API (JSON)**, se construye un **pipeline ETL**, se realiza **EDA** con visualizaciones y se elabora un **informe** con insights y **recomendaciones accionables** para reducir el churn.

---

## Objetivo

Identificar **patrones y factores** asociados al abandono para:
- Priorizar **segmentos en riesgo**,
- Diseñar **estrategias de retención**,
- Preparar un **dataset “model-ready”** para futuros modelos predictivos.

---

## Dataset (API)

- Fuente pública del reto LATAM (JSON con campos **anidados**):
  - `https://raw.githubusercontent.com/alura-cursos/challenge2-data-science-LATAM/refs/heads/main/TelecomX_Data.json`
  - Alternativa: `https://raw.githubusercontent.com/ingridcristh/challenge2-data-science-LATAM/main/TelecomX_Data.json`

> El JSON incluye claves anidadas: `customer`, `phone`, `internet`, `account`.  
> Se normalizan con `pd.json_normalize` para construir un **DataFrame analítico**.

---

## Estructura sugerida del repositorio
   notebooks/
       TelecomX_Churn_Analisis.ipynb
    src/
      init.py
      etl.py # Pipeline ETL (carga, normalización, limpieza, features)
      eda.py # Funciones analíticas (tablas, descriptivos, correlaciones)
      viz.py # Gráficos con matplotlib
   reports/ # Salidas (gráficos, HTMLs)
    run_analysis.py # Script end-to-end (opcional)
    requirements.txt
  
---

## Requisitos

- Python 3.9+
- Paquetes:
  - `pandas`, `numpy`, `requests`, `matplotlib` (y opcional `plotly`)

Instalación:

```bash
pip install -r requirements.txt
o bien
pip install pandas numpy requests matplotlib
```
## Cómo ejecutar
**Opción A) Notebook**
1. Abre notebooks/TelecomX_Churn_Analisis.ipynb.
2. Ejecuta las celdas en orden:
3. Extracción → carga del JSON y vista previa.
4. Transformación (ETL) → normalización, limpieza y estandarización.
5. Enriquecimiento → creación de cuentas_diarias y bins por cuantiles.
6. EDA → descriptivos, churn y visualizaciones por variables.
7. Export → CSV “model-ready”.

**Opción B) Script end-to-end**
```bash
python run_analysis.py
```
El script:
- Construye el dataset limpio
- Muestra descriptivos y tablas
- Genera gráficos clave
- Exporta telecomx_model_ready.csv

## Pipeline ETL 

1. Extracción: descarga del JSON (API).
2. Normalización: pd.json_normalize de customer, phone, internet, account.
3. Renombrado al español y nombres claros.
4. Estandarización de texto: minúsculas, trim, reemplazo de guiones/underscores en tipo_contrato, metodo_pago.
5. Conversión de tipos: cargos_mensuales, cargos_totales, meses_en_empresa.
6. Depuración: eliminación de filas con cargos_totales nulo (datos incompletos).
7. Codificación binaria (Yes/No → 1/0): abandono, tiene_pareja, tiene_dependientes, servicio_telefonico, factura_electronica.
8. Feature engineering:
9. cuentas_diarias = cargos_mensuales / 30 (prorrateo).
10. Bins por cuantiles para análisis por rangos: meses_en_empresa, cargos_mensuales, cargos_totales, cuentas_diarias.

## EDA y visualizaciones

- Distribución de churn (barras y pastel).
- Churn por categorías: tipo_contrato, metodo_pago, tipo_internet, genero.
- Churn por rangos (bins) de variables numéricas: meses_en_empresa, cargos_mensuales, cargos_totales, cuentas_diarias.
- Correlaciones numéricas (heatmap).
> Resultados esperados del dominio:
- Contrato mensual concentra mayor churn.
- Menor tenencia (clientes nuevos) → mayor probabilidad de abandono.
- Cargos mensuales altos elevan el riesgo si no hay valor percibido.
- Método de pago: electronic check suele asociarse con mayor churn.
- Servicios/engagement (seguridad, soporte) correlacionan con menor aband

  ##  Requisitos

- **Python** 3.9+ (recomendado 3.10/3.11)
- Librerías:
  - `pandas` (>=2.0.0)
  - `numpy` (>=1.24.0)
  - `requests` (>=2.31.0)
  - `matplotlib` (>=3.7.0)
  - `notebook` y `ipykernel` (para abrir/ejecutar notebooks)
  - `nbconvert` (para ejecutar el `.ipynb` desde terminal)
