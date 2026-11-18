# Perfiles de riesgo de anemia infantil en el Perú (ENDES 2025)

Proyecto de **Data Mining** que identifica perfiles de riesgo de anemia en niños de 6 a 59 meses combinando sus medidas (edad, peso, talla, hemoglobina) con las de su madre, y ubica esos perfiles en el territorio peruano.

> **Hito 4 – Resultados finales, discusión y repositorio reproducible**

**Autores:** Gadea · Uchuypoma · Zevallos
<!-- Agrega aquí: curso, universidad, docente y semestre -->

---

## Pregunta de análisis

> ¿Qué perfiles de riesgo de anemia se pueden identificar en niños de 6 a 59 meses combinando sus medidas (edad, peso, talla, hemoglobina) con las de su madre, y dónde se ubican en el territorio?

## Resultados principales

- **La anemia varía mucho entre regiones:** desde ~18 % en Tacna hasta ~39 % en Puno. Es más alta en Puno, Loreto, Huánuco, Cusco y Madre de Dios, y más baja en la costa (Tacna, Callao, Lima, La Libertad).
- **Tres perfiles de riesgo (K-means, k = 3):**

  | Cluster | Perfil | Rasgos principales |
  |---|---|---|
  | 0 | Mayores, madre con más recursos | ~44 meses; madre de mayor talla, peso y educación; mayormente urbano |
  | 1 | Mayores, madre en contexto vulnerable | ~41 meses; madre de menor talla, peso y educación; más rural y a mayor altitud |
  | 2 | **Pequeños (6 a 24 meses)** | ~16 meses; **grupo con mayor anemia** |

- **El contexto materno es un gradiente.** Dentro del grupo de pequeños, la anemia es mayor en zona rural (50 % vs 40 %), con madres de menor educación (51 % vs 33 % con educación superior) y de menor talla (46 % vs 39 %).
- **El promedio regional esconde focos locales.** Hay centros poblados con anemia alta incluso en regiones de tasa baja o media. DBSCAN identificó **18 zonas** de concentración; las mayores están en Ayacucho, San Martín y Huánuco, con más de 50 % de anemia.
- **K-means dice *a quién* afecta; DBSCAN dice *dónde* se concentra.** Ambas miradas se complementan.
- **Limitación:** la silueta es baja y el PCA muestra que los grupos son franjas de una misma nube, no islas separadas. Los perfiles sirven para describir y priorizar, pero la frontera entre ellos es gradual.

## Metodología

| Parte | Contenido |
|---|---|
| **I. Datos** | Carga, limpieza de códigos sentinela, decimal implícito, integración madre–niño y hogar, y corrección/verificación de coordenadas con shapefile |
| **II. Variable anemia** | Tres categorías (*Con anemia*, *Sin anemia*, *Sin clasificación*) y dos mejoras metodológicas |
| **III. K-means** | Comparación de configuraciones con inercia, silueta, Davies-Bouldin, tamaño mínimo de grupo y estabilidad (ARI) |
| **IV. Clustering jerárquico** | Método Ward, dendrograma y corte por salto de distancia de fusión |
| **V. Decisión final** | Elección de k = 3 y perfilado de los grupos |
| **VI. Mapas** | Mapas HTML interactivos con Folium y DBSCAN espacial |
| **VII. Salidas** | Matriz final con clusters, figuras, tablas y mapas |

### Mejoras respecto al Hito 3

1. **No se imputa la hemoglobina.** Los niños sin medición (casi todos menores de 6 meses) ya no entran al modelo con una hemoglobina inventada: la población del clustering son los **niños de 6 a 59 meses con hemoglobina medida**.
2. **Hemoglobina ajustada por altitud** (`HC56A` / `HA56A`; directriz OMS 2024 y RM 251-2024-MINSA). Sin el ajuste, K-means agrupaba en parte por geografía y no por riesgo.

### Variables del clustering (matriz B)

`edad_madre`, `peso_madre`, `talla_madre`, `hb_madre`, `educacion_madre`, `peso_nino`, `talla_nino`, `hb_nino`, `edad_meses` (escaladas con z-score).

La anemia, el sexo y las variables territoriales **no** forman los grupos: solo se usan para describirlos e interpretarlos.

### ¿Por qué k = 3 y no k = 2?

Las métricas internas favorecían k = 2, pero esa solución solo separaba niños pequeños de mayores. Se eligió k = 3 porque:

- el dendrograma Ward muestra un segundo salto claro al pasar de 3 a 2 grupos;
- la estabilidad es alta (ARI ≈ 0,98);
- aporta información adicional a la edad (el contexto materno).

## Estructura del repositorio

```
.
├── Hito4_Final_Gadea_Uchuypoma_Zevallos.ipynb   # Notebook principal
├── README.md
├── requirements.txt
├── RECH0_2025.csv                # Hogares (INEI)     – no incluido, ver "Datos"
├── RECH5_2025.csv                # Mujeres (INEI)     – no incluido, ver "Datos"
├── RECH6_2025.csv                # Niños (INEI)       – no incluido, ver "Datos"
├── peru_shapes/                  # Shapefiles adm0 y adm1 del Perú
├── figuras/                      # Figuras PNG (se generan al ejecutar)
├── tablas/                       # Tablas .csv y .tex (se generan al ejecutar)
├── mapas/                        # Mapas HTML interactivos (se generan al ejecutar)
└── resultados/
    └── matriz_final_clusters.csv # Matriz B sin escalar + cluster y perfil por niño
```

## Datos

Los microdatos provienen de la **Encuesta Demográfica y de Salud Familiar (ENDES) 2025** del INEI, disponibles en el portal de microdatos: <https://proyectos.inei.gob.pe/microdatos/>

| Archivo | Contenido |
|---|---|
| `RECH0_2025.csv` | Datos del hogar (región, área, altitud, UBIGEO, coordenadas) |
| `RECH5_2025.csv` | Mujeres (madres) |
| `RECH6_2025.csv` | Niños |
| `peru_shapes/per_admbnda_adm0_ign_20200714.shp` | Contorno del país |
| `peru_shapes/per_admbnda_adm1_ign_20200714.shp` | Contornos de regiones |

Coloca estos archivos en la raíz del proyecto antes de ejecutar el notebook.

## Cómo reproducirlo

1. Clona el repositorio:
   ```bash
   git clone https://github.com/<tu-usuario>/<tu-repositorio>.git
   cd <tu-repositorio>
   ```
2. Crea un entorno e instala las dependencias:
   ```bash
   python -m venv .venv
   source .venv/bin/activate        # En Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. Agrega los archivos de datos (ver sección anterior).
4. Abre y ejecuta el notebook completo:
   ```bash
   jupyter notebook Hito4_Final_Gadea_Uchuypoma_Zevallos.ipynb
   ```

La semilla es fija (`SEMILLA = 42`), por lo que K-means y las muestras dan siempre el mismo resultado.

## Mapas interactivos

Se generan en la carpeta `mapas/`. Ábrelos en el navegador:

| Archivo | Contenido |
|---|---|
| `m01_centros_anemia_alta.html` | Centros poblados con ≥ 40 % de anemia (mín. 5 niños) |
| `m02_calor_ninos_con_anemia.html` | Mapa de calor de niños con anemia |
| `m04_coropletico_anemia_region.html` | Coroplético: % de anemia por región |
| `m06_dbscan_zonas_anemia_alta.html` | Zonas de concentración detectadas con DBSCAN |
| `m07_mapa_final_capas.html` | Mapa final con todas las capas y selector |

> Las conclusiones territoriales se basan en **porcentajes** y no en densidad, porque los mapas de calor siguen el diseño muestral de la encuesta.

## Tecnologías

Python · pandas · NumPy · GeoPandas · scikit-learn · SciPy · Matplotlib · Seaborn · Folium

## Licencia y uso de datos

<!-- Elige una licencia para el código (por ejemplo MIT) y agrégala como LICENSE. -->
Los datos pertenecen al INEI y se rigen por sus términos de uso.
