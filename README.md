# ETL - Sucursales en Rappi

## Proyecto / Próposito:

Este repositorio documenta como obtuve y transformé los datos para realizar el siguiente reporte: [Rappi: Estado actual de su servicio estrella](https://app.powerbi.com/view?r=eyJrIjoiZGUxMDc4YWUtOWYwYi00NGIzLTkyZWMtMTk1NjhlZTAzOTY4IiwidCI6ImY4ODBjYzBjLTM5ODMtNDA1Mi04OTE1LTdlNDcxNzNmZDlhMSJ9)  

El cual forma parte de mi [portfolio](https://augustohuerta.notion.site/e37e6bbe39d84967a21219096fe9c0cc?v=d799e48262644e14b7c915e76520f1f0).

## Datos recopilados:
En este ETL se recopilan 4 dimensiones principales:

1. La calificación 
2. El número de calificaciones.
3. Las opiniones.
4. El precio promedio por producto en el catálogo.
5. El tiempo de delivery promedio.

De cada sucursal listada en Rappi.

Entendiendo como sucursal (O restaurante en este proyecto) la división específica de una cadena de restaurantes en un país específico.

## Archivos importantes:

### Los notebooks:

1. extract_part_ETL_rappi.ipynb: Aquí explico cómo obtuve estos datos. (Ya te adelanto que se usó scrapping con Python).
2. transform_part_ETL_rappi.ipynb: Y aquí cómo realicé su limpieza y transformación.
3. analysis_load_part_ETL_rappi.ipynb: En este analizo mejor esta data. (Estado: Incompleto / descontinuado)

### La data:

Lo siguiente es una breve descripción de los .csv's ubicados en `./csv_files/`

#### paises_rappi.csv:
Descripción: Tabla de 8 rows que relaciona datos importantes de cada país con su id.

| Nombre de la columna         | Tipo de dato | Descripción                                           |
|------------------------------|--------------|-------------------------------------------------------|
| id_pais                      | int64        | El id único de cada país                              |
| nombre                       | str          | El nombre de cada país                                |
| suburl_pais                  | str          | La URL de cada país en la aplicación web                 |
| conversion_dolares_moneda    | float64      | La conversión de 1000 doláres para cada moneda local  |

#### cadenas_restaurantes.csv:
Descripción: Tabla de 13,096 rows que relaciona las cadenas de restaurantes de todo Rappi con cada uno de sus países.

| Nombre de la columna | Tipo de dato | Descripción                         |
|----------------------|--------------|-------------------------------------|
| id_cadena            | int64        | El id único de la cadena           |
| nombre_cadena        | str          | El nombre de la cadena             |
| url_cadena           | str          | La URL de la cadena                |
| id_pais              | int64        | El país asociado a la cadena       |

#### sucursales.csv:
Descripción: Tabla de 44,517 rows que relaciona cada sucursal en Rappi con su respectiva cadena de restaurantes.

| Nombre de la columna   | Tipo de dato | Descripción                                   |
|------------------------|--------------|-----------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                    |
| id_cadena              | int64        | El id de la cadena asociada a la sucursal     |
| url_sucursal           | str          | La URL asociada a la sucursal                 |
| nombre_sucursal        | str          | El nombre de la sucursal                      |
| direccion_sucursal     | str          | La dirección de la sucursal                   |

#### atributos_sucursales_parsed.csv:
Descripción: Tabla de 44,517 rows que relaciona cada sucursal de la tabla anterior y le añade datos sobre su performance en Rappi.
Nota: Esta tabla podría hacer un join con la tabla anterior, al final ambas hablan a nivel sucursales, pero para evitar enviar a github un archivo enorme y pesado, decidí dejarlas divididas.

| Nombre de la columna   | Tipo de dato | Descripción                                                                                                 |
|------------------------|--------------|-------------------------------------------------------------------------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                                                                                  |
| mostraba_opiniones     | bool         | True o False, indica si la cadena registraba opiniones                                                      |
| overral_stars          | float64      | La calificación promedio del 1 al 5 de la sucursal (0 indica que no ha sido calificada aún)                 |
| number_opinions        | int64        | El número de calificaciones registradas en la sucursal (0 indica que no se encontraron opiniones)           |
| tiempo_delivery        | int64        | El tiempo promedio de delivery de la sucursal (0 indica que no se encontraron tiempos de delivery)          |
| tipo_envio             | str          | Indica si la sucursal tenía envío gratuito o no ha sido señalado                                           |
| dollar_mean_price      | float64      | El precio promedio en dólares de los productos listados en el catálogo de la sucursal (0 indica sin productos) |

#### opiniones_sucursales.csv:
Descripción: Tabla de 44,517 rows dividida en 46 columnas, una por cada tipo de opinión posible en Rappi junto con el id de la sucursal.

| Nombre de la columna   | Tipo de dato | Descripción                                           |
|------------------------|--------------|-------------------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                            |
| demás columnas         | float64      | Todas las demás opiniones (45 columnas en total)      |

#### atributos_sucursales_bruto.csv:
Descripción: Tabla de 44,517 rows agrupando todos los metadatos que una sucursal expone en la platforma (opiniones, precios de sus productos, tiempo de delivery, etc)
Nota: De este archivo sale el de `atributos_sucursales_parsed.csv`

| Nombre de la columna   | Tipo de dato | Descripción                                                                                       |
|------------------------|--------------|---------------------------------------------------------------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                                                                        |
| attributes             | list         | Lista que contiene `[calificación_promedio, número de calificaciones, tiempo_entrega, tipo de envío]` |
| prices                 | list         | Lista que contiene los precios de todo el catálogo de dicha sucursal como strings                |
| opinions               | list         | Lista que contiene tuplas con las opiniones y sus porcentajes para dicha sucursal                 |

#### subcatalogos.csv:
Descripción: Tabla de 224 rows que expone todas las URLs posible de un catalogo de un pais.
Nota: Esta tabla no contiene información importante a nivel BI, fue creada mas que todo para facilitar el proceso de scraping.

| Nombre de la columna   | Tipo de dato | Descripción                                    |
|------------------------|--------------|------------------------------------------------|
| id_subcatalogo         | int64        | El id único de este subcatálogo                |
| url_subcatalogos       | str          | Los URLs de cada subcatálogo                   |
| id_pais                | int64        | El id del país correspondiente a este subcatálogo |


#### sucursales_pbi.csv:
Descripción: Tabla de 44,517 rows similar a la del archivo `sucursales.csv` pero con columnas eliminadas y algunos ajustes a los datos.
Nota: En su momento, Power BI tenía problemas al leer el archivo original (decía que algunos ids de sucursales se repetían) así que cree este para solucionar el bug. 

| Nombre de la columna   | Tipo de dato | Descripción                                   |
|------------------------|--------------|-----------------------------------------------|
| id_sucursal            | int64        | El id único de la sucursal                   |
| id_cadena              | int64        | El id de la cadena asociada a la sucursal    |
| nombre_sucursal        | str          | El nombre de la sucursal                     |

# Next steps:
1. Transformar las direcciones del archivo `./csv_files/sucursales.csv` en coordenadas geoespaciales usando la API de [Google Maps](https://developers.google.com/maps/documentation/geocoding)
2. Normalizar correctamente el archivo `./csv_files/opiniones_sucursales.csv`, poniendo en las 43 columnas distintas en 2 llamadas 'Tipo de opinión' y 'valor'.

# Metadatos:
Tiempo tomado: 5 días laborales.