## 1. Definición del problema y fuentes de datos 

* Título del trabajo: Manejo de la red hospitalaria a raíz de los casos confirmados y potenciales casos de gravedad por COVID-19

* Descrpción y enfoque del trabajo: Con el siguiente ejercicio se busca mostrar como se daria manejo a la red hospitalaria del país (en una posible saturación), asumiendo el escenario en que los casos actuales reportados como leves empeorarán y deberan ser atendidos en un centro médico, de igual manera asumimos que aquellos municipios que no cuentan con un adecuado hospital para el manejo de estos casos, remitiran a sus contagiados al centro medico adecaudo más cercano.

* Mockup:
Una vez iniciadala pagina este será el visor principal en donde se encontraran los datos de:

![Imagen001](Imagenes/1.png "Imagen001")

* En el panel derecho de arriba hacia abajo aparecerán:
 * Estadisiticas generales: En donde estaran los datos totales de contagiados, fallecidos y recuperados.
 * Estadisticas por ciudad/municipio: En esta sección apareceran los mismos datos reportados en estadisticas generales pero discriminados por ciudad o municipio en forma de grafico de barras.
 * Estadisticas por departamento: En esta sección apareceran los mismos datos reportados en estadisticas generales pero discriminados por departamento en forma de grafico de barras.
 
* En el panel izquierdo de arriba hacia abajo aparecerán:
 * Estado sanitario: En donde estaran los datos por ciudad principal del estado en alto, medio o bajo de posible saturación hospitalaria.
 * Hositalizados por ciudad/municiio: En esta sección apareceran las cantidades de hospitalizados por cada zona.
 
 * En el panel central aparecera el mapa de Colombia con la representación volumetrica de ciudad principal con su cantidad de pacientes moderados y graves. Igualmente el área correspondiente tomara el color de su estado sanitario.

* Fuentes de datos:
  * Capa de municipios de Colombia:
    * Titulo: mpio
    * Descripción: Conjunto de poligonos representado los municipios de Colombia.
    * Enlace: https://sites.google.com/site/seriescol/shapes

  * Capa de departamentos de Colombia:
    * Titulo: depto
    * Descripción: Conjunto de poligonos representado los municipios de Colombia.
    * Enlace: https://sites.google.com/site/seriescol/shapes
    
  * Archivo en excel/Capa con los reportes de los contagiados con COVID-19:
    * Titulos: CASOS
    * Descripción: Tabla de excel con el reporte de cada ciudadano reportado como positivo por COVID-19, en el que se especifican los datos personales del ciudadano, estado actual, fecha de contagio, fecha de muerte (en algunos casos), ciudad de viviendo y sus coordenadas.
    * Enlace: https://esri.co/covid-19/    
    
  * Archivo en excel con los centros hospitalarios para atenciòn medica en Colombia:
    * Titulo: Hospitales
    * Descripción: Listado con los centros hospitalarios tanto públicos como privados por departamento y ciudad.
    
## 2. Procesamiento de datos:

* Lo que primero se hará, será cargar los datos en Arcmap, pero esto se hace clic en la opción de Add Data y seleccionamos los archivos .shp correspondientes a la capa de Municipios y Departamentos.

![Imagen002](Imagenes/2.PNG "Imagen002")

* Para subir los puntos guardados en la tabla de excel, hacemos clic en la misma opción de Add Data, pero esta vez seleccionamos el archivo llamado CASOSDEF.

* Una vez abierta la tabla, en la pestaña de List By Source de la tabla de contenidos, hacemos clic derecho y seleccionamos Display XY Data y en la nueva ventana emergente seleccionamos en el campo de X la longitud y en el campo Y la latitud.

![Imagen003](Imagenes/3.PNG "Imagen003")
![Imagen004](Imagenes/4.PNG "Imagen004")

* Una vez cargada la capa con los casos reportados de COVID-19, se iniciara la depuración de los datos, para esto se usara la herramienta de Select By Attributes, para seleccionar aquellos reportes cuyo estado actual sea recuperado, fallecido o asintomático, ya que ningunó de estos dos tipos de pacientes recibirá atención medica.

![Imagen005](Imagenes/5.PNG "Imagen005")

* Dejando por fuera 5517 casos de los 14216 casos iniciales. Con la opción de Export Data, exportamos los 8699 casos restantes se asume que los que esten catalogados como leve pasaran a moderado y tendra de recibir tancion en un centro hospitalario, este cambio lo  hacemos usando la herramienta Field Calculator:

![Imagen006](Imagenes/6.PNG "Imagen006")
![Imagen007](Imagenes/7.PNG "Imagen007")

Dejando en total 8551 casos moderados y 149 graves.

Debido a que todos los casos reportados en las ciudades cuentan con la misma coordenada, se realiza un Summarize para saber cuantos casos hay por cada ciudad o municipio.

![Imagen008](Imagenes/8.PNG "Imagen008")

Dando como resultado la tabla con la suma de los casos reportados en los 215 municipios y ciudades que reportan un caso positivo.

![Imagen009](Imagenes/9.PNG "Imagen009")

Segun la información consultada, las únicas ciudades con algún hospital (público o privado) con la infraestructura necesaria para poder atender casos de COVID-19 son los listados a continuación (la cantidad de hospitales de encuentra entre los parentésis):
 * Leticia (1)
 * Medellín (9)
 * Barranquilla (7)
 * Bogotá (16)
 * Cartagena (4)
 * Tunja (3)
 * Manizales (4)
 * Florencia (2)
 * Popayán (2)
 * Quibdó (1)
 * Chia (2)
 * Neiva (4)
 * Ipiales (2)
 * Pasto (4)
 * Tumaco (1)
 * Cúcuta (8)
 * Armenia (7)
 * Pereira (2)
 * Bucaramanga (4)
 * Sincelejo (2)
 * Ibagué (4)
 * Cali (5)
 * San Andres (1)
 * Santa Marta (1)
 * Villavicencio (2)

Dado que estas son las únicas ciudades que podiran atender casos complicados de COVID-19, las ciudades o municipios que cuenten con algun caso positivo y no tengan un centro medico adecuado, seran transportados a la ciudad con la capacidad sanitaria más cercana, dando como resultado la siguiente tabla:

![Imagen010](Imagenes/10.PNG "Imagen010")

A esta tabla se le agregan los siguentes atributos "Cant_Hos" que será la cantidad de hospitales mencionada más arriba, "Camas" el cual sera el numero de camas que tendran disponibles todos los hospitales de la zona, teniendo en cuenta que en promedio estos hospitales tiene 40 camas para cuidados intensivos, "Pacientes" el cual será la sumatoria de los pacientes y finalemnte el atributo "Estado" el cual será el nivel potencial de saturación de la zona, esta se calculará dividiendo la cantidad de pacientes entre la cantidad de camas. Para valores mayores 0.8 se le catalogará como Grave ya que significa que el 80% o más de las camas estan ocupadas, para valores entre 0.79 y 0.4 se le catalogará como Moderado, ya que entre el 79% y 40% de las camas estan ocupadas y menores al 0.4 serán catalagodas como Leves.

Estos atributos se adicionarán con el botón de Add Field en la tabla de atributos.

![Imagen011](Imagenes/11.PNG "Imagen011")

La cantidad de hospitales las ponemos manualmente activando la opción de Start Editing.

![Imagen012](Imagenes/12.PNG "Imagen012")

La cantidad de camas, los pacientes y el estado final se calculara con la herramiente de Field Calculator

![Imagen013](Imagenes/13.PNG "Imagen013")
![Imagen014](Imagenes/14.PNG "Imagen014")

Finalmente exportamos esta tabla para poder realizar los demas puntos.

## 3. Carto

Para usar la herramienta de Carto, lo primero de hacemos es con la tabla exportada desde Acrgis se guarda de nuevo para que quede en formato .xls

Una vez en la pagina de Carto usamos la opción de Data, después en la opción de New Dataset y elegimos la tabla guardada antes.

![Imagen015](Imagenes/15.PNG "Imagen015")
![Imagen016](Imagenes/16.PNG "Imagen016")
![Imagen017](Imagenes/17.PNG "Imagen017")

Al darle clic a la opción de Create Map, vemos que se ubican los puntos, ya que la infomación geometrica es obtenidad de las columnas de Latitud y Longitud. 

![Imagen020](Imagenes/20.PNG "Imagen020")


Sin embargo buscamos también mostrar la información a nivel de departamentos, por esta razón reorganizamos la tabla para que quede por departamentos, una vez obtenida la tabla la agregamos como una nueva capa y usamos la opción Análisis y en Análisis la opcion de Geocode.

![Imagen018](Imagenes/18.PNG "Imagen018")

Llenamos la información necesaria, pidiendole que haga la georeferenciación usando los nombres de las departamentos listadas en la columna departamento y confirmamos

![Imagen019](Imagenes/19.PNG "Imagen019")

Una vez acabado el proceso, se ve el resultado en la siguiente imagen

![Imagen021](Imagenes/21.PNG "Imagen021")

Como se puede ver no resaltó los departementos de Boyacá, Quindío y a Bogotá, de igual manera al resaltar el departamento de Amazonas, resalto al departamento de Brasil, para corregir estos errores se editará la tabla deirectamente.

En los casos de Boyacá, Quindío y Bogotá se modifico su escritura para que le programa los asociara, pero en el caso del Amazoas, este solo reconocía la parte del Amazonas de Brasil. Por lo tanto no se pudo obtener el resaltdo en esa zona.

Para la respresentación del nivel de saturación de cada ciudad, se representara de forma volumetrica, siendo los casos graves las de mayor tamaño y los casos leves de menor tamaño, diviendo los tamaños en 3 tipos. De igual manera el color también se le asignó por valor, siendo los casos graves de color rojo, los moderados de color amarillo y los leve de verde.

![Imagen022](Imagenes/22.PNG "Imagen022")

Para la represntación de los departamentos, se clasificaron en una rampa de colores dependiendo de la cantidad de pacienes. Para esto se hara la representación por valor, en la columna pacientes. La rampa de colores elegida fue con el sentido de demostrar que todas tendran un potencial de aumentar su cantidad de pacientes, se decidio por dividir en 7 los valores para tener una mayor diferenciación y un nivel de opacidad.

![Imagen023](Imagenes/23.PNG "Imagen023")

Para asiganar la leyenda usamos la opcion legend, en la que asignamos el nombre de cada parte

![Imagen025](Imagenes/25.PNG "Imagen025")
![Imagen026](Imagenes/26.PNG "Imagen026")

Para añadir los paneles lateras usamos la opción de Widgets.
En el primer panel es un resumen de la cantidad de paciente que hay en cada ciudad.

![Imagen027](Imagenes/27.PNG "Imagen027")

Para el segundo panel es un resumen de la cantidad de paciente que hay en cada departamento.

![Imagen028](Imagenes/28.PNG "Imagen028")

Dando como resultado:

![Imagen024](Imagenes/24.PNG "Imagen024")

URL: https://mariopalomino1.carto.com/builder/6d661a64-3d8e-4ce0-b0aa-855931b27191/embed

## 4. Geonode


 
















