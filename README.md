# Trabajo Práctico de Diseño
## Diseño de Circuitos Electronicos (86.10)
### Diseño de un Circuito amplificador clase G paralelo.
####  Rodrigo Nahuel Parra
####  Gonzalo Gilces Duran
####  Segundo Molina Abeniacar


##### Resumen
El objetivo de este trabajo fue diseñar un amplificador de audio clase G con la etapa de salida conmutada en paralelo. Se logró diseñar un circuito que amplifica una señal de 1Vp a una de 25Vp para una carga de hasta 8Ohms. El circuito simulado entrega una potencia máxima al parlante de 75 Watts, con una distorsión menor al 0.1%. El circuito cuenta con un LED de encendido, una limitación de corriente que protege al circuito y a la carga y protecciones que protegen al circuito de una conexión de tensión a la salida. La tensión de salida con entrada nula se simuló y se obtuvo un valor de 3mV. Al tener los transistores de la etapa de potencia en paralelo se logró aumentar la eficiencia, logrando que se conduzca un solo transistor a la vez. Esto se logró con un circuito de control que conmuta los transistores según una tensión de referencia. El circuito del amplificador es alimentado con una fuente de ±30V y una de ±15V. Estos valores de tensión se logran con una fuente de laboratorio de ±30V y una fuente switching que otorga los valores de ±15 necesarios. 

#### Desarrollo
El circuito fue diseñado en base al amplificador [Kenwood KA-7X](http://materias.fi.uba.ar/6610/Manuales%20de%20servicio%20tecnico/Clase%20G%20y%20H/Kenwood/KA-7X/hfe_kenwood_ka_7x_service.pdf). Este es un amplificador _stereo_ clase G con etapa de salida en paralelo. Nuestro amplificador, en cambio, es de tipo _mono_. A continuación en la imagen se muestra una parte de la etapa de salida del amplificador Kenwood.
![Amplificador Kenwood][kenwood]


Nuestro circuito simulado se encuentra a continuación:
![Circuito LTSpice][Nuestro Circuito]

##### Elección de las tecnologías utilizadas.
Nuestro primer requisito para seleccionar los transistores necesarios fue que la etapa de entrada tenía que ser de montaje superficial y que los transistores de salida debían poder aguantar la corriente a entregar a la carga y pudieran ser montados en un disipador.  
* [**BC860C**](https://assets.nexperia.com/documents/data-sheet/BC849_BC850.pdf) y [**BC850C**](https://assets.nexperia.com/documents/data-sheet/BC859_BC860.pdf)  
  Para la etapa de entrada elegimos los transistores **BC860C**(PNP) y **BC850C**(NPN) de Phillips. Estos son transistores de montaje superficial que cumplen con los requerimientos de corriente y tensión planteados en el circuito. Si bien estos transistores son de propósito general, son de bajo ruido, requisito indispensable para la etapa de entrada. Además, por su capacidad de conmutar entre conducción y corte fueron también elegidos para los _switches_ que conmutan los transistores de salida. El _beta_ de estos transistores sin embargo puede variar entre 420 y 800, por lo que se tuvo que realimentar por emisor en el par diferencia y en al carga activa para apaciguar estas diferencias. Se ensayaron simulaciones con diferencias de los _beta_ de hasta un 50% e igual se cumplieron las especificaciones planteadas. No obstante, en el armado del circuito se le medirá los valores de _beta_ y se acoplaran aquellos que tengan las menores diferencias. 

* [**2N3055** y **MJ2955**](https://www.onsemi.com/pub/Collateral/2N3055-D.PDF)  
  Para los tranistores de potencia se buscó unos que fueran montados en un disipador y que soportaran los valores de corriente que entrega el circuito. Finalmente elegimos los transistores **2N3055**(NPN) y **MJ2955**(PNP). La elección de estos transistores se basó en que cumplian las especificaciones, su modelo de Spice era accesible y se podía simular, se puede comprar en el país, y su precio no era tan elevado como otros transistores de aplicación especifica para audio. Antes de decidirnos por esta tecnología simulamos el circuito y comprobamos que cumpliera el circuito con las especificaciones. 


* [**MJE340**](https://www.onsemi.com/pub/Collateral/MJE340-D.PDF) y  [**MJE350**](https://www.onsemi.com/pub/Collateral/MJE350-D.PDF)  
  Para el multiplicador de Vbe, que debe estar acoplado térmicamente a los transistores de salida, se eligió un transistor que pudiera ser montado en un disipador. Para este propósito se eligió el transistor **MJE340**(NPN). Además, se eligió la misma tecnología para los transistores que forman el la base del Darlington a la salida de alta potencia. Si bien para este último propósito se podría haber utilizado los transistores de potencia, los transistores elegidos cumplieron con los requerimientos a un costo menor que los de potencia. Esto se debe a que la corriente que manejan estos es mucho menor a la que entregan los transistores de potencia. 
[kenwood]: 
[Nuestro Circuito]:
