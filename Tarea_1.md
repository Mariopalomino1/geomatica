## 1. Cual es el problema a tratar?

El problema que se va a tratar será determinar cuales de los parques entregados por la Administración Distrital entre 2017 y 2018 cuentan cuentan con paraderos para libros par así determinar potenciales sitios para la ubicación de nuevos paraderos para libros para fomentar la lectura.

## 2. Por que los datos geográficos ayudan a resolverlo?

Los datos geográficos son esenciales en la solución de este problema ya que al tener un componente espacial se es posible tener la ubiación real de estos elementos y junto a otras capas y diferentes herramientas, es posible realizar diferentes procedimientos para llegar una mejor y más rapida solución.

## 3. Descripción de la solución propuesta.

La solución propuesta es:
+ Cargar las capas de las localidades de Bogotá, lass canchas sinteticas y los paraderos para libros.

+ Realizar un Join entre las capas de localidades y canchas sinteticas para determinar la localidad en la que estan cada cancha.

+ Crear un buffer de 200 metros que sera la cobertura de los paraderos de libros.

+ Determinar mediante un select by location cuales de estas canchas no tienen la cobertura de los paraderos 

+ Determinar mediante un mapa de densidades de los paraderos de libros cuales sitios tienen una mayor y menor cantidad de paraderos sin tener en cuenta el buffer de cobertura para determinar las zonas con menos paraderos.

## 4. Listado detallado de las fuentes de datosseleccionados.

+ Capa de localidades de Bogotá:
  * Dataset:Localidades
  * Feature Class: Localidades
  * Atributos usados: 
  * Link de descarga: https://bogota-laburbano.opendatasoft.com/explore/dataset/poligonos-localidades/export/?flg=es
 
+ Capa de localidades de Paraderos Para Libros:
  * Dataset:Paraderos
  * Feature Class: ppp
  * Atributos usados: 
  * Link de descarga: https://datosabiertos.bogota.gov.co/dataset/paraderos-paralibros-paraparques
  
+ Capa de localidades de Canchas sinteticas:
  * Dataset:Canchas sinteticas
  * Feature Class: canchas
  * Atributos usados: 
  * Link de descarga: https://datosabiertos.bogota.gov.co/dataset/canchas-sinteticas
  
## 5. Geoprocesamiento.


  
  
