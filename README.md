# Predicción del Redshift mediante Modelos de Machine Learning

Este repositorio contiene varios modelos de machine learning que se comparan entre sí con el objetivo de predecir el redshift a partir de espectros astronómicos obtenidos del Sloan Digital Sky Survey (SDSS).

## Descripción

El proyecto implementa y evalúa diferentes modelos de machine learning para la predicción del redshift a partir de datos longitud de onda - flujo (habiendo eliminado el ruido lumínico ambiental previamente). Los modelos incluidos en este proyecto son:

- **MLP (Multi-Layer Perceptron):** Redes neuronales tradicionales que ayudan a modelar relaciones complejas en los datos.
- **CNN (Convolutional Neural Network):** Redes neuronales convolucionales, que se utilizan para extraer características relevantes de los espectros.

## Estructura del Proyecto

- **/storage:** Carpeta destinada a almacenar los modelos entrenados.
- **/extra:** Carpeta auxiliar usada para la obtención y procesamiento de los datos y para la construcción de los datasets de entrenamiento.
- **Fits Retriever:** Extrae todos los archivos .fits de la base de datos de la SDSS y los descarga.
- **Fits Selector:** Programa clave para la obtención del dataset principal de entrenamiento, creado para seleccionar de manera un poco más uniforme los .fits sobre todo para que se aprovechen mucho más los datos con Z >= 4.
- **Spectra Processing:** Toma todos los .fits de una carpeta y construye el dataset de entrenamiento, que consta únicamente de los datos de fluxes/wavelenghts y de Z, y los guarda en la carpeta /data de [Onedrive](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/ErgQAaw-289HgM3ra2E1VKcBO0YnTupebQfolK2shqnXAg?e=nDuxJh). Es el archivo estándar que marca como se deben procesar los archivos antes de pasarselos a los modelos, ya que el input debe ser obligatoriamente de 5000 fluxes + 5000 wavelenghts.
- **Pearson:** Aquí se puede observar como se comportan todos los modelos entrenados tanto con su dataset de entrenamiento como con todo el conjunto de datos del SDSS.

## Uso

1. Clona el repositorio:
   ```
   git clone https://github.com/albertnica/TFGF-abertnica
   ```
2. Instala las dependencias (Python 3.13.2):
   ```
   pip install -r requirements.txt
   ```
3. Descarga los datasets de entrenamiento preprocesados:\
   [Onedrive](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/ErgQAaw-289HgM3ra2E1VKcBO0YnTupebQfolK2shqnXAg?e=nDuxJh)
   
4. El archivo más visual e interesante es en el que realiza la comparativa:
   ```
   pearson.ipynb
   ```