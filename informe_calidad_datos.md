# Informe de calidad de datos geoespaciales — V2.0

**Proyecto:** Urban Creek  
**Responsable:** Alejandro Usma  
**Fecha de auditoría:** 2026-10-20  
**Versión:** 2.0  
**Software:** ArcGIS Pro 3.7.0

---

## 1. Resumen

La auditoría inicial de datos del proyecto Urban Creek comprende la validación y evaluación de la calidad de datos de análisis. En particular, se auditaron las capas vectoriales de retiros, quebradas, área de interés específica y construcciones; así como las capas raster correspondientes a las ortofotos 2004, 2010, 2016, 2019, 2021 y 2024 de Medellín.

El proceso para datos vectoriales incluyó verificación y corrección de geometrías, identificación de duplicados geométricos y de identificación, y validación topológica. Las capas de retiros y quebradas fueron auditadas en la extensión de interés (`aoi`). La capa de construcciones fue auditada en toda la extensión de interés hasta la identificación de duplicados, restringiendo la validación topológica únicamente a aquellas instancias localizadas en el área de interés específica (`aoie`). En general, los datos mostraron una buena calidad. Únicamente las capas de retiros y construcciones presentaron algún tipo de error geométrico, y construcciones presentó algunos errores topológicos que resultaron ser excepciones legítimas luego de inspeccionar a fondo. No se identificaron bloqueos críticos para iniciar las fases subsiguientes. Las capas `aoie`, `quebradas` y `retiros` están depuradas para su uso en dichas fases. La capa `construcciones` está preparada para iniciar el fechado, sujeto a la revisión de los hallazgos pendientes documentados en la sección 5.

En cuanto a los datos ráster, el proceso incluyó la localización de sitios de validación posicional repartidos por toda la extensión de interés (`aoi`) pero por fuera del área de interés específica (`aoie`), debido a la ausencia de un número suficiente de construcciones dentro de esta área en las ortofotos más antiguas (2004 y 2010). La validación posicional comprendió la medición de los vectores de desfase entre esquinas de construcciones bajas (máximo dos pisos) visibles en las ortofotos con respecto a los vértices correspondientes en la capa catastral de `construcciones` como referencia. Con esta información se calcularon los desplazamientos y errores cuadráticos por vector, y finalmente el RMSE por ortofoto. Las ortofotos de 2004 y 2010 presentan RMSE de 1,41 m y 1,06 m, respectivamente, superiores a los de las ortofotos posteriores. Esta diferencia podría estar relacionada con errores de ortorrectificación u otras fuentes de incertidumbre posicional, pero la validación realizada no permite determinar su causa de forma concluyente. Los RMSE de las ortofotos de 2016 y posteriores oscilan entre los 0.77 y 0.80 m. Esta información permite concluir que los datos ráster son aceptables para los objetivos de análisis de este estudio (verificar presencia/ausencia de edificaciones presentes en la capa de `construcciones`) y no requieren georreferenciación adicional.

## 2. Datos auditados

### 2.1. Datos vectoriales

Las capas vectoriales auditadas se listan a continuación:

| Capa | Fuente / archivo de origen | Fecha de descarga | CRS original | Registros de entrada | Flujo aplicado |
|---|---|---|---|---:|---|
| `construcciones` | [Geomedellín](https://www.medellin.gov.co/geomedellin/datosAbiertos/395) | 2026-10-13 | EPSG:9377 | 1133927 | `selección → verificación → reparación → verificación → auditoría → promoción → selección → topología` |
| `quebradas` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/vivienda_ciudad_terri/VM_Base_Catastral/MapServer/9) | 2026-10-08 | EPSG:9377 | 4212 | `selección → verificación → auditoría → promoción → topología` |
| `retiros` | [Alcaldía de Medellín](https://www.medellin.gov.co/geomedellin/datosAbiertos/343) | 2026-10-05 | EPSG:6257 | 1289 | `selección → verificación → proyección (MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1) → verificación → reparación → verificación → auditoría → promoción` |
| `aoie` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/ServiciosCatastro/Base_Catastral/MapServer/1) | 2026-10-12 | EPSG:9377 | 349 | `selección → verificación → promoción` |

### 2.2. Datos ráster

Las capas ráster auditadas se listan a continuación:

| Capa | Fuente / archivo de origen | Fecha de descarga | CRS original | Resolución espacial origen | Flujo aplicado |
|---|---|---|---|---:|---|
| `ortofoto_2004` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/ServiciosImagen/Ortofotos_2004_SIGMA/ImageServer) | 2026-10-09 | EPSG:6257 | 13 cm | `descarga → mosaicado → verificación → validación → proyección (bilineal, MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1) → revisión → promoción` |
| `ortofoto_2010` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/ServiciosImagen/CARTOANTIOQUIA_2010_URBANO_Z3/ImageServer) | 2026-10-09 | EPSG:6257 | 15 cm | `descarga → mosaicado → verificación → validación → proyección (bilineal, MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1) → revisión → promoción` |
| `ortofoto_2016` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/ServiciosImagen/Ortofoto_Medellin_2016_Centro/ImageServer) | 2026-10-09 | EPSG:6257 | 7 cm | `descarga → mosaicado → verificación → validación → proyección (bilineal, MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1) → revisión → promoción` |
| `ortofoto_2019` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/ServiciosImagen/Ortofoto_Medellin_2019/ImageServer) | 2026-10-09 | EPSG:6257 | 8 cm | `descarga → mosaicado → verificación → validación → proyección (bilineal, MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1) → revisión → promoción` |
| `ortofoto_2021` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/ServiciosImagen/Ortofoto_Medellin_2021_MAGNA_Medellin/ImageServer) | 2026-10-09 | EPSG:6257 | 8 cm | `descarga → mosaicado → verificación → validación → proyección (bilineal, MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1) → revisión → promoción` |
| `ortofoto_2024` | [Alcaldía de Medellín](https://www.medellin.gov.co/servidormapas/rest/services/ServiciosImagen/Ortofoto_Medellin_2024/ImageServer) | 2026-10-09 | EPSG:9377 | 8 cm | `descarga → mosaicado → verificación → validación → revisión → promoción` |

## 3. Método de auditoría

En esta sección se explican los flujos de trabajo seguidos para auditar los datos vectoriales y ráster. Cada etapa de cada flujo se llevó a cabo sobre copias de trabajo, conservando la trazabilidad de la información en cada etapa. No todas las capas requirieron realizar todos los pasos.

### 3.1. Flujo de trabajo vectorial

El flujo de trabajo vectorial se sintetiza como `selección → verificación → reparación → verificación → proyección → verificación → reparación → verificación → auditoría → promoción → topología`.

| Etapa | Propósito | Evidencia / resultado |
|---|---|---|
| `selección` | Seleccionar y preparar los registros de interés. | `construcciones` (intersecan `aoi`): 48343 <br> `construcciones` (intersecan `aoie`): 1289 <br> `quebradas` (intersecan `aoi`): 113 <br> `retiros` (intersecan `aoi`): 72 <br> `aoi` (polígono 6097): 1 |
| `verificación` | Revisar la calidad geométrica. | Se ejecutó en todas las capas una primera vez, una segunda vez en aquellas que requirieron reparación, y nuevamente luego de reproyectar. Si luego de reproyectar aparecieron errores, se corrió una vez más luego de repararlos: <br> `construcciones`: 9 <br>  `quebradas`: 0 <br> `retiros` (antes de proyección): 0 <br> `retiros` (después de proyección): 1 <br> `aoie`: 0 |
| `proyección` | Transformar los datos al CRS de destino. | `retiros`: De EPSG:6257 a EPSG:9377 (transformación `MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1`) |
| `reparación` | Reparar geometrías cuando corresponda. | Se usó repair geometry en `construcciones` y `retiros` (después de proyección), resultando en 0 errores |
| `auditoría` | Revisar duplicados, atributos, valores extremos y otras anomalías. | `construcciones`: 4 geometrías duplicadas dentro de `aoie`, todas ellas justificadas al tratarse de mejoras. 1261 instancias con el campo `ID_CONSTRUCCION` duplicado, justificado ya que una misma construccion puede tener diferentes polígonos (balcones, desniveles, entre otros). No hay valores nulos y los datos son estadísticamente consistentes. Existe un valor de `TIPO_CONSTRUCCION` de 0 en dos registros. Corresponde a dos espacios dentro de edificios que pueden haber sido asignados a propósito. No debería afectar análisis posteriores <br> `quebradas`: Sin duplicaciones ni inconsistencias en atributos <br> `retiros`: sin duplicaciones ni inconsistencias en atributos |
| `promoción` | Aceptar la salida validada para la siguiente fase del flujo de trabajo e incorporarla a la GDB oficial del proyecto (urban_creek.gdb). | La promoción certifica los controles de calidad documentados en esta auditoría, pero no implica que el fechado ni la revisión de los hallazgos pendientes de construcciones estén completados |
| `topología` | Evaluar las relaciones espaciales entre entidades. | `construcciones`: Must Not Overlap (Area) (0 errores, 4 excepciones) <br> `quebradas`: Must Not Overlap (Line), Must Not Self Overlap (Line) (0 errores) |

### 3.2. Flujo de trabajo ráster

El flujo de trabajo ráster se sintetiza como `descarga → mosaicado → verificación → validación → proyección → revisión → promoción`.

| Etapa | Propósito | Evidencia / resultado |
|---|---|---|
| `descarga` | Descargar teselas regulares que cubran la extensión de interés, desde el servicio REST que permite acceder a los datos. | Se descargaron 32 teselas para las ortofotos de 2004 hasta 2021, y 80 teselas para la ortofoto de 2024 debido a un menor tamaño máximo de área descargable. |
| `mosaicado` | Unir todas las teselas de una misma ortofoto en un solo ráster dataset. | Cada ortofoto quedó contenida en un solo ráster para la extensión de interés. |
| `verificación` | Revisar los ráster resultantes en búsqueda de líneas de costura, áreas faltantes, artefactos o metadatos inconsistentes con los archivos originales. <br> Recálculo de pirámides y estadísticas ráster (opcional). | Los archivos ráster resultantes son consistentes con los archivos originales en términos de CRS, tamaño de celda, número de bandas, tipo de dato y método de compresión. No se observaron líneas de costura o áreas faltantes. Sin embargo, se observaron artefactos en algunas ortofotos que resultaron ser producto de pirámides poco optimizadas. En estos casos, se recalcularon las pirámides y estadísticas ráster, dando como resultado imágenes coherentes y utilizables. |
| `validación` | Cuantificar la exactitud posicional de las ortofotos tomando como referencia la capa catastral de `construcciones`. | Se ubicaron 12 puntos de validación distribuidos por la extensión de interés y por fuera del área de interés específica, debido a que era necesario ubicarlos sobre vértices de construcciones ya existentes en 2004 para evaluar la exactitud de forma consistente a lo largo de todo el periodo de análisis. Luego, se midieron los vectores de desplazamiento desde las esquinas visibles en las ortofotos hacia el vértice de referencia correspondiente en la capa de `construcciones`. Estos vectores permitieron conocer la magnitud de los desplazamientos y sus componentes horizontales y verticales, utilizadas para calcular el RMSE posicional bidimensional mediante la expresión $RMSE_{2D} = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(dx_i^2 + dy_i^2)}$, donde $dx_i$ y $dy_i$ corresponden a las componentes horizontal y vertical del desplazamiento del punto $i$, respectivamente, y $n$ es el número total de puntos de validación por ortofoto ($n=12$). El resultado se expresa en metros. Las ortofotos de 2004 y 2010 tuvieron RMSE superiores a un metro; esto podría deberse a errores de ortorrectificación u otras fuentes de incertidumbre posicional. Las ortofotos de 2016 en adelante tuvieron RMSE entre 0.77 y 0.80 m. Los resultados muestran una concordancia posicional variable entre las ortofotos y la referencia catastral. Para los objetivos específicos de este estudio —identificar la presencia o ausencia de edificaciones mediante comparación visual—, se considera que las ortofotos son operativamente utilizables. Esta conclusión está condicionada por el tamaño y la distribución de la muestra, y no constituye una certificación de exactitud cartográfica para una escala determinada. |
| `proyección` | Reproyectar los ráster dataset hacia el mismo CRS `EPSG:9377` | Las ortofotos 2004, 2010, 2016, 2019 y 2021 se reproyectaron desde el CRS `EPSG:6257` hacia `EPSG:9377` utilizando la transformación `MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1` y el método de remuestreo bilineal. La ortofoto de 2024 ya estaba en el CRS `EPSG:9377` desde su origen, así que no fue necesario reproyectarla. |
| `revisión` | Revisar visualmente que no se hayan generado artefactos o desplazamientos luego de la reproyección. | Las versiones reproyectadas de las ortofotos están libres de artefactos y desplazamientos con respecto a las originales. |
| `promoción` | Aceptar la salida validada para la siguiente fase del flujo de trabajo e incorporarla a la GDB oficial del proyecto (urban_creek.gdb). | La promoción certifica los controles de calidad documentados en esta auditoría. |

## 4. Resultados: acciones y hallazgos

### 4.1. Datos vectoriales

El registro completo de verificaciones, hallazgos y acciones por capa vectorial se puede consultar en la [tabla de hallazgos](./tablas/v_hallazgos.tsv).

### 4.2. Datos ráster

Los desplazamientos en `x` y en `y`, así como los desfases y el RMSE para cada ortofoto como resultado del proceso de validación posicional se muestran a continuación:

| Ortofoto | Número de puntos | dx medio (m) | dx std (m) | dy medio (m) | dy std (m) | Desfase medio (m) | Desfase máximo (m) | RMSE (m) |
|---|---|---|---|---|---|---|---|---|
|Ortofoto 2004 | 12 | -0.40 | 0.95 | -0.19 | 1.02 | 1.29 | 2.56 | 1.41 |
|Ortofoto 2010 | 12 | -0.01 | 0.76 | -0.61 | 0.51 | 0.95 | 1.74 | 1.06 |
|Ortofoto 2016 | 12 | 0.21 | 0.37 | -0.46 | 0.53 | 0.72 | 1.39 | 0.80 |
|Ortofoto 2019 | 12 | 0.22 | 0.40 | -0.44 | 0.47 | 0.67 | 1.36 | 0.77 |
|Ortofoto 2021 | 12 | 0.28 | 0.34 | -0.34 | 0.58 | 0.69 | 1.37 | 0.78 |
|Ortofoto 2024 | 12 | 0.22 | 0.35 | -0.47 | 0.51 | 0.69 | 1.31 | 0.79 |

Las ortofotos de 2016 a 2024 presentan RMSE entre 0,77 y 0,80 m, con componentes medias de desplazamiento similares entre ellas. Las ortofotos de 2004 y 2010 presentan RMSE de 1,41 m y 1,06 m, respectivamente, lo que indica una menor concordancia posicional con la referencia catastral utilizada. Esta diferencia podría estar relacionada con errores de ortorrectificación u otras fuentes de incertidumbre posicional, pero la validación realizada no permite determinar su causa de forma concluyente.

Considerando el propósito del estudio —determinar la presencia o ausencia de edificaciones a partir de la comparación visual con la capa catastral de construcciones—, se decidió conservar las ortofotos sin georreferenciación adicional y documentar los errores medidos como referencia de la incertidumbre posicional. Esta decisión se limita al objetivo de análisis definido y no implica que las ortofotos satisfagan necesariamente un estándar general de exactitud cartográfica.

## 5. Asuntos abiertos

**Fechado de las construcciones.** Se asignarán las fechas de construcción mediante inspección visual y contraste temporal con las ortofotos disponibles, determinando, para cada entidad, su presencia o ausencia en las distintas fechas de observación. La fecha asignada estará sujeta a la resolución temporal de las ortofotos disponibles.

**Revisión de construcciones transitorias.** Los seis polígonos clasificados como transitorios (OBJECTID 14209, 14211, 14215, 14217, 14219 y 15492) no presentan huellas consistentes en las ortofotos revisadas. Durante la fase de fechado se determinará si deben conservarse, ajustarse o eliminarse y, cuando corresponda, si es necesario digitalizar manualmente las huellas de construcciones permanentes observadas.

## 6. Limitaciones y reproducibilidad

### 6.1. Entorno de trabajo

| Elemento | Valor |
|---|---|
| Software | ArcGIS Pro 3.7.0 |
| Fecha final de ejecución | 2026-10-19 |
| Geodatabase de origen | `raw.gdb` |
| Geodatabase de trabajo | `intermediate.gdb` |
| Geodatabase definitiva | `urban_creek.gdb` |
| CRS de destino | EPSG:9377 |
| Script / modelo utilizado | No aplica |

### 6.2. Convención de nombres

| Sufijo / convención | Significado | Ejemplo |
|---|---|---|
| `_sel` | Capa seleccionada | `construccion_sel` |
| `_9377` | Capa proyectada | `retiros_corrientes_sel_9377` |
| `_repair` | Capa reparada | `construccion_sel_repair` |

### 6.3. Limitaciones

La topología de `construcciones` se evaluó únicamente dentro del área de interés específica (`aoie`). No se evaluó la topología de `retiros`, ya que la calidad del análisis no se ve impactada por la presencia o no de traslapes o incumplimiento de reglas topológicas específicas. El cumplimiento de reglas geométricas basta para esta capa.

Con respecto a la validación de los datos ráster, es importante tener en cuenta que la muestra para la evaluación de la exactitud posicional es pequeña (12 puntos por ortofoto). Además, los puntos de validación están en el borde consolidado, así que el error en el centro del área de interés específica `aoie` no está medido directamente. Finalmente, el resultado mide concordancia entre dos fuentes, no exactitud absoluta, y la referencia es la capa catastral de `construcciones`, que tiene su propia precisión gráfica.

### 6.4. Reproducción y evidencias

- **Diccionario de datos:** [`tablas/diccionario_datos.tsv`](./tablas/diccionario_datos.tsv)
- **Hoja de hallazgos:** [`tablas/v_hallazgos.tsv`](./tablas/v_hallazgos.tsv)
- **Resultados estadísticos:** [`master_urban_creek/v-stats`](https://docs.google.com/spreadsheets/d/15PgQ1Kc-udwsVlLMGDneEA-I97T0o1CEujMTC6aOn-k/edit?gid=287431301#gid=287431301&range=B2:M2)
- **Resultados topología:** [`master_urban_creek/v-topo-errors`](https://docs.google.com/spreadsheets/d/15PgQ1Kc-udwsVlLMGDneEA-I97T0o1CEujMTC6aOn-k/edit?gid=1958271399#gid=1958271399&range=B2)
- **Vectores de desplazamiento:** [`tablas/r_desfases.tsv`](./tablas/r_desfases.tsv)
- **Estadísticas de validación posicional ráster:** [`master_urban_creek/r-stats`](https://docs.google.com/spreadsheets/d/15PgQ1Kc-udwsVlLMGDneEA-I97T0o1CEujMTC6aOn-k/edit?gid=825215358#gid=825215358&range=B2)
- **Notebook de manipulación ráster:** [`notebooks/raster_processing.ipynb`](./notebooks/raster_processing.ipynb)

No se utilizaron modelos ni scripts para la auditoría de datos vectoriales.

---

## Historial de versiones

| Versión | Fecha | Cambios | Estado |
|---|---|---|---|
| 1.0 | 2026-10-16 | Primera versión del informe de calidad. | Aprobado |
| 2.0 | 2026-10-20 | Adición de las secciones de calidad ráster. | Aprobado |

