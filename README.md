# Predicción del Redshift mediante Modelos de Machine Learning

Este repositorio contiene varios modelos de machine learning que se comparan entre sí con el objetivo de predecir el redshift a partir de espectros astronómicos obtenidos del Sloan Digital Sky Survey (SDSS).

## Descripción

El proyecto implementa y evalúa diferentes modelos de machine learning para la predicción del redshift a partir de datos longitud de onda - flujo (habiendo eliminado el ruido lumínico ambiental previamente). Los modelos incluidos en este proyecto son:

- **MLP (Multi-Layer Perceptron):** Redes neuronales tradicionales que ayudan a modelar relaciones complejas en los datos.
- **CNN (Convolutional Neural Network):** Redes neuronales convolucionales, que se utilizan para extraer características relevantes de los espectros.

## Estructura del Proyecto

- **/data:** Carpeta con los datasets utilizados durante el entrenamiento de los modelos.
- **[/extra](extra):** Carpeta auxiliar usada para la obtención y procesamiento de los datos y para la construcción de los datasets de entrenamiento.
- **[/SciServer](SciServer):** Carpeta con todos los archivos necesarios para que funcionen los comandos del módulo SciServer, para evitar instalaciones del mismo.
- **/spectrums:** Carpeta con varios espectros descargados con [Fits Retriever](<Fits Retriever.ipynb>) que pueden usarse para probar los modelos (última línea del notebook de cualquier modelo).
- **[/storage](storage):** Carpeta destinada a almacenar los modelos entrenados.
- **[Fits Retriever](<Fits Retriever.ipynb>):** Extrae todos los archivos .fits de la base de datos de la SDSS y los descarga.
- **[Fits Selector](<Fits Selector.ipynb>):** Programa clave para la obtención del dataset principal de entrenamiento, creado para seleccionar de manera un poco más uniforme los .fits sobre todo para que se aprovechen mucho más los datos con Z ≥ 4.
- **[Dataset Builder](<Dataset Builder.ipynb>):** Toma todos los .fits de una carpeta y construye el dataset de entrenamiento, que consta únicamente de los datos de fluxes/wavelenghts y de Z, y los guarda en la carpeta /data de [Onedrive](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/EiifkyAiWZ9HqcUwLZppl-4BypXdYoWkSBb3XhzWpCAhqw?e=Zl62pY). Es el archivo estándar que marca como se deben procesar los archivos antes de pasarselos a los modelos, ya que el input debe ser obligatoriamente de 5000 fluxes + 5000 wavelenghts.

<br />

- **[Spectra MLP 600k 1](<Spectra MLP 600k 1.ipynb>), [Spectra MLP 1M 1](<Spectra MLP 1M 1.ipynb>):** Modelo basado en Perceptrones Multicapa.

<br />

- **[Spectra CNN 600k 0](<Spectra CNN 600k 0.ipynb>), [Spectra CNN 1M 0](<Spectra CNN 1M 0.ipynb>):** Modelo básico sustentado en redes convolucionales.
- **[Spectra CNN 600k 1](<Spectra CNN 600k 1.ipynb>), [Spectra CNN 1M 1](<Spectra CNN 1M 1.ipynb>):** Se introduce el concepto de dropout en la capa lineal.
- **[Spectra CNN 600k 2](<Spectra CNN 600k 2.ipynb>), [Spectra CNN 1M 2](<Spectra CNN 1M 2.ipynb>):** Se amplía el número de capas convolucionales.
- **[Spectra CNN 600k 3](<Spectra CNN 600k 3.ipynb>), [Spectra CNN 1M 3](<Spectra CNN 1M 3.ipynb>):** Se introduce un módulo de atención.
- **[Spectra CNN 600k 4](<Spectra CNN 600k 4.ipynb>), [Spectra CNN 1M 4](<Spectra CNN 1M 4.ipynb>):** Se introduce dropout en todas las capas.

<br />

- **[Spectra TRA 600k 1](<Spectra TRA 600k 1.ipynb>), [Spectra TRA 1M 1](<Spectra TRA 1M 1.ipynb>):** Modelo basado en Transformers.

<br />

- **[Spectra Diffusion](<Spectra Diffusion.ipynb>):** Modelo que es capaz de generar espectros de manera eficiente y coherente gracias a los datasets inicialmente presentados y al modelo basado en Transformers.

## Uso

1. Clona el repositorio:
   ```
   git clone https://github.com/albertnica/TFGF-albertnica
   ```
2. Instala todas las dependencias menos SciServer, la cual debería funcionar abriendo el repositorio clonado en tu IDE (Python 3.13.2):
   ```
   pip install -r requirements.txt
   ```
3. Descarga los datasets de entrenamiento preprocesados y los espectros de prueba:\
   - [/data](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/EiifkyAiWZ9HqcUwLZppl-4BypXdYoWkSBb3XhzWpCAhqw?e=Zl62pY)
   - [/spectrums](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/EhBzxXiQb2pOuGz-wNQrxUwBxq-gw6r4Nu20r6s61r3LSg?e=C3JDgY)
   - [/storage](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/EsZm738_E2FKoC7dOV0W8hIB3JHfctzO4ZNtGTlCgmFDTg?e=Sc1KSx)