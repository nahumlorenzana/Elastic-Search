Proyecto integrador
# Proyecto NoSQL: Análisis de Amazon Fine Food Reviews con Elasticsearch

Archivo del Dataset
> **Nota:** El archivo original `Reviews.csv` se encuentra en este repositorio
Descripción del Dataset
**Nombre:** Amazon Fine Food Reviews
**Fuente:** [Kaggle - Amazon Fine Food Reviews](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)

**¿En qué consiste?:**
Este dataset consiste en reseñas de alimentos de alta cocina vendidas en Amazon. Los datos abarcan un periodo de más de 10 años, incluyendo todas las reseñas desde octube de 1999 hasta octubre de 2012. Incluye más de 500,000 reseñas dejadas por usuarios sobre diversos productos alimenticios.

La información es valiosa para realizar análisis de sentimientos, minería de texto y entender patrones de comportamiento de los consumidores mediante el análisis de los textos de las reseñas y sus puntuaciones.

Diccionario de Datos
A continuación se describen las columnas (campos) presentes en el dataset y su mapeo en Elasticsearch:

| Campo (CSV) | Tipo de Dato (Elasticsearch) | Descripción |
| `Id` | `integer` / `keyword` | Identificador único de la fila en el dataset original. |
| `ProductId` | `keyword` | Identificador único del producto en Amazon (ASIN). |
| `UserId` | `keyword` | Identificador único del usuario que hace la reseña. |
| `ProfileName` | `text` | Nombre de perfil del usuario. |
| `HelpfulnessNumerator` | `integer` | Número de usuarios que encontraron la reseña útil. |
| `HelpfulnessDenominator` | `integer` | Número total de usuarios que votaron si la reseña era útil o no. |
| `Score` | `integer` / `short` | Calificación del producto (de 1 a 5 estrellas). |
| `Time` | `date` (epoch_millis) | Fecha y hora de la reseña (formato timestamp UNIX). |
| `Summary` | `text` | Resumen breve o título de la reseña. |
| `Text` | `text` | Cuerpo completo de la reseña (texto no estructurado). |

## Modelado del Dataset (Base de Datos Documental).
**Tipo de NoSQL:** Documental (Document Store).

En Elasticsearch, los datos no se almacenan en tablas con filas y columnas rígidas, sino como **Documentos JSON**.

**Estrategia de Modelado:**
Cada fila del archivo CSV original se ha transformado en un único objeto JSON (un documento) indexado dentro de Elasticsearch.

1.  **Estructura JSON:** Se ha aplanado la estructura para mantener la simplicidad, donde cada campo representa una propiedad clave-valor.
2.  **Inverted Index (Índice Invertido):** Para los campos `Summary` y `Text`, Elasticsearch crea un índice invertido. Esto tokeniza las palabras (separa en términos individuales), permitiendo búsquedas de texto completo que no serían eficientes en una base de datos SQL tradicional.
3.  **Keyword vs Text:**
    * Campos como `ProductId` o `UserId` se modelan como `keyword` para permitir filtrado exacto y agregaciones (e.g., "Top 10 productos más comentados").
    * Campos como `Text` se modelan como `text` para permitir análisis lingüístico.

## Herramientas utilizadas.

Kaggle: Plataforma utilizada para descargar el archivo original del dataset.
ElasticSearch: Motor de búsqueda y análisis de datos donde se almacenan y consultan los datos.
Python: Herramienta utilizada para la importacion del dataset a ElasticSearch.
VS Code: Herramienta para instalación de extensiones de comprobación.
Postman Extension: Extensión utilizada en VS Code para verificar que el archivo del dataset se haya importado correctamente.

## Descripcion de proceso de importacion 

Para poder importar el dataset a ElasticSearch se necesito de un proceso largo, se instalo el paquete comprimido de ElasticSearch para Windows que se encuentra en el siguiente link:[InternetShortcut](https://www.elastic.co/downloads/elasticsearch), al descomprimir, se ejercutó en la terminal de PowerShell de Windows utilizando ejecutando la siguiente sentencia:(bin\elasticsearch.bat).
En python se utilizó un codigo para poder pasar todo el dataset a ElasticSearch[importar.py](https://github.com/user-attachments/files/23866958/importar.py). Posteriormente se verifico en el navegador utilizando:[InternetShortcut](http://localhost:9200/amazon_reviews).

## Ejemplo.
**Ejemplo de un documento en Elasticsearch:**
```json
{
  "_index": "amazon_reviews",
  "_id": "1",
  "_source": {
    "ProductId": "B001E4KFG0",
    "UserId": "A3SGXH7AUHU8GW",
    "ProfileName": "delmartian",
    "HelpfulnessNumerator": 1,
    "HelpfulnessDenominator": 1,
    "Score": 5,
    "Time": 1303862400,
    "Summary": "Good Quality Dog Food",
    "Text": "I have bought several of the Vitality canned dog food products and have found them all to be of good quality..."
  }
}
