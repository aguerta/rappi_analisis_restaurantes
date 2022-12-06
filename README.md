# ETL - Sucursales en Rappi

## Proyecto / Próposito:

Este repositorio documenta como obtuve y transformé los datos para realizar el siguiente reporte: [Rappi: Estado actual de su servicio estrella](https://app.powerbi.com/view?r=eyJrIjoiZGUxMDc4YWUtOWYwYi00NGIzLTkyZWMtMTk1NjhlZTAzOTY4IiwidCI6ImY4ODBjYzBjLTM5ODMtNDA1Mi04OTE1LTdlNDcxNzNmZDlhMSJ9)  

Este formando parte de mi [portfolio](https://augustohuerta.notion.site/e37e6bbe39d84967a21219096fe9c0cc?v=d799e48262644e14b7c915e76520f1f0).

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

Estos son los significados de cada atributo en los .csv's:

#### Archivos principales:
<ol>
<li>paises_rappi.csv:
    <ul>- id_pais (int64): El id único de cada país</ul>
    <ul>- nombre (str): El nombre de cada país</ul>
    <ul>- suburl_pais (str): La extensión del dominio de cada país</ul>
    <ul>- conversion_dolares_moneda (float64): La conversión de 1000 doláres para cada moneda local.</ul>
</li>

<li>cadenas_restaurantes.csv: 
    <ul>- id_cadena (int64): El id único de la cadena</ul>
    <ul>- nombre_cadena (str): El nombre de la cadena.</ul>
    <ul>- url_cadena (str): La url de la cadena.</ul>
    <ul>- id_pais (int64): El país asociada a la cadena.</ul>
</li>

<li>sucursales.csv.
    <ul>- id_sucursal (int64): El id único de la sucursal</ul>
    <ul>- id_cadena (int64): EL id de la cadena asociada a la sucursal.</ul>
    <ul> - url_sucursal (str): La url asociada a la sucursal.</ul>
    <ul>- nombre_sucursal (str): El nombre de la sucursal.</ul>
    <ul>- direccion_sucursal (str): La dirección de la sucursal.</ul>
</li>

<li>atributos_sucursales_parsed.csv.
    <ul>- id_sucursal (int64): El id único de la sucursal</ul>
    <ul>- mostraba_opiniones (bool): True o False de si la cadena registraba opiniones.</ul>
    <ul>- overral_stars (float64): La calificación promedio del 1 al 5 de la sucursal. (Los 0s significan que la sucursal no fue calificada aún).</ul>
    <ul>- number_opinions (int64): El número de calificaciones registradas en la sucursal. (Los 0s significan que no se encontraron sus números de opiniones).</ul>
    <ul>- tiempo_delivery (int64): El tiempo de delivery promedio dela sucursal. (Los 0s significan que no se encontraron los tiempos de delivery).</ul>
   <ul> - tipo_envio (str): Si la sucursal tenía envío gratuito o aún no había sido señalado. (o encontrado).</ul>
   <ul> - dollar_mean_price: El precio promedio en doláres de todos los productos listados en el catálogo de la sucursal. (Los 0s significa que no mostraba ningún producto).</ul>
</li>

<li>opiniones_sucursales.csv:
    <ul>- id_sucursal (int64): El id único de la sucursal.</ul>
    <ul>- demás columnas (float64): todas las demás opiniones. (Total 45 columnas separadas).</ul></li>
</ol>

#### Archivos secundarios:
<li>atributos_sucursales_bruto.csv: (Los atributos de cada sucursal pero sin limpiar / transformar)
    <ul>| Name of attribute     | Type of data               | Descripción                |
| :---                  |    :----:                 |          ---:             |
| id_sucursal           | int64                     | El id único de la sucursal.               |
| attributes            | list                      | Lista que contiene [calificación_promedio, número de calificaciones, tiempo_entrega, tipo de envío].                  |
| prices            | list                      | Lista que contiene strings de los precios de todo el catálogo de dicha sucursal.                 |
| opinions            | list                      | Lista que contiene en tuplas las opiniones con sus porcentajes de dicha sucursal.                  |</ul>

<li>subcatalogos.csv: (Lista de urls de cada subcatálogo de cadenas de comida de cada país)
    <ul>- id_subcatalogo (int64): El id único de este subcatálogo.</ul>
    <ul>- url_subcatalogos (str): Los urls de cada subcatálogo.</ul>
    <ul>- id_pais (int64): El id del país correspondiente a este subcatálogo.</ul>
</li>

<li>sucursales_pbi.csv: (Tabla de datos especial para el uso con Power Bi)
    <ul>- id_sucursal (int64): El id único de la sucursal</ul>
     <ul>- id_cadena (int64): EL id de la cadena asociada a la sucursal.</ul>
    <ul>- nombre_sucursal (str): El nombre de la sucursal.</ul>
</li>