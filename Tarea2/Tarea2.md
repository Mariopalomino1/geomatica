## 1. Definición del problema

* En este ejercicio se trataran de identificar los puntos críticos de arrojo de basuras clandestinos que esten próximos a algún cuerpo de agua, tanto de origen natural como artificial que puedan verse afectados por la proximidad con estos puntos, ya que el arrojo de basuras puede generar enfermedades las cuales pueden ser propagadas por el cuerpo de agua. Finalmente se buscará categorizar la vulnerabilidad del cuerpo de agua en alto, medio y bajo, esto con el fin de priorizar y evaluar que medidas tomar para evitar que estos los cuerpos de agua se vean afectados.

* Para resolver este problema se propone:
  * Primero se le hará un buffer a las canecas de 25 metros y a los contenedores de 50 metros, estos valores son tomados debido a una encuesta realizada a los bogotanos en los que afirman que una distancia mayor a esta es mut lejos para botar la basura.
  * De los puntos críticos que entren dentro de estos buffer serán retirados.
  * Según la OMS un foco de basuras alcanza a esparcir enfermedades y elementos contaminantes en un radio de hasta 400 metros, por lo tanto a cada punto crítico se le hará un buffer de 100 metros.
  * Se distinguiran aquellos puntos que no llegan a afectar ningún cuerpo de agua con los que si afectan.
  * Se hará un conteo para determinar cual cuerpo de agua es afectado por la mayor cantidad de puntos.
  * Teniendo en cuenta este valor maximo, aquellos cuerpos de agua que tengan entre el 100% y el 70% de este valor serán categorizados con alta vulnerabilidad, entre el 69% y 30% de este valor serán categorizados con media vulnerabilidad y menor a 30% serán categorizados con baja vulnerabilidad.
  
## Fuentes de datos

* Capa puntos críticos de arrojo de basuras clandestinos:
  * Descripción: Lugares donde se acumulan residuos sólidos, generando afectación y deterioro sanitario que conlleva la afectación de la limpieza del área, por la generación de malos olores, focos de propagación de vectores, y enfermedades, entre otros.
  * Fuente: https://datosabiertos.bogota.gov.co/dataset/puntos-criticos-de-arrojo-clandestino-de-residuos-bogota-d-c

* Capa de cuerpos de agua:
  * Descripción: Extensión de agua sobre la tierra, de origen natural o artificial representada a través de geometrías tipo polígono. Tiene categorías como río, canal, laguna, humedal, embalse y pantano.
  * Fuente: https://datosabiertos.bogota.gov.co/dataset/cuerpo-de-agua-bogota-d-c

* Capa de contenedores de basura:
  * Descripción: Sistema de disposición temporal de residuos para su recolección mecanizada a través del esquema de aseo del distrito.
  * Fuente: https://datosabiertos.bogota.gov.co/dataset/contenerizacion-bogota-d-c

* Capa de canecas:
  * Descripción: Elemento dispositivo de aseo de alta resistencia al vandalismo para el almacenamiento de residuos producidos por los peatones o transeuntes en el espacio público.
  * Fuente: https://datosabiertos.bogota.gov.co/dataset/caneca-bogota-d-c


## Procesamiento de datos

Primero se cargan los capas 4 capas (Canecas, Contenedores, Puntos críticos y CUerpos de agua)

![Imagen001](Imagenes/1.PNG "Imagen001")

Y con la herramienta Project, se ptoyectan todas las capas a MAGNA_Colombia_Bogota, a las capas necesarias.

![Imagen002](Imagenes/2.PNG "Imagen002")

Después de cargadas las capas, se realiza el buffer de 25 y 50 metros a las capas de canecas y contenedores respectivamente.

![Imagen003](Imagenes/3.PNG "Imagen003")
![Imagen004](Imagenes/4.PNG "Imagen004")

Posterior a esto, mediante la herramienta select by location se seleccionan aquellos puntos criticos que sean afectados por el radio de cobertura de alguna caneca o contenedor de basura y se retiran de la lista de punto criticos. Pasando de tener 747 puntos criticos a 621 puntos criticos 

![Imagen005](Imagenes/5.PNG "Imagen005")
![Imagen006](Imagenes/6.PNG "Imagen006")

Con esta capa con los sitios criticos depurada llamada (Criticos_Depurado) se realizará un buffer de 100 metros y se añadirá un nuevo atributo la capa para distinguir cual punto afecto un cuerpo de agua y cual no. Obteniendo que un total de 133 puntos afectan algún cuerpo de agua.

![Imagen007](Imagenes/7.PNG "Imagen007")

El nuevo atributo recibe el nombre de AFECTA, teniendo valores en texto de SI si llega a afectar un cuerpo de agua y vacio si no afecta un cuerpo de agua

![Imagen008](Imagenes/8.PNG "Imagen008")

Con la Herramienta Spatial Join contaremos cuantos puntos estan dentro (completa o parcialmente) dentro de algún polígono de cuerpo de agua.

![Imagen009](Imagenes/9.PNG "Imagen009")
![Imagen010](Imagenes/10.PNG "Imagen010")

Obteniendo que el cuerpo de agua con mayor cantidad de puntos que le afectan es el Río Tunjuelito con 13 puntos.

Teniendo esto en cuenta se determina que aquellos cuerpos de agua con una cantidad de afección entre 9 y 13 puntos reciben vunerabilidad alta, de 8 a 4 puntos vulnerabilidad media y de 3 a 1 vulnerabilidad baja.

Para poder ilustrarlos de mejor maner se añadirá un nuevo atributo (VULN) en el cual a la capa de cuerpos de agua en el que se le dará el nivel de vulnerabilidad indicado.

Obteniendo 3 en categoria alta, 11 en media y 37 en baja.

![Imagen011](Imagenes/11.PNG "Imagen011")

Para distinguir a los cuerpos de agua se le asignará la siguiente simbología.

![Imagen012](Imagenes/12.PNG "Imagen012")
![Imagen013](Imagenes/13.PNG "Imagen013")

Para los puntos criticos, aquellos que si afecten un cuerpo de agua reciibiran un color para diferenciar a los que no.

![Imagen014](Imagenes/14.PNG "Imagen014")
![Imagen015](Imagenes/15.PNG "Imagen015")

## Cargar documentos a PostGIS

Primero se abre la aplicación de Qgis y se cargan los archivos en .shp usados en el ejercicio
 ![Imagen016](Imagenes/16.PNG "Imagen016")

Después de esto se abre el administrador de bases de datos y entramos a la base de datos de la clase

![Imagen017](Imagenes/17.PNG "Imagen017")

Vamos a la pestaña de Public y cargamos las capas

## Cargar datos a Geoserver

Una vez se entra a geoserver con el usuario y clave dados entramos a la sección de capas

![Imagen018](Imagenes/18.PNG "Imagen018")

Elegimos la opcion de agregar nuevo recurso y seleccionamos la opcion de agregar capa de clase_2020_01:clase_postgis_202001

![Imagen019](Imagenes/19.PNG "Imagen019")

Una vez ahí, hace clic en publicación en las capas nuestras. En este caso son las capas ma_canecas, ma_contenedores, ma_cuerpos_agua y ma_puntos_criticos.

![Imagen020](Imagenes/20.PNG "Imagen020")

## Capa de simbología SLD

A cada una de las capas trabajadas se les da la simbología de la manera elegida

![Imagen02](Imagenes/21.PNG "Imagen021")

Una vez arreglada la simbología se elige estilo, guardar estilo y Archivo SLD

![Imagen022](Imagenes/22.PNG "Imagen022")

Una vez guardado el archivo SLD, en Geoserver hacemos clic en Estilos y agregar nuevo estilo

![Imagen023](Imagenes/23.PNG "Imagen023")

En la nueva ventana asignamos el nombre, seleccionamos que sea SLD y cargamos el archivo guardado antes y validamos el código

![Imagen024](Imagenes/24.PNG "Imagen024")

Y se la asignamos a la capa correspondiente y le damos aplicar

![Imagen025](Imagenes/25.PNG "Imagen025")

## Capa simbologia CSS

Para crear la simbologia CSS hacemos lo mismo que en el caso anterio pero esta vez cambiamos el formato a CSS, elegimos el Style Content y le damos a Generate.

![Imagen026](Imagenes/26.PNG "Imagen026")
![Imagen027](Imagenes/27.PNG "Imagen027")

## Capa simbología YSLD

Para crear la simbologia YSLD hacemos lo mismo que en el caso anterio pero esta vez cambiamos el formato a YSLD, elegimos el Style Content y le damos a Generate.

![Imagen028](Imagenes/28.PNG "Imagen028")
![Imagen029](Imagenes/29.PNG "Imagen029")

## Grupo de capas

Para poder crear el Layer Group se busca la opción de grupo de capas y seleccionamos nuevo grupo

![Imagen030](Imagenes/30.PNG "Imagen030")

En esta nueva ventana elegimos las capas a agrupar y le damos agrupar

![Imagen031](Imagenes/31.PNG "Imagen031")

Enlace del grupo de capas:

http://34.83.176.208:18080/geoserver/wms?service=WMS&version=1.1.0&request=GetMap&layers=ma_grupo_capas&bbox=919462.4375%2C908037.875%2C1059746.75%2C1071691.75&width=658&height=768&srs=EPSG%3A3116&format=application/openlayers


## Conclusiones

Enlace video:

















