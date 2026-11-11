# Diccionario de datos: `violencia_limpia.csv`

Datos limpios de las denuncias de violencia intrafamiliar en Guatemala (2008-2023) para el dashboard de Power BI. Este archivo es la misma base que usa el informe de Quarto (`Violencia_en_Guatemala.qmd`), así que el dashboard y el informe muestran las mismas cifras.

## Resumen

| | |
|---|---|
| **Filas** | 590,672. **Cada fila es una denuncia** |
| **Columnas** | 26 |
| **Periodo** | 2008-2023 (año en que se registró la denuncia) |
| **Cobertura** | Los 22 departamentos de Guatemala |
| **Formato** | CSV separado por comas, codificación UTF-8, fechas `AAAA-MM-DD` |
| **Fuente** | INE Guatemala, versión unificada de Kaggle: [Violencia Intrafamiliar en Guatemala INE 2008-2023](https://www.kaggle.com/datasets/dieggspapu/violencia-intrafamiliar-en-guatemala-ine-2008-2023) (archivo `violenciaIntrafamiliarGuatemala2008_2023_v3.csv`) |

## De dónde sale

El archivo de Kaggle tiene 190 columnas. El informe de Quarto lo lee, lo limpia y guarda aquí solo las 26 columnas que se analizan. Los cambios respecto al original son estos:

1. **Edades, años y conteos sin dato** (en el original, `9999`) quedan **vacíos**.
2. **Edades de agresores menores de 12 años** (41 casos) quedan vacías, porque casi seguro son errores de captura.
3. **`MEDIDAS_SEGURIDAD`** se unificó: desde 2014 el INE escribía `si`/`no` en lugar de `medidas otorgadas`/`medidas no otorgadas`.
4. **Columnas nuevas** calculadas a partir de las originales: `FECHA_HECHO`, `DIA_SEMANA`, `MES_EMISION_NUM` y `DIF_EDAD`.

El archivo pesa unos 124 MB, así que se comparte por Google Drive y no está en el repositorio. Para regenerarlo, hay que renderizar el informe (ver `README.md`).

## Reglas importantes antes de hacer gráficas

- **Contar denuncias = contar filas.** No hay columna de identificador ni de cantidad.
- **Los tipos de violencia no son excluyentes.** Una denuncia puede tener violencia física y psicológica a la vez, así que los porcentajes de las cuatro columnas `VIOLENCIA_*` **suman más de 100 %**. Para una sola categoría por denuncia, usar `HEC_TIPAGRE`.
- **`ignorado` significa "sin dato"** en las columnas de texto. Conviene dejarlo visible o excluirlo con un filtro, pero no mezclarlo con otra categoría. En las columnas numéricas, "sin dato" es una **celda vacía**.
- **Dos fechas distintas.** `ANO_EMISION` y `MES_EMISION_NUM` son la fecha de **registro** de la denuncia: úsenlas para tendencias por año o mes. `FECHA_HECHO` es cuándo **ocurrió** la violencia: úsenla para el día de la semana o las fechas festivas.
- **`VIC_REL_AGR` se lee "la víctima es … del agresor".** Por ejemplo, `padres/madres` significa que la víctima es la madre o el padre del agresor, es decir, **un hijo que agrede a su padre o madre**.
- **Comparar departamentos con tasas, no con conteos.** Guatemala tiene la mayor cantidad de denuncias porque tiene más población. Usen la tasa por 100 mil habitantes (ver Medidas DAX sugeridas).
- **Son denuncias, no toda la violencia.** Un departamento con pocas denuncias puede tener más barreras para denunciar, no menos violencia.

## Variables

### Cuándo y dónde

| Columna | Tipo | Descripción | Valores |
|---|---|---|---|
| `ANO_EMISION` | Entero | Año en que se **registró** la denuncia | 2008-2023. Sin vacíos. 2008 es un año con registro parcial |
| `MES_EMISION_NUM` | Entero | Mes en que se **registró** la denuncia | 1 (enero) a 12 (diciembre). Sin vacíos |
| `FECHA_HECHO` | Fecha | Fecha en que **ocurrió** la violencia | 2000-01-01 a 2023-12-31. Vacía en el 1.1 % (fecha inválida). Puede ser anterior a 2008 si se denunció tarde. Los días 1 y 15 de cada mes aparecen de más, porque cuando no se recuerda el día exacto se suele anotar uno de esos dos |
| `DIA_SEMANA` | Entero | Día de la semana del hecho, calculado a partir de `FECHA_HECHO` | **0 = lunes**, 1 = martes, 2 = miércoles, 3 = jueves, 4 = viernes, 5 = sábado, 6 = domingo. Vacío si no hay fecha |
| `HEC_DEPTO` | Texto | Departamento donde ocurrió el hecho | 22 departamentos en minúsculas y sin tildes (`guatemala`, `alta verapaz`, `quiche`, `peten`…), más `ignorado` (0.5 %) y `desconocido` (68 casos) |
| `HEC_AREA` | Texto | Área donde ocurrió el hecho | `urbana` 54.4 %, `rural` 40.5 %, `ignorado` 5.1 % |

### Víctima

| Columna | Tipo | Descripción | Valores |
|---|---|---|---|
| `VIC_SEXO` | Texto | Sexo de la víctima | `mujeres` 88.8 %, `hombres` 11.2 % |
| `VIC_EDAD` | Entero | Edad de la víctima en años | 1-98, mediana 31. Vacía en el 1.4 % |
| `VIC_RANGO_EDAD` | Texto | Rango de edad de la víctima | `ninio` = niñez, 1-13 años (0.7 %); `adolescente` = 14-18 (5.6 %); `adulto joven` = 19-40 (70.2 %); `adulto medio` = 41-60 (18.0 %); `adulto mayor` = 61 o más (4.3 %); `ignorado` (1.4 %) |
| `VIC_NIV_ESCOLARIDAD` | Texto | Nivel educativo más alto de la víctima | `ninguno` 21.2 %, `primaria` 39.8 %, `basicos` 14.7 %, `diversificado` (bachillerato) 18.1 %, `universidad` 3.4 %, `ignorado` 2.9 %. Para ordenarlo en las gráficas, usar este mismo orden |
| `VIC_ES_INDIGENA` | Texto | Si la víctima se identifica como indígena | `no` 57.0 %, `si` 31.0 %, `ignorado` 12.0 % |
| `VIC_TRABAJA` | Texto | Si la víctima tiene un trabajo remunerado | `no` 63.3 %, `si` 36.0 %, `ignorado` 0.7 % |
| `TOTAL_HIJOS` | Entero | Número de hijos de la víctima | 0-19. Vacío en el 18.8 % |

### Agresor y relación

| Columna | Tipo | Descripción | Valores |
|---|---|---|---|
| `AGR_SEXO` | Texto | Sexo del agresor | `hombres` 86.7 %, `mujeres` 13.3 % |
| `AGR_EDAD` | Entero | Edad del agresor en años | 12-98. Vacía en el 6.6 % |
| `DIF_EDAD` | Entero | Edad del agresor **menos** edad de la víctima | Positivo = el agresor es mayor; negativo = la víctima es mayor. Vacía si falta alguna de las dos edades (7.5 %) |
| `VIC_REL_AGR` | Texto | Relación de la víctima con el agresor ("la víctima es … del agresor") | `esposos(as)` 32.5 %, `conviviente` (unión de hecho) 30.1 %, `ex-conyuges` 15.1 %, `otro pariente` 9.2 %, `hermanos(as)` 4.7 %, `padres/madres` 4.4 % (un hijo agrede a su padre o madre), `hijos(as)` 2.2 % (un padre o madre agrede a su hijo), `suegros(as)` 1.2 % (un yerno o nuera agrede), `hijastros(as)` 0.3 %, `nietos(as)` 0.1 %. **Pareja o expareja** = las tres primeras (77.7 %) |

### Tipo de violencia

| Columna | Tipo | Descripción | Valores |
|---|---|---|---|
| `VIOLENCIA_FISICA` | Texto | Si la denuncia incluye violencia física | `presente` 57.7 %, `no presente` 42.3 % |
| `VIOLENCIA_PSICOLOGICA` | Texto | Si incluye violencia psicológica | `presente` 86.2 %, `no presente` 13.8 % |
| `VIOLENCIA_SEXUAL` | Texto | Si incluye violencia sexual | `presente` 2.5 %, `no presente` 97.5 %. Está subregistrada: estos casos suelen ir por la vía penal |
| `VIOLENCIA_PATRIMONIAL` | Texto | Si incluye violencia patrimonial (daño, control o despojo de bienes o dinero) | `presente` 13.2 %, `no presente` 86.8 % |
| `HEC_TIPAGRE` | Texto | Combinación de tipos de violencia de la denuncia (una sola categoría por fila) | 15 combinaciones. Las principales: `fisica-psicologica` 37.6 %, `psicologica` 35.6 %, `fisica` 11.8 %, `fisica-psicologica-patrimonial` 6.3 %, `psicologica-patrimonial` 4.8 % |

### La denuncia

| Columna | Tipo | Descripción | Valores |
|---|---|---|---|
| `INST_DENUN_HECHO` | Texto | Institución que recibió la denuncia | `organismo judicial` 56.2 %, `policia nacional civil` 35.2 %, `ministerio publico` 6.3 %, `procuraduria de los derechos humanos` 1.2 %, `procuraduria general de la nacion` 1.1 %, `bufete popular` (menos de 0.1 %) |
| `QUIEN_REPORTA` | Texto | Quién presentó la denuncia | `victima` 96.5 %, `familiar de la victima` 1.8 %, `otro` 0.6 %, `ignorado` 1.1 % |
| `HEC_RECUR_DENUN` | Texto | Si la víctima **ya había denunciado antes** al mismo agresor (reincidencia) | `no` 83.3 %, `si` 12.1 %, `ignorado` 4.6 % |
| `MEDIDAS_SEGURIDAD` | Texto | Si se otorgaron medidas de seguridad a la víctima | `medidas otorgadas` 49.9 %, `medidas no otorgadas` 0.8 %, `ignorado` 49.3 %. Falta en la mitad de los registros y en 2008 no existe: interpretar con cuidado |

## Archivo complementario: `poblacion_departamentos_2018.csv`

Población total de cada departamento según el **XII Censo Nacional de Población 2018** (INE). Sirve para calcular tasas.

| Columna | Tipo | Descripción |
|---|---|---|
| `HEC_DEPTO` | Texto | Departamento, escrito igual que en `violencia_limpia.csv` (22 filas) |
| `POBLACION_2018` | Entero | Población total del departamento en 2018 |

**Relación en Power BI**: `poblacion_departamentos_2018[HEC_DEPTO]` (lado 1) → `violencia_limpia[HEC_DEPTO]` (lado varios). Las denuncias con `ignorado` o `desconocido` no tienen población (0.5 % del total).

## Medidas DAX sugeridas

Con los nombres de tabla `violencia_limpia` y `poblacion_departamentos_2018`:

```dax
Denuncias = COUNTROWS(violencia_limpia)

% Violencia física =
DIVIDE(
    CALCULATE([Denuncias], violencia_limpia[VIOLENCIA_FISICA] = "presente"),
    [Denuncias]
)

-- Tasa media anual por cada 100 mil habitantes.
-- En los visuales, usar el departamento de la tabla de población para que el filtro llegue a ambas tablas.
Tasa por 100 mil =
DIVIDE(
    [Denuncias],
    SUM(poblacion_departamentos_2018[POBLACION_2018]) * DISTINCTCOUNT(violencia_limpia[ANO_EMISION])
) * 100000
```

Repitan `% Violencia física` cambiando la columna para las otras violencias. Como referencia, la tasa nacional del informe es de **248 denuncias por cada 100 mil habitantes al año**.

## Consejos para el dashboard

- **Al importar el CSV**: en *Obtener datos → Texto/CSV*, elegir el origen de archivo **65001: Unicode (UTF-8)**. Revisar que `FECHA_HECHO` quede como fecha y que las edades y los años queden como número entero.
- **Nombres del día de la semana**: crear una columna con `SWITCH(violencia_limpia[DIA_SEMANA], 0, "Lunes", 1, "Martes", 2, "Miércoles", 3, "Jueves", 4, "Viernes", 5, "Sábado", 6, "Domingo")`.
- **Mapas**: los departamentos van sin tildes. Para que el mapa los ubique bien, agreguen una columna con el texto `"<departamento>, Guatemala"` y asígnenle la categoría de datos *Estado o provincia*.
- **Segmentadores útiles** (alineados con las preguntas del informe): año, departamento, área, sexo de la víctima, rango de edad, relación con el agresor y tipo de violencia.
- **Hallazgos del informe que conviene poder explorar**:
  - La violencia física baja de 67 % (2008) a 43 % (2023).
  - Caída de las denuncias de marzo a junio de 2020 (confinamiento).
  - 30 % de violencia sexual en niñas de 1 a 13 años.
  - Tasas más bajas en Quiché, Huehuetenango y Totonicapán.
