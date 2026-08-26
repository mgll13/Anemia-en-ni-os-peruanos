# Perfiles de riesgo nutricional y de anemia en niños menores de 5 años (ENDES 2025)

Proyecto del curso **Data Mining** - Universidad del Pacífico
Docente: Soledad Espezúa (s.espezual@up.edu.pe)

## Descripción del proyecto

La anemia infantil es uno de los problemas de salud pública más persistentes en el Perú, con brechas importantes entre regiones y entre zonas urbanas y rurales. Este proyecto busca caracterizar el estado nutricional y de anemia de niños menores de 5 años a partir de la Encuesta Demográfica y de Salud Familiar (ENDES) 2025, incorporando además el contexto de sus madres (edad, hemoglobina, nivel educativo), para identificar perfiles de riesgo que puedan orientar la priorización de intervenciones de salud (suplementación de hierro, seguimiento nutricional, programas alimentarios).

**A quién beneficia:** entidades de salud pública (MINSA, gobiernos regionales de salud) y programas sociales de primera infancia, que necesitan priorizar recursos limitados hacia los grupos de mayor riesgo en lugar de aplicar intervenciones genéricas a toda la población infantil.

**Objetivo de análisis:** segmentación (clustering) - agrupar niños según la combinación de sus indicadores antropométricos y de hemoglobina, para descubrir perfiles de riesgo no evidentes al mirar cada variable por separado.

**Unidad de análisis:** cada fila de la base final representa un niño menor de 5 años, vinculado a los datos de su madre (par madre-hijo).

## Estructura del repositorio

```
├── README.md                          # Este archivo
├── notebooks/
│   └── Hito1_DM_RECH5_RECH6.ipynb     # Notebook principal: carga, limpieza, integración y visualizaciones
├── DATABASE/                              # Carpeta para los CSV originales (no incluidos, ver más abajo)
└── outputs/
    └── base_integrada_hito1.csv       # Base integrada resultante, lista para el siguiente hito
```

## Fuentes de datos

| | RECH5 — Mujeres de 12 a 49 años | RECH6 — Niños menores de 5 años |
|---|---|---|
| **Dataset** | Cuestionario del Hogar, módulo de mujeres (RECH5) | Cuestionario del Hogar, módulo de niños (RECH6) |
| **Institución responsable** | Instituto Nacional de Estadística e Informática (INEI) - Encuesta Demográfica y de Salud Familiar (ENDES) 2025 | INEI - ENDES 2025 |
| **Enlace de acceso** | [https://proyectos.inei.gob.pe/microdatos](https://proyectos.inei.gob.pe/microdatos/consulta.asp?cmbencuesta=Encuesta+Demogr%E1fica+y+de+Salud+Familiar+-+ENDES&cmbanno=2025&cmbTrimestre=5	)/ |[ https://proyectos.inei.gob.pe/microdatos/](https://proyectos.inei.gob.pe/microdatos/consulta.asp?cmbencuesta=Encuesta+Demogr%E1fica+y+de+Salud+Familiar+-+ENDES&cmbanno=2025&cmbTrimestre=5	) |
| **Ruta de navegación** | El portal no permite un enlace directo de descarga: *Microdatos* → *Consulta por encuestas* → seleccionar la encuesta "Encuesta Demográfica y de Salud Familiar - ENDES", año 2025 → ubicar el módulo de mujeres (RECH5) en la lista de módulos disponibles → descargar en formato CSV. | Misma ruta que RECH5, seleccionando el módulo de niños (RECH6) en vez de mujeres. |
| **Variables principales usadas** | Edad, peso, talla, nivel de hemoglobina, nivel de anemia, nivel educativo | Edad en meses, peso, talla, sexo, nivel de hemoglobina, nivel de anemia |
| **Forma de acceso** | Descarga directa en CSV, de libre acceso, sin registro previo | Descarga directa en CSV, de libre acceso, sin registro previo |

Ambas fuentes se documentan con el diccionario oficial de variables publicado por el INEI junto con cada módulo (`Diccionario_-_RECH5.pdf`, `Diccionario_-_RECH6.pdf`), que define los códigos de captura y los valores sentinela usados para "no medido".

> **Nota:** los archivos CSV originales no se incluyen en este repositorio por su tamaño y por buenas prácticas de control de versiones. Deben descargarse siguiendo la ruta indicada arriba y colocarse en la carpeta `data/` antes de ejecutar el notebook.

## Cómo ejecutar el proyecto

### Opción A — Google Colab (recomendado)

1. Abrir [Google Colab](https://colab.research.google.com/) y cargar `notebooks/Hito1_DM_RECH5_RECH6.ipynb` (`Archivo → Subir notebook`, o directo desde GitHub con `Archivo → Abrir notebook → GitHub` pegando la URL de este repositorio).
2. Descargar `RECH5_2025.csv` y `RECH6_2025.csv` siguiendo la ruta de navegación descrita en la sección **Fuentes de datos**.
3. Subir ambos archivos al entorno de Colab (ícono de carpeta en el panel izquierdo → *Subir*), en el mismo directorio raíz donde corre el notebook.
4. Ejecutar todas las celdas en orden: `Entorno de ejecución → Ejecutar todas`.

### Opción B — Entorno local

```bash
git clone <URL-de-este-repositorio>
cd <nombre-del-repositorio>
pip install -r requirements.txt
jupyter notebook notebooks/Hito1_DM_RECH5_RECH6.ipynb
```

Colocar `RECH5_2025.csv` y `RECH6_2025.csv` (descargados según la sección **Fuentes de datos**) en la carpeta `data/`, y ajustar las rutas de lectura al inicio del notebook si es necesario.

### Dependencias

```
pandas
numpy
plotly
```

## Contenido del notebook

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

## Próximos pasos

- Estandarizar (media 0, desviación 1) las variables numéricas antes de aplicar K-means o PCA.
- Incorporar la ubicación geográfica del hogar (módulo RECH0) para explorar patrones territoriales de anemia.
- Ejecutar K-means con distintos valores de *k*, evaluando con el método del codo o *silhouette score*.
- Interpretar los clusters resultantes cruzándolos con variables de contexto (educación de la madre, edad del niño, región).
