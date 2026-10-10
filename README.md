# Sistema de optimización de precios y segmentación de mercado en Airbnb

Código del análisis del Trabajo Fin de Máster del **Máster en Data Science y Big Data** de la Universidad de Sevilla.

**Autor:** Antón Maestre Gómez · **Tutor:** Francisco Jesús Martín Mateos

## Consultar los documentos

Los cuatro documentos del análisis pueden leerse directamente en el navegador, sin descargar ni instalar nada:

**https://antonmg4.github.io/tfm-airbnb-analisis/**

| Orden | Documento | Contenido | Ver |
|:-:|---|---|---|
| 1 | `TFM_Airbnb_ETL` | Limpieza de los alojamientos, conversión a euros, detección de precios sospechosos, tratamiento de nulos y variables de comodidades | [Abrir](https://antonmg4.github.io/tfm-airbnb-analisis/TFM_Airbnb_ETL.html) |
| 2 | `TFM_Reviews_ETL` | Deduplicación e integridad de las reseñas, evolución temporal y estacionalidad | [Abrir](https://antonmg4.github.io/tfm-airbnb-analisis/TFM_Reviews_ETL.html) |
| 3 | `TFM_Clustering` | PCA de las reseñas, selección de variables, clustering en dos niveles e Isolation Forest | [Abrir](https://antonmg4.github.io/tfm-airbnb-analisis/TFM_Clustering.html) |
| 4 | `TFM_ModeloPrecio` | Variable objetivo, XGBoost y Random Forest, evaluación, importancia de variables, rango de referencia y exportación a la aplicación | [Abrir](https://antonmg4.github.io/tfm-airbnb-analisis/TFM_ModeloPrecio.html) |

Cada documento combina el código, sus resultados y los gráficos con la justificación de cada decisión. Se recomienda leerlos en el orden indicado, que coincide con el orden de ejecución.

## Estructura del repositorio

```
tfm-airbnb-analisis/
├── TFM_Airbnb_ETL.qmd          # Documentos fuente (Quarto)
├── TFM_Reviews_ETL.qmd
├── TFM_Clustering.qmd
├── TFM_ModeloPrecio.qmd
├── TFM_*.html                  # Documentos renderizados
├── TFM_*_files/                # Gráficos y estilos de cada HTML
├── index.html                  # Página de inicio de GitHub Pages
└── data/                       # Datos (no incluidos, ver más abajo)
```

## Flujo de datos

Cada documento lee los ficheros generados por el anterior:

| Documento | Entrada | Salida |
|---|---|---|
| `TFM_Airbnb_ETL` | `data/listings.csv` | `data/listings_clean.csv` |
| `TFM_Reviews_ETL` | `data/reviews.csv`, `data/listings_clean.csv` | `data/reviews_clean.csv` |
| `TFM_Clustering` | `data/listings_clean.csv`, `data/reviews_clean.csv` | `data/listings_enriched.csv` |
| `TFM_ModeloPrecio` | `data/listings_enriched.csv` | `app_artefactos/` (modelo y tablas de la aplicación) |

## Reproducir el análisis

1. **Datos.** Los ficheros de partida no se incluyen por su tamaño. Se descargan del conjunto [Airbnb Listings & Reviews](https://www.kaggle.com/datasets/mysarahmadbhat/airbnb-listings-reviews) de Kaggle (licencia CC0) y se colocan como `data/listings.csv` y `data/reviews.csv`.
2. **Software.** R y [Quarto](https://quarto.org), con los paquetes:
   ```r
   install.packages(c("tidyverse", "naniar", "skimr", "jsonlite", "FSelector", "caret",
                      "cluster", "factoextra", "isotree", "randomForest", "xgboost",
                      "GGally", "corrplot", "geosphere"))
   ```
3. **Ejecución.** Renderizar los documentos en el orden de la tabla, por ejemplo con `quarto render TFM_Airbnb_ETL.qmd`.

El ajuste de Random Forest en `TFM_ModeloPrecio` tarda más de 10 horas; el resto de documentos se ejecuta en minutos.

## Aplicación web

El modelo final está disponible en una aplicación web, cuyo código se encuentra en un repositorio aparte:

- Aplicación: https://antonmg-recomendador-precio-app.hf.space
- Código: https://github.com/AntonMG4/tfm-precio-app

Los ficheros de `app_artefactos/` generados por `TFM_ModeloPrecio` son los que la aplicación carga desde su carpeta `datos/`.
