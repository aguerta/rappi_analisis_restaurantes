# Análisis de restaurantes en Rappi

## Resumen:

¿Nunca te has preguntado cuál es el mejor restaurante en Rappi? o ¿Quién tiene los precios más bajos?

Para responder estas preguntas, este repo recopila 5 dimensiones vitales para cada restaurante dentro de la app:
1. La calificación 
2. El número de calificaciones.
3. Las opiniones.
4. El precio promedio por producto en el catálogo.
5. El tiempo de delivery promedio.

¿Cómo? Mediante un ETA (Extract, Transform y Analysis) de toda la data de restaurantes en la plataforma.

Nota: Este código forma parte de un proyecto más grande, el cual puedes más explorar [aquí](https://augustohuerta.notion.site/e37e6bbe39d84967a21219096fe9c0cc?v=d799e48262644e14b7c915e76520f1f0&p=51350da1b8aa40dfa013ac2a32472e1a&pm=c).

## Archivos importantes:    

### Los notebooks:

Breve descripción de los cada paso realizado en el ETA (Extract, Transform y Analysis). Puedes encontrar sus archivos en `./notebooks/`:

1. Extract.ipynb: Cómo obtuve los datos mediante scraping, Python, multiproccesing, Pandas y Numpy.
2. Transform.ipynb: Cómo los limpié y transformé para su posterior análisis.
3. Analysis.ipynb: Breve análisis de la data usando Matplotlib y Seaborn.

### La data:

Breve descripción de los cada csv ubicado en `./csv_files/`:

#### paises_rappi.csv:
Tabla de 8 rows y 4 columnas que relaciona datos importantes de cada país con su id. Esta fue creada en el notebook `Extract`.

| Nombre de la columna         | Tipo de dato | Descripción                                           |
|------------------------------|--------------|-------------------------------------------------------|
| id_pais                      | int64        | El id único de cada país                              |
| nombre                       | str          | El nombre de cada país                                |
| suburl_pais                  | str          | La URL de cada país en la aplicación web              |
| Equivalente_1_dolar          | float64      | La equivalencia de 1 dólar a moneda local             |

#### cadenas_restaurantes.csv:
Tabla de 13,096 rows y 4 columnas que relaciona las cadenas de restaurantes de todo Rappi con cada uno de sus países. Esta fue creada en el notebook `Extract`.

| Nombre de la columna | Tipo de dato | Descripción                        |
|----------------------|--------------|------------------------------------|
| id_cadena            | int64        | El id único de la cadena           |
| nombre_cadena        | str          | El nombre de la cadena             |
| url_cadena           | str          | La URL de la cadena                |
| id_pais              | int64        | El país asociado a la cadena       |

#### sucursales.csv:
Tabla de 44,517 rows y 11 columnas que relaciona cada sucursal con datos sobre su atención al cliente (Tiempo de delivery, tipo de envío, opiniónes, rating, etc). Esta fue creada en el notebook `Transform` a partir de `sucursales_bruto.csv` y `atributos_sucursales_bruto.csv`.

| Nombre de la columna   | Tipo de dato | Descripción                                      |
|------------------------|--------------|--------------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                       |
| id_cadena              | int64        | El id de la cadena asociada a la sucursal        |
| url_sucursal           | str          | La URL asociada a la sucursal                    |
| nombre_sucursal        | str          | El nombre de la sucursal                         |
| direccion_sucursal     | str          | La dirección de la sucursal                      |
| Mostraba opinión       | bool         | Si la sucursal recibió alguna opinión en la App  |
| Overral_stars          | float64      | El número de estrellas que la sucursal recibió   |
| Number_opinions        | int64        | El número de opiniones que la sucursal recibió   |
| Tipo_envío             | str          | Si el envío era gratis u otro                    |
| Min_tiempo_delivery    | int64        | El tiempo mínimo que toma el delivery            |
| Max_tiempo_delivery    | int64        | El tiempo máximo que toma el delivery            |

#### prices_per_sucursal.csv:
Tabla de 1,208,784 rows y 3 columnas que relaciona cada sucursal de la tabla anterior y le añade datos sobre su performance en Rappi. Esta fue creada en el notebook `Transform` a partir de `atributos_sucursales_bruto.csv`.

| Nombre de la columna   | Tipo de dato | Descripción                                                         |
|------------------------|--------------|---------------------------------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                                          |
| Prices                 | float64      | El precio en moneda local de alguno de los productos en la sucursal |
| Num_products           | int64        | Cantidad de productos que reflejaban ese precio en la sucursal      |

#### opinions_per_sucursal.csv:
Tabla de 229,949 rows y 5 columnas que relaciona todas las opiniones que ha recibido cada sucursal y sus características. Esta fue creada en el notebook `Transform` a partir de `atributos_sucursales_bruto.csv`.

| Nombre de la columna   | Tipo de dato | Descripción                                                |
|------------------------|--------------|------------------------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                                 |
| Opinion                | str          | Una de los 22 tipos de opiniones que podría dar un usuario |
| Value                  | float64      | Porcentaje de opiniones* que dieron este tipo de opinión   |
| Type_of_experience     | str          | Clasificación de la opinión en una 7 categorías            |
| Kind_of_feeling        | str          | Si la opinión es positiva, negativa o neutra               |

*Son opiniones y no personas porque un mismo usuario puede dar más de una opinión a un mismmo restaurante.

#### sucursales_bruto.csv:
Tabla de 44,517 rows y 5 columnas que relaciona cada sucursal en Rappi con su respectiva cadena de restaurantes. Esta fue creada en el notebook `Extract`.

| Nombre de la columna   | Tipo de dato | Descripción                                   |
|------------------------|--------------|-----------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                    |
| id_cadena              | int64        | El id de la cadena asociada a la sucursal     |
| url_sucursal           | str          | La URL asociada a la sucursal                 |
| nombre_sucursal        | str          | El nombre de la sucursal                      |
| direccion_sucursal     | str          | La dirección de la sucursal                   |

#### atributos_sucursales_bruto.csv:
Tabla de 44,517 rows y 4 columnas agrupando todos los metadatos que una sucursal expone en la platforma (opiniones, precios de sus productos, tiempo de delivery, etc) en listas de Python. Esta fue creada en el notebook `Extract`.

| Nombre de la columna   | Tipo de dato | Descripción                                                                                       |
|------------------------|--------------|---------------------------------------------------------------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                                                                        |
| attributes             | list         | Lista que contiene `[calificación_promedio, número de calificaciones, tiempo_entrega, tipo de envío]` |
| prices                 | list         | Lista que contiene los precios de todo el catálogo de dicha sucursal como strings                |
| opinions               | list         | Lista que contiene tuplas con las opiniones y sus porcentajes para dicha sucursal                 |

#### subcatalogos.csv:
Tabla de 224 rows y 3 columnas que expone todas las URLs posible de un catalogo de un pais. Esta fue creada en el notebook `Extract`.

Este archivo no contiene información importante a nivel BI, fue creada para facilitar el proceso de scraping.

| Nombre de la columna   | Tipo de dato | Descripción                                    |
|------------------------|--------------|------------------------------------------------|
| id_subcatalogo         | int64        | El id único de este subcatálogo                |
| url_subcatalogos       | str          | Los URLs de cada subcatálogo                   |
| id_pais                | int64        | El id del país correspondiente a este subcatálogo |

# Next steps:
1. Transformar las direcciones del archivo `./csv_files/sucursales.csv` en coordenadas geoespaciales usando la API de [Google Maps](https://developers.google.com/maps/documentation/geocoding)

# Metadatos:
Tiempo tomado: 5 días laborales.