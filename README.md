# Predicción del Redshift mediante Modelos de Machine Learning

Este repositorio contiene varios modelos de machine learning que se comparan entre sí con el objetivo de predecir el redshift a partir de espectros astronómicos obtenidos del Sloan Digital Sky Survey (SDSS).

## Descripción

El proyecto implementa y evalúa diferentes modelos de machine learning para la predicción del redshift. Los modelos incluidos en este proyecto son:

- **MLP (Multi-Layer Perceptron):** Redes neuronales tradicionales que ayudan a modelar relaciones complejas en los datos.
- **CNN (Convolutional Neural Network):** Redes neuronales convolucionales, que se utilizan para extraer características relevantes de los espectros.

## Estructura del Proyecto

- **/storage:** Carpeta destinada a almacenar los modelos entrenados.
- **/spectrums:** Carpeta que contiene algunos espectros en formato `.fits`, que se utilizan para testear y validar los modelos. Estos archivos son la base a partir de la cual se parte el análisis.

## Uso

1. Clona el repositorio:
   ```
   git clone https://github.com/albertnica/TFGF
   ```
2. Instala las dependencias (Python 3.13.2):
   ```
   pip install -r requirements.txt
   ```
3. Descarga los datasets de entrenamiento preprocesados:\
   [Onedrive](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/ErgQAaw-289HgM3ra2E1VKcBO0YnTupebQfolK2shqnXAg?e=nDuxJh)
   
5. El archivo más visual e interesante es en el que realiza la comparativa:
   ```
   pearson.ipynb
   ```
