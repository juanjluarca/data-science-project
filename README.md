# Violencia intrafamiliar en Guatemala (2008-2023)

Proyecto final del curso de Ciencia de Datos. Analiza 590,672 denuncias de violencia intrafamiliar registradas por el Instituto Nacional de Estadística (INE) de Guatemala entre 2008 y 2023. Incluye limpieza de datos, análisis exploratorio, un modelo de regresión logística y un dashboard interactivo.

**Preguntas de investigación**

1. ¿Quiénes son las víctimas y los agresores, y qué tipos de violencia predominan?
2. ¿Cómo cambiaron las denuncias entre 2008 y 2023, incluido el efecto de la pandemia?
3. ¿Qué diferencias hay entre departamentos al ajustar por población?
4. ¿Qué características de la víctima y del vínculo con el agresor se asocian a la violencia física?

## Integrantes y división de tareas

| Integrante | Rol | Tareas |
|---|---|---|
| _Pendiente_ | Informe reproducible | Limpieza, EDA y modelo en Quarto (Python + R) |
| _Pendiente_ | Dashboard | Power BI |
| _Pendiente_ | Dashboard | Power BI |
| _Pendiente_ | _Pendiente_ | _Pendiente_ |

## Estructura

```
.
├── Violencia_en_Guatemala.qmd    # informe (código fuente)
├── Violencia_en_Guatemala.html   # informe renderizado
├── Violencia_en_Guatemala.pdf    # informe renderizado
├── data/
│   ├── violencia_intrafamiliar_gt.csv     # datos originales (descargar, ver abajo)
│   ├── violencia_limpia.csv               # datos limpios para Power BI (los genera el informe)
│   ├── DICCIONARIO_DATOS.md               # descripción de cada variable de violencia_limpia.csv
│   └── poblacion_departamentos_2018.csv   # población por departamento, Censo 2018
├── dashboard/                    # archivo .pbix
├── requirements.txt              # paquetes de Python
└── README.md
```

## Datos

- **Fuente directa**: *Violencia Intrafamiliar en Guatemala INE 2008-2023*, Kaggle (usuario DieggsPapu, versión 11): <https://www.kaggle.com/datasets/dieggspapu/violencia-intrafamiliar-en-guatemala-ine-2008-2023>
- **Fuente original**: registros administrativos de violencia intrafamiliar del INE de Guatemala (<https://www.ine.gob.gt/>).
- **Licencia**: en Kaggle figura como *Unknown*. Los datos de origen son estadísticas públicas del INE. Se usan con fines académicos.

El archivo pesa unos 830 MB y supera el límite de GitHub, así que no está en el repositorio. Para obtenerlo:

1. Inicia sesión en Kaggle y abre el enlace anterior.
2. Descarga el archivo `violenciaIntrafamiliarGuatemala2008_2023_v3.csv` (834.25 MB).
3. Renómbralo a `violencia_intrafamiliar_gt.csv` y colócalo en la carpeta `data/`.

## Cómo reproducir el informe

Requisitos: [Quarto](https://quarto.org) ≥ 1.4, R ≥ 4.3, conexión a internet la primera vez y, para el PDF, una distribución de LaTeX. No hace falta instalar Python a mano.

```bash
# 1. Paquetes de R (reticulate >= 1.41)
Rscript -e 'install.packages(c("reticulate", "knitr", "rmarkdown", "ggplot2"))'

# 2. Renderizar (HTML y PDF). También funciona con el botón "Render" de RStudio
quarto render Violencia_en_Guatemala.qmd
```

**Python**: la primera vez, `reticulate` descarga Python y los paquetes de `requirements.txt` (con las mismas versiones) en un entorno automático gestionado por `uv`; las siguientes veces lo reutiliza. Si prefieres tu propio entorno, el informe lo usa con esta prioridad:

1. La variable `RETICULATE_PYTHON`, si está definida: `RETICULATE_PYTHON=/ruta/a/python quarto render Violencia_en_Guatemala.qmd`.
2. Un entorno `.venv/` dentro del proyecto: `python3 -m venv .venv && .venv/bin/pip install -r requirements.txt`.
3. Si no hay ninguno de los dos, el entorno automático.

El informe usa el motor **knitr**. R controla el documento y ejecuta Python con `reticulate`: Python hace la ingesta, la limpieza y el análisis exploratorio, y R ajusta y grafica el modelo (`glm`, `ggplot2`). El render tarda alrededor de un minuto y medio. Además de los informes, genera `data/violencia_limpia.csv` y `data/poblacion_departamentos_2018.csv`, que usa el dashboard. `violencia_limpia.csv` pesa unos 124 MB y tampoco va en el repositorio: el equipo lo comparte por Google Drive (_enlace pendiente_). Sus variables están descritas en `data/DICCIONARIO_DATOS.md`.

## Dashboard

_Pendiente: describir el archivo `.pbix`, cómo abrirlo y qué páginas o filtros incluye._

## Declaración de autoría

_Pendiente: breve declaración de cada integrante sobre su rol y las tareas que realizó._
