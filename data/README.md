# Carpeta `data/`

Esta carpeta **no se versiona** (ver `.gitignore`): contiene el dataset fuente y todos los
ficheros derivados que generan los notebooks. Solo se sube este `README.md` para documentar
qué debe haber aquí y quién lo produce.

## 1. Fichero de entrada (hay que descargarlo)

| Fichero | Origen | Descripción |
|---|---|---|
| `sp500_history.parquet` | [Google Drive del enunciado](https://drive.google.com/file/d/1nvubXdAu0EONlrP_yrURZbnPhBQ-uDaB/view?usp=sharing) | Histórico diario del S&P 500 en formato *long*: 7.250.110 filas, 1.289 tickers, 1990-01-02 → 2026-01-30. Columnas: `date`, `symbol`, `assetid`, `security_name`, `sector`, `industry`, `subsector`, `in_sp500`, `open`, `high`, `low`, `close`, `volume`, `unadjusted_close`. |

Descárgalo y colócalo en esta carpeta antes de ejecutar `practica/nb1_carga_datos.ipynb`.

> El notebook opcional `extra/nb0_exploracion_parquet.ipynb` busca el parquet **subiendo
> directorios** desde su ubicación (no en `data/`). Si quieres ejecutarlo, deja también una
> copia del parquet en la raíz del repositorio.

## 2. Ficheros generados por los notebooks

| Fichero | Lo genera | Lo consume | Contenido |
|---|---|---|---|
| `closes.parquet` | NB1 (y NB2 re-exporta limpio) | NB2, NB3, NB4, NB5 | Precios *close* en formato wide: 9.087 × 1.273 (fecha × ticker) |
| `opens.parquet` | NB1 (y NB2 re-exporta limpio) | NB2, NB3, NB4 | Precios *open* en formato wide |
| `sp500_membership.parquet` | NB1 (y NB2 re-exporta limpio) | NB2, NB3 | Pertenencia al índice (1/0) en formato wide |
| `spy.parquet` | NB1 (yfinance) | NB2, NB3, NB5 | Benchmark SPY: `open` y `close` diarios |
| `rebalance_dates.csv` | NB2 | NB3 | 133 fechas de rebalanceo (último día hábil de cada mes, 2015-01 → 2026-01) |
| `eligible_universe.parquet` | NB2 | NB3, NB5 | Universo elegible por fecha de rebalanceo (133 × 1.273, valores 1/0) |
| `share_class_log.csv` | NB2 | — | Traza de tickers eliminados por clase de acción duplicada en cada rebalanceo |
| `signals.csv` | NB3 | — | **Entregable del enunciado**: los 20 activos seleccionados en cada fecha (2.660 filas) |
| `target_portfolios.parquet` | NB3 | NB4 | Pesos objetivo en formato wide (133 × 416) |
| `equity_costs.parquet` | NB4 | NB5 | Curva de capital diaria **con** costes de transacción |
| `equity_no_costs.parquet` | NB4 | NB5 | Curva de capital diaria **sin** costes (contrafactual) |
| `logs_costs.parquet` | NB4 | NB5 | Registro de las 3.686 operaciones ejecutadas, con comisión por orden |

Ejecutando los notebooks en orden (NB1 → NB5) se regenera todo el contenido de esta carpeta.
