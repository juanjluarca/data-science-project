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
| Juan Luarca | Informe de Quarto | Limpieza y transformación de los datos en Python, análisis exploratorio, modelo de regresión logística en R y redacción del informe reproducible (HTML y PDF) |
| Roberto Calderón | Dashboard de Power BI | Modelo de datos y medidas DAX, diseño de las cinco páginas del dashboard con segmentadores sincronizados y documentación de su uso |
| Carlos Vela | Datos y documentación | Selección de la fuente, instrucciones de descarga, diccionario de datos y README con las instrucciones de reproducción |
| Mayda Matul | Presentación y defensa | Diapositivas de la exposición, guion de la defensa y coordinación de los tiempos de cada integrante |

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
├── dashboard/
│   ├── dashboard.pbix            # dashboard para la entrega (datos incluidos)
│   ├── dashboard.pbip            # proyecto de Power BI (código fuente, se edita aquí)
│   ├── dashboard.SemanticModel/  # modelo de datos (TMDL)
│   └── dashboard.Report/         # reporte (PBIR)
├── presentacion/
│   └── violencia-en-guatemala.pdf    # diapositivas de la defensa oral
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

El informe usa el motor **knitr**. R controla el documento y ejecuta Python con `reticulate`: Python hace la ingesta, la limpieza y el análisis exploratorio, y R ajusta y grafica el modelo (`glm`, `ggplot2`). El render tarda alrededor de un minuto y medio. Además de los informes, genera `data/violencia_limpia.csv` y `data/poblacion_departamentos_2018.csv`, que usa el dashboard. `violencia_limpia.csv` pesa unos 124 MB y tampoco va en el repositorio: el equipo lo comparte por [Google Drive](https://drive.google.com/drive/folders/1VRrZMza08YiJrxO6vL2MaNazTsPhTS_K?usp=sharing). Sus variables están descritas en `data/DICCIONARIO_DATOS.md`.

## Dashboard

El dashboard está en `dashboard/` en dos formatos:

- **`dashboard.pbix`**: el archivo de la entrega. Incluye los datos y el modelo, así que se abre directamente en Power BI Desktop sin configurar nada.
- **`dashboard.pbip`**: el proyecto de Power BI, que es el código fuente del dashboard. Guarda el modelo de datos en formato TMDL (`dashboard.SemanticModel/`) y el reporte en formato PBIR (`dashboard.Report/`). Son archivos de texto, así que los cambios se pueden revisar en Git. Lee `data/violencia_limpia.csv` y `data/poblacion_departamentos_2018.csv`, los mismos datos del informe.

Son copias independientes: los cambios se hacen en el `.pbip` y después se vuelve a generar el `.pbix` (paso 4 de abajo). Un cambio hecho solo en el `.pbix` no pasa al `.pbip`.

### Abrir, ajustar la ruta y actualizar

Requisitos: Power BI Desktop para Windows, en una versión reciente, y los dos CSV en la carpeta `data/` (ver [Cómo reproducir el informe](#cómo-reproducir-el-informe)).

1. Abre `dashboard/dashboard.pbip` con Power BI Desktop.
2. Indica dónde están los CSV: **Inicio > Transformar datos > Editar parámetros > RutaDatos**. Escribe la ruta de la carpeta `data` del proyecto, terminada en `\`. Por ejemplo:
   - Repositorio en Windows: `C:\Users\tu_usuario\data-science-project\data\`
   - Repositorio dentro de WSL: `\\wsl.localhost\Ubuntu\home\tu_usuario\...\data-science-project\data\`

   El valor que trae por defecto es la ruta de quien creó el proyecto. Las dos consultas usan este parámetro, así que solo hay que cambiarlo en un lugar.
3. Pulsa **Inicio > Actualizar**. Carga las 590,672 denuncias.
4. Para generar el archivo de la entrega, usa **Archivo > Guardar como**, elige el tipo **Archivo de Power BI (.pbix)** y reemplaza `dashboard/dashboard.pbix`. Antes de subirlo a GitHub, comprueba su tamaño: GitHub rechaza archivos de más de 100 MB. El actual pesa unos 5 MB.

**Mapa de la página Departamentos.** Usa el visual de Azure Maps y muestra un círculo por departamento, con su tamaño según la tasa. Para colorear el área de cada departamento (mapa coroplético): selecciona el mapa, abre **Formato del visual**, activa la **capa de mapa coroplético** (*Filled map layer*) y en **Colores** pulsa **fx** para dar formato por **degradado** según la medida *Tasa por 100 mil*. Después puedes desactivar la capa de burbujas. Si el mapa no se muestra, activa los mapas en **Archivo > Opciones y configuración > Opciones > Seguridad > Usar visuales de mapa y de mapa coroplético**. Necesitan conexión a internet.

### Páginas

| Página | Pregunta | Qué muestra |
|---|---|---|
| Resumen | - | 6 indicadores (denuncias, promedio anual, tasa por 100 mil, % de víctimas mujeres, % con agresor pareja o expareja, % con violencia física), denuncias por año, tipos de violencia, vínculo con el agresor y hallazgos principales |
| Víctimas y agresores | 1 | Sexo de víctima y agresor, relación con el agresor, edad y sexo de la víctima, tipos de violencia y sus combinaciones, violencia sexual por edad y sexo |
| Evolución en el tiempo | 2 | Serie mensual 2008-2023, variación interanual, tipos de violencia por año, 2020 frente al promedio 2017-2019 y día de la semana del hecho |
| Departamentos | 3 | Mapa, tasa por 100 mil por departamento frente a la nacional, tasa frente a % de víctimas indígenas y tabla resumen |
| Factores de violencia física | 4 | % de denuncias con violencia física según escolaridad, pueblo, área, vínculo, edad y sexo de la víctima |

**Filtros**: todas las páginas tienen los mismos segmentadores, sincronizados entre sí: año de registro, departamento, área, sexo de la víctima, rango de edad y relación con el agresor. El gráfico "2020 frente al promedio 2017-2019" ignora a propósito el filtro de año.

### Modelo de datos

- `violencia_limpia`: una fila por denuncia. Además de las 26 columnas del diccionario, tiene columnas para mostrar en el reporte con etiquetas en español y en orden (`RANGO_EDAD`, `ESCOLARIDAD`, `RELACION`, `RELACION_GRUPO`, `DIA_SEMANA_NOMBRE`, etc.). Las columnas originales no cambian.
- `poblacion_departamentos_2018`: población del Censo 2018, con el nombre del departamento con tildes (`DEPARTAMENTO`). Se relaciona con `violencia_limpia` por `HEC_DEPTO`.
- `Calendario`: fechas de registro de las denuncias (`ANO_EMISION` y `MES_EMISION_NUM`). Las tendencias usan la fecha de registro; el día de la semana usa la fecha del hecho.
- `Medidas`: 24 medidas DAX en las carpetas General, Víctima y agresor, Tipos de violencia, Tiempo y Territorio. La tasa por 100 mil es la tasa media anual con la población de 2018, igual que en el informe (248 a nivel nacional).

## Presentación

Las diapositivas de la defensa oral están en [`presentacion/violencia-en-guatemala.pdf`](presentacion/violencia-en-guatemala.pdf). La exposición dura de 15 a 20 minutos.

## Declaración de autoría

Cada integrante declara su rol y las tareas que realizó en el proyecto.

- **Juan Luarca** (Informe de Quarto): limpié y transformé los datos en Python, hice el análisis exploratorio, ajusté y evalué el modelo de regresión logística en R y redacté el informe reproducible en Quarto.
- **Roberto Calderón** (Dashboard de Power BI): construí el modelo de datos y las medidas DAX, diseñé las páginas del dashboard alineadas con las preguntas de investigación y documenté cómo abrirlo y actualizarlo.
- **Carlos Vela** (Datos y documentación): seleccioné y documenté la fuente de datos, escribí las instrucciones de descarga, elaboré el diccionario de datos y redacté el README.
- **Mayda Matul** (Presentación y defensa): preparé las diapositivas de la defensa oral, organicé el guion de la exposición y coordiné los tiempos de cada integrante.
