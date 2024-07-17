# Valores_Comerciales_Cleaning

<video width="320" height="240" controls>
  <source src="https://github.com/AlvaroVillamizar/Valores_Comerciales_Cleaning/blob/main/Images/Main_function.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


El alcance de este proyecto se centra en el desarrollo de un algoritmo Orientado a Objetos para la limpieza de los datos del DANE, esta base de datos cuenta con 477.000 ofertas de lotes o viviendas categorizadas desde casas, apartamentos, oficinas, fincas, entre otras. Adicionalmente se implementaron modelos predictivos para la evaluación de la precisión de las estimaciones realizadas utilizando la información.  

## Metodología

El proyecto busca identificar las posibles relaciones entre el precio de la vivienda y sus características, enfocándose principalmente en municipios de categoría 4, 5 y 6 de Colombia. La metodología implementada incluye:

1.	**Webscraping:** Se desarrolló un algoritmo que permite obtener los lugares cercanos y la distancia promedio desde un par de coordenadas longitudinales.

2.	**Limpieza:** Se desarrolló un módulo de limpieza que procesa las columnas individuales de los datos utilizando la distancia de Levenshtein para prevenir errores ortográficos cometidos durante la redacción de las ofertas.

3.	**Procesamiento:** Luego se desarrolló el procesamiento de la información, para ello se filtraron los datos que no tuvieron resultados relevantes en el paso anterior y por último se crearan modelos predictivos para estimar el costo de las propiedades.


### Webscraping

<video width="320" height="240" controls>
  <source src="https://github.com/AlvaroVillamizar/Valores_Comerciales_Cleaning/blob/main/Images/Nearby_Places.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

Se diseñó un algoritmo llamado "Nearby_locations.py", el cual utiliza las coordenadas longitudinales de las viviendas para concectarse a la API de Nominatim, por medio de la librer GeoPy en Python, de esta forma se extraen los nombres de los lugares cercanos y sus distancias respectivas para aumentar el número y la calidad de los datos. Las solicitudes realizadas por el algoritmo generan consultas de 1 kilómetro de radio alrededor de las coordenadas para limitar el conjunto de lugares, posteriormente calcula el promedio de las distancias de todos los lugares de igual categoría.

<figure class="image">
<p align="center">
<img src="https://github.com/AlvaroVillamizar/Valores_Comerciales_Cleaning/blob/main/Images/Radio_mapa.png" width="auto" height="auto">

**Por ejemplo:** Si en las coordenadas son: 4.4471 latitud y -69.7975 longitud.

Entonces los lugares cercanos encontrados son: 1 Estación de policía, 1 hospital, 4 Restaurantes, 1 Escuela, 1 Banco, 1 Supermercado, 4 Tiendas, 3 Parques y 1 Iglesia. La cantidad de lugares está limitado a una lista de 17 lugares predefinidos.

<figure class="image">
<p align="center">
<img src="https://github.com/AlvaroVillamizar/Valores_Comerciales_Cleaning/blob/main/Images/Lugares_cercanos_algoritmo.png" width="auto" height="auto">

### Módulo de limpieza

<video width="320" height="240" controls>
  <source src="https://github.com/AlvaroVillamizar/Valores_Comerciales_Cleaning/blob/main/Images/Cleaning_funcs.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

Para realizar la limpieza de los datos, se seleccionaron las columnas más completas para el entrenamiento de los modelos, utilizando el porcentaje de datos faltantes como criterio excluyente. Posteriormente, se creó la clase "DataProcessing", la cual cuenta con varias funciones encargadas de acomodar y transformar sus respectivas variables. Finalmente, se integró la clase mencionada y el algoritmo de web scraping en el código principal para ejecutar la limpieza y la transformación de los datos usando la librería 'Multiprocessing'. El algoritmo está diseñado de modo que el usuario ingrese un número del 1 al 439.526, seleccionando aleatoriamente la cantidad de filas indicada y realizando la respectiva limpieza.


#### Requerimientos

Las librerías utilizadas en el código se describen a continuación:

| **Librerías**           | **Versión** |
|-------------------------|---------|
| sklearn.model_selection | 1.5.0   |
| category_encoders       | 2.6.3   |
| unicodedata             | 1.3.8   |
| fuzzywuzzy              | 0.18.0  |
| geopy                   | 2.4.1   |
| pandas                  | 2.1.4   |
| numpy                   | 1.26.2  |
| json                    | 1.6.3   |

Del módulo de **sklearn** se utilizó la función *model_selection* para la selección y división de datos en conjutnos de entrenamiento y prueba.

El módulo **category_encoders** se utilizó para transformar variables categóricas en formatos adecuados para el modelo, como el one-hot encoding y target encoding.

El módulo **unicodedata** se utilizó para remover tildes y normalizar las cadenas de texto, asegurando consistencia en los nombres de las localidades y otros campos de texto.

Del módulo **fuzzywuzzy** se utilizaron las funciones *fuzz* y *process* para corregir errores humanos en la digitacion de la información en las ofertas de la base de datos. Estas funciones permitieron realizar comparaciones y coincidencias difusas entre textos para identificar y corregir nombres de municipios y departamentos mal escritos.

Del módulo **geopy** se utilizaron las funciones *Nominatim*, *geodesic* y *GeocoderTimedOut* para conectarse al API de OpenStreetMap para obtener información de ubicación (como departamento y barrio) a partir de coordenadas geográficas, luego se calcularon las distancias geográficas entre las coordenadas y los lugares cercanos obtenidos por el módulo de webscrapping, finalmente se implementaron reintentos para evitar la saturación en el API.

Los módulos **pandas**, **numpy** y **json** se utilizaron para el procesamiento y manipulación de la data.
