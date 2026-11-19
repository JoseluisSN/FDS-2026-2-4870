# Creación de conocimiento a partir de datos de tendencias de videos de YouTube (Gran Bretaña - GB)

Trabajo Final (TB2) del curso **Fundamentos de Data Science (1ACC0216)** — Ciclo 2026-02  
**Universidad Peruana de Ciencias Aplicadas (UPC)** | Carrera de Ciencias de la Computación  
**Sección:** 4870 | **Grupo:** 4 | **País asignado:** Gran Bretaña (GB)

## Objetivo del proyecto

Este repositorio documenta el desarrollo de un proyecto de Ciencia de Datos guiado por la metodología **CRISP-DM**. El trabajo parte del análisis de los registros diarios de videos en tendencia de YouTube en Gran Bretaña (GB), con el propósito de extraer hallazgos relevantes, construir y evaluar un modelo predictivo de visualizaciones (`views`) y plantear recomendaciones sustentadas en datos que sirvan de apoyo para una empresa de marketing digital.

## Integrantes

* Jacobo Emmanuel Diaz Beatriz
* Jose Luis Sancho Navarro
* Raul Tomas Bustamante Cruzado

## Descripción del conjunto de datos

Para el análisis se utilizó la versión adaptada del conjunto de datos *Trending YouTube Video Statistics* correspondiente a Gran Bretaña (GB), integrada por dos archivos base:

* `GBvideos.csv`: archivo original que contiene 38,916 registros y 20 columnas con información diaria de videos en tendencia entre el 14 de noviembre de 2017 y el 14 de junio de 2018.
* `GB_category_id.json`: catálogo en formato JSON que asocia cada identificador numérico (`category_id`) con el nombre oficial de su categoría en YouTube.

### Preparación y consideraciones metodológicas

Como resultado de la fase de preparación de datos, se generó el archivo limpio `data/GBvideos_clean.csv` con 38,742 registros y 31 columnas, los cuales corresponden a 3,272 videos únicos. Durante este proceso se eliminaron 174 registros duplicados por par video-día, se imputaron 608 descripciones vacías, se etiquetaron explícitamente los 90 registros cuyo `category_id = 29` no figuraba en el archivo JSON y se crearon 11 variables derivadas relacionadas con el tiempo de publicación y el nivel de interacción.

Es importante señalar que las columnas `state`, `lat`, `lon` y `geometry` fueron incorporadas de manera aleatoria sobre el conjunto de datos original. En consecuencia, su exploración se presenta únicamente como un ejercicio descriptivo de simulación y se excluye tanto del modelado predictivo como de las recomendaciones comerciales.

## Estructura del repositorio

```text
FDS-2026-2-4870/
├── data/
│   ├── GBvideos.csv                  # Conjunto de datos inicial sin modificaciones
│   ├── GB_category_id.json           # Catálogo de categorías de YouTube (GB)
│   └── GBvideos_clean.csv            # Conjunto de datos limpio y preparado para el análisis
├── code/
│   └── upc-2026-02-4870-4-tf.ipynb   # Notebook reproducible con el desarrollo en CRISP-DM
└── README.md
```

## Conclusiones

1. El consumo de tendencias en Gran Bretaña se concentra principalmente en dos categorías: **Music** (35.36% de los registros y 222.8 millones de *likes* en videos únicos) y **Entertainment** (23.37% de los registros y 53.9 millones de *likes*), que en conjunto representan más de la mitad de la presencia total en el periodo observado.
2. Si se consideran únicamente las categorías con un volumen representativo de videos, **Howto & Style** (96.77%) y **Comedy** (96.54%) muestran los niveles más altos de aprobación, mientras que **News & Politics** destaca en generación de conversación con 6.69 comentarios por cada 1,000 vistas. Asimismo, se observó una correlación monótona fuerte entre el número de vistas y la cantidad de comentarios ($\rho_{\text{Spearman}} = 0.8433$).
3. El día de publicación influye de forma marcada en el alcance: los videos publicados los viernes (mediana de 777,061 vistas) y los jueves (732,848 vistas) superan en más del doble la mediana alcanzada por los publicados los sábados (346,555) y domingos (320,510).
4. En la etapa de modelado sobre los 3,272 videos únicos (divididos en 80% para entrenamiento y 20% para prueba para evitar fuga de datos), el algoritmo **Random Forest Regressor** superó claramente a la Regresión Lineal Ridge, alcanzando un $R^2 = 0.9035$ en escala $\log(1 + \text{views})$ y un $R^2 = 0.8278$ en la escala original, con un MAE de 1.67 millones de vistas. Este modelo resulta útil para estimar el alcance de manera contemporánea cuando ya existen métricas de interacción, aunque no puede emplearse como pronóstico previo a la publicación.
5. La distribución casi homogénea de vistas entre los diez estados con mayor volumen (todos ubicados entre 5.0 y 5.5 mil millones) respalda empíricamente el carácter aleatorio de la geografía simulada y justifica no utilizar dichas variables para sustentar decisiones de segmentación real.

## Reproducibilidad

Para ejecutar el proyecto de principio a fin en un entorno local:

1. Clonar el repositorio y ubicarse en la carpeta raíz `FDS-2026-2-4870`.
2. Instalar las librerías necesarias en Python:

   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```

3. Abrir y ejecutar secuencialmente todas las celdas del archivo `code/upc-2026-02-4870-4-tf.ipynb`, el cual trabaja con rutas relativas hacia la carpeta `../data/`.

## Licencia

Este proyecto se distribuye bajo la Licencia MIT. Su contenido puede ser consultado y reutilizado con fines académicos respetando los términos estándar de dicha licencia.