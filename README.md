# Evolución de la ocupación constructiva en el área de expansión Altos de Calasanz (Medellín), 2004-2024

> **Estado: proyecto en curso.** Fase actual: auditoría de datos y preparación de la serie temporal. Aún no hay resultados del análisis; este repositorio documenta el método y la calidad de los insumos.

## Pregunta

¿Cómo se ocupó con construcciones, entre 2004 y 2024, un sector de Medellín que pasó de estar casi vacío a estar urbanizado, y cómo se distribuye esa ocupación dentro y fuera de las fajas de retiro de la red hídrica definidas por el POT?

La faja de retiro actúa como **variable de contraste**, no como protagonista. El proyecto describe **coincidencia espacial y época de aparición**; no evalúa cumplimiento normativo ni riesgo, y no establece relaciones causales.

## Zona de estudio

- **Área de expansión Altos de Calasanz** (código de barrio 6097, capa oficial de barrios), ~75 ha (750.000 m²).
- Se adopta el polígono oficial tal cual, sin ajustes propios. El desarrollo adyacente fuera del polígono no se analiza. Se usa solo como contexto para mapeo.

## Datos de entrada

Todos son datos abiertos de la **Alcaldía de Medellín** (licencia declarada: CC BY-SA 4.0). **No se incluyen en este repositorio**; se enlazan los servicios originales.

| Insumo | Fuente | Fecha de descarga | Notas |
|---|---|---|---|
| Retiros de la red hídrica (POT) | Geomedellín, datos abiertos | 2026-10-05 | Capa actualizada por última vez en 2017 |
| Construcciones (catastro) | Servicio REST, capa Construcciones | 2026-10-13 | La Alcaldía declara actualización a 2025 |
| Quebradas | Servicio REST, capa Quebradas Medio Ambiente | 2026-10-08 | Escala 1:10000 |
| Ortofotos 2004, 2010, 2016, 2019, 2021, 2024 | Servicios de imagen de la Alcaldía | 2026-10-09/10 | 2004 y 2010 parecen no estar ortorrectificadas |
| Modelo digital de terreno 2024 (1 m) | Servicio de imagen de la Alcaldía | 2026-10-09 | Contexto |
| Límites de barrio | Geomedellín | 2026-10-12| Define la zona de estudio |

El detalle (URL, metadatos, CRS, escala, comentarios) está en [`tablas/diccionario_datos.tsv`](tablas/diccionario_datos.tsv). 

## Método (resumen)

1. **Copia congelada** de cada capa y recorte por extensión (selección de entidades completas, sin *Clip*).
2. **Auditoría vectorial:** geometrías (Check/Repair Geometry), duplicados, nulos y extremos, topología.
3. **Auditoría ráster:** mecición de exactitud posicional de las seis ortofotos frente a la capa de construcciones catastrales (12 sitios por ortofoto).
4. **Fechado de construcciones por época de aparición**, retrocediendo desde la capa catastral y comparando ortofotos sucesivas, etiquetando los períodos pre-2004, 2004-2010, 2010-2016, 2016-2019, 2019-2021, 2021-2024 y post-2024.
5. **Análisis:** área construida, número de edificaciones y densidad por época, dentro y fuera de las fajas de retiro.

Se trabaja en EPSG:9377 (MAGNA-SIRGAS Origen Nacional). Retiros y ortofotos en EPSG:6257 se reproyectan con la cadena `MAGNA_To_WGS_1984_1 + MAGNA-SIRGAS_2018_to_WGS_1984_1`.

## Resultados parciales de calidad

### Auditoría ráster

Se midió la exactitud posicional de las ortofotos con respecto a las construcciones catastrales, utilizando 12 puntos de validación.

| Ortofoto | RMSE (m) | dx medio (m) | dy medio (m) |
|---|---|---|---|
| 2004 | 1.41 | -0.40 | -0.19 |
| 2010 | 1.06 | -0.01 | -0.61 |
| 2016 | 0.80 | 0.21 | -0.46 |
| 2019 | 0.77 | 0.22 | -0.44 |
| 2021 | 0.78 | 0.28 | -0.34 |
| 2024 | 0.79 | 0.22 | -0.47 |

Las ortofotos de 2016 a 2024 presentan RMSE entre 0,77 y 0,80 m, con componentes medias de desplazamiento similares entre ellas. Las ortofotos de 2004 y 2010 presentan RMSE de 1,41 m y 1,06 m, respectivamente, lo que indica una menor concordancia posicional con la referencia catastral utilizada. Esta diferencia podría estar relacionada con errores de ortorrectificación u otras fuentes de incertidumbre posicional, pero la validación realizada no permite determinar su causa de forma concluyente.

Considerando el propósito del estudio —determinar la presencia o ausencia de edificaciones a partir de la comparación visual con la capa catastral de construcciones—, se decidió **conservar las ortofotos sin georreferenciación adicional** y documentar los errores medidos como referencia de la incertidumbre posicional. Esta decisión se limita al objetivo de análisis definido y no implica que las ortofotos satisfagan necesariamente un estándar general de exactitud cartográfica.

### Auditoría vectorial

Ver [`informe_calidad_datos.md`](informe_calidad_datos.md).

## Limitaciones conocidas

- La exactitud posicional se midió en 12 sitios del borde consolidado; el error en el interior de la zona de estudio no se midió directamente.
- La validación posicional de las ortofotos mide concordancia entre dos fuentes, no exactitud absoluta. Las construcciones catastrales tienen su propia precisión gráfica (escala 1:1.000).
- El fechado es una interpretación visual y puede variar entre intervalos; cada registro lleva su nivel de confianza.
- Las ortofotos no permiten fechar mejoras en altura; estas heredan la época de la edificación base.
- Las fajas de retiro son la capa normativa (POT) publicada (actualización 2017). **No incorporan** eventuales ajustes por estudios de detalle, por lo que una coincidencia espacial no equivale a incumplimiento.
- La capa de construcciones se sincroniza a diario en la fuente; los resultados corresponden a la copia congelada en la fecha de descarga indicada.

## Estructura del repositorio

```
LICENSE.md
README.md
img/                           # capturas ilustrativas
notebooks/
  raster_processing.ipynb      # descarga y composición de teselas de ortofotos, así como su validación y manipulación (sin datos)
tablas/
  diccionario_datos.tsv
  r_desfases.tsv
  v_hallazgos.tsv
informe_calidad_datos.md
```

## Herramientas

- ArcGIS Pro 3.7.0 (licencia educativa de Esri)
- Python (notebook de descarga)
- Google Sheets (diccionario de datos y hallazgos).

## Licencia y atribución

- **Código** (notebooks, scripts): licencia MIT, ver [`LICENSE`](LICENSE.md).
- **Documentos, tablas e imágenes propias:** CC BY 4.0.
- **Datos de origen:** © Alcaldía de Medellín, CC BY-SA 4.0. Este repositorio no los redistribuye; cualquier derivado que los incluya debe respetar la atribución y la condición de compartir igual.

## Contacto

Cristian Usma · [LinkedIn](www.linkedin.com/in/causmar)
