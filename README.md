# Perfiles de riesgo nutricional y de anemia en niños menores de 5 años (ENDES 2025)

Proyecto del curso **Data Mining** - Universidad del Pacífico
Docente: Soledad Espezúa (s.espezual@up.edu.pe)

## Descripción del proyecto

La anemia infantil es uno de los problemas de salud pública más persistentes en el Perú, con brechas importantes entre regiones y entre zonas urbanas y rurales. Este proyecto busca caracterizar el estado nutricional y de anemia de niños menores de 5 años a partir de la Encuesta Demográfica y de Salud Familiar (ENDES) 2025, incorporando además el contexto de sus madres (edad, hemoglobina, nivel educativo) y del hogar (región, área urbana/rural), para identificar perfiles de riesgo que puedan orientar la priorización de intervenciones de salud (suplementación de hierro, seguimiento nutricional, programas alimentarios).

**A quién beneficia:** entidades de salud pública (MINSA, gobiernos regionales de salud) y programas sociales de primera infancia, que necesitan priorizar recursos limitados hacia los grupos y territorios de mayor riesgo en lugar de aplicar intervenciones genéricas a toda la población infantil.

**Objetivo de análisis:** segmentación (clustering) - agrupar niños según la combinación de sus indicadores antropométricos y de hemoglobina, para descubrir perfiles de riesgo no evidentes al mirar cada variable por separado.

**Unidad de análisis:** cada fila de la base final representa un niño menor de 5 años, vinculado a los datos de su madre (par madre-hijo) y al contexto de su hogar (región, área).

## Estructura del repositorio

```
├── README.md                                  # Este archivo
├── notebooks/
│   ├── Hito1_DM_RECH5_RECH6.ipynb              # Hito 1: carga, limpieza, integración y visualizaciones
│   └── Hito2_DM_Integrada_RECH0_2025.ipynb     # Hito 2: outliers, variables derivadas, discretización, escalamiento e integración de RECH0
├── DATABASE/                                   # Carpeta para los CSV originales (no incluidos, ver más abajo)
└── outputs/
    ├── base_integrada_hito1.csv                # Base integrada del Hito 1, insumo del Hito 2
    └── matriz_analitica_hito2.csv              # Matriz analítica (sin escalar y escalada), lista para PCA/K-means
```

## Fuentes de datos

| | RECH5 — Mujeres de 12 a 49 años | RECH6 — Niños menores de 5 años | RECH0 — Cuestionario del Hogar |
|---|---|---|---|
| **Dataset** | Cuestionario del Hogar, módulo de mujeres (RECH5) | Cuestionario del Hogar, módulo de niños (RECH6) | Cuestionario del Hogar, módulo de vivienda/hogar (RECH0) |
| **Institución responsable** | INEI - Encuesta Demográfica y de Salud Familiar (ENDES) 2025 | INEI - ENDES 2025 | INEI - ENDES 2025 |
| **Enlace de acceso** | [https://proyectos.inei.gob.pe/microdatos](https://proyectos.inei.gob.pe/microdatos/consulta.asp?cmbencuesta=Encuesta+Demogr%E1fica+y+de+Salud+Familiar+-+ENDES&cmbanno=2025&cmbTrimestre=5) | [https://proyectos.inei.gob.pe/microdatos](https://proyectos.inei.gob.pe/microdatos/consulta.asp?cmbencuesta=Encuesta+Demogr%E1fica+y+de+Salud+Familiar+-+ENDES&cmbanno=2025&cmbTrimestre=5) | [https://proyectos.inei.gob.pe/microdatos](https://proyectos.inei.gob.pe/microdatos/consulta.asp?cmbencuesta=Encuesta+Demogr%E1fica+y+de+Salud+Familiar+-+ENDES&cmbanno=2025&cmbTrimestre=5) |
| **Ruta de navegación** | *Microdatos* → *Consulta por encuestas* → ENDES 2025 → módulo de mujeres (RECH5) → descargar en CSV | Misma ruta, seleccionando el módulo de niños (RECH6) | Misma ruta, seleccionando el módulo del hogar (RECH0) |
| **Variables principales usadas** | Edad, peso, talla, nivel de hemoglobina, nivel de anemia, nivel educativo | Edad en meses, peso, talla, sexo, nivel de hemoglobina, nivel de anemia | Región (`HV024`), área urbano/rural (`HV025`), identificador de hogar (`HHID`) |
| **Forma de acceso** | Descarga directa en CSV, libre acceso, sin registro previo | Descarga directa en CSV, libre acceso, sin registro previo | Descarga directa en CSV, libre acceso, sin registro previo |

Las tres fuentes se documentan con el diccionario oficial de variables publicado por el INEI junto con cada módulo (`Diccionario_-_RECH5.pdf`, `Diccionario_-_RECH6.pdf`, `Diccionario_-_RECH0.pdf`), que define los códigos de captura, categorías y los valores sentinela usados para "no medido".

> **Nota:** los archivos CSV originales no se incluyen en este repositorio por su tamaño y por buenas prácticas de control de versiones. Deben descargarse siguiendo la ruta indicada arriba y colocarse en la carpeta `DATABASE/` antes de ejecutar los notebooks.

## Cómo ejecutar el proyecto

### Opción A — Google Colab (recomendado)

**Hito 1:**
1. Abrir [Google Colab](https://colab.research.google.com/) y cargar `notebooks/Hito1_DM_RECH5_RECH6.ipynb`.
2. Descargar `RECH5_2025.csv` y `RECH6_2025.csv` siguiendo la ruta de navegación descrita en **Fuentes de datos**.
3. Subir ambos archivos al entorno de Colab, en el mismo directorio raíz donde corre el notebook.
4. Ejecutar todas las celdas en orden: `Entorno de ejecución → Ejecutar todas`.
5. El notebook exporta `base_integrada_hito1.csv`, insumo del Hito 2.

**Hito 2:**
1. Cargar `notebooks/Hito2_DM_Integrada_RECH0_2025.ipynb`.
2. Descargar `RECH0_2025.csv` siguiendo la misma ruta de navegación (módulo del hogar).
3. Subir `RECH0_2025.csv` y `base_integrada_hito1.csv` (resultado del Hito 1) al entorno de Colab.
4. Ejecutar todas las celdas en orden.

### Opción B — Entorno local

```bash
git clone <URL-de-este-repositorio>
cd <nombre-del-repositorio>
pip install -r requirements.txt
jupyter notebook notebooks/Hito1_DM_RECH5_RECH6.ipynb
```

Colocar `RECH5_2025.csv`, `RECH6_2025.csv` y `RECH0_2025.csv` (descargados según **Fuentes de datos**) en la carpeta `DATABASE/`, y ajustar las rutas de lectura al inicio de cada notebook si es necesario. El Hito 2 requiere además `base_integrada_hito1.csv`, generado al ejecutar el Hito 1.

### Dependencias

```
pandas
numpy
plotly
seaborn
matplotlib
scikit-learn
```

## Contenido del notebook — Hito 1

1. Carga de datos y selección de variables relevantes.
2. Inspección inicial (tamaño, tipos, resumen estadístico).
3. Diagnóstico preliminar de calidad (valores faltantes ocultos tras códigos sentinela, categorías con errores de registro).
4. Definición de la clave de integración y validación de duplicados.
5. Tratamiento de valores inválidos y estandarización de categorías.
6. Plan e integración de fuentes (`merge` auditado con `indicator=True`).
7. Registro de decisiones de limpieza e integración, con su justificación.
8. Visualizaciones iniciales, seleccionadas según el tipo de cada variable.
9. Hallazgos y conclusiones preliminares.
10. Próximos pasos.

## Contenido del notebook — Hito 2

1. Incorporación de `RECH0` (región y área del hogar) a la base integrada, mediante una relación hogar (1) → pares madre-niño (N).
2. Detección de valores atípicos en el nivel de hemoglobina del niño (regla IQR y z-score), con decisión justificada de **no** tratarlos como error, por representar la población de interés del proyecto.
3. Construcción de variables derivadas: `diferencia_hemoglobina` (madre-niño) y `tiene_anemia_bin` (indicador binario de riesgo, para validar clusters posteriormente).
4. Discretización de `Edad en meses del niño` en tres grupos (0-6 / 6-24 / 24-60 meses), con puntos de corte basados en el hallazgo del Hito 1 sobre la ventana etaria de mayor riesgo de anemia.
5. Codificación de variables categóricas: binaria (sexo), ordinal (nivel educativo de la madre, grupo de edad), y decisión justificada de **no** aplicar one-hot encoding a región (25 categorías) dentro de la matriz de clustering.
6. Escalamiento de variables numéricas continuas mediante estandarización z-score (`StandardScaler`), como preparación para PCA y K-means.
7. Construcción de la matriz analítica actualizada (versión sin escalar y versión escalada).
8. Tabla resumen a nivel región (`groupby` por región y área), con tasa de anemia y hemoglobina promedio, para reportar hallazgos geográficos accionables.
9. Verificación final de la matriz (dimensiones, faltantes, tipos de datos).
10. Registro de decisiones de transformación, con su justificación.

## Próximos pasos

- ~~Estandarizar (media 0, desviación 1) las variables numéricas antes de aplicar K-means o PCA.~~ ✅ Completado en el Hito 2.
- ~~Incorporar la ubicación geográfica del hogar (módulo RECH0) para explorar patrones territoriales de anemia.~~ ✅ Completado en el Hito 2.
- Ejecutar PCA sobre la matriz analítica escalada para reducir dimensionalidad y explorar la varianza explicada por componente.
- Ejecutar K-means con distintos valores de *k*, evaluando con el método del codo o *silhouette score*.
- Interpretar los clusters resultantes cruzándolos con variables de contexto (educación de la madre, grupo de edad del niño, región y área de residencia).
- Evaluar si la tabla resumen por región (Hito 2) debe incorporarse como capa adicional de interpretación de los clusters (ej. ¿los clusters de mayor riesgo se concentran en regiones específicas?).
