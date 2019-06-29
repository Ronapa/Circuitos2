# Trabajo Práctico de Diseño
## Diseño de Circuitos Electronicos (86.10) - Primer cuatrimestre 2019
## Diseño de un Circuito amplificador clase G paralelo.
#### Integrantes: 
#####  Rodrigo Nahuel Parra - Padrón: 98785 - email: [ro.nahuel95@gmail.com](mailto:ro.nahuel95@gmail.com)
#####  Gonzalo Gilces Duran - Padrón: 99074 - email: [gonzalo.gilces@hotmail.com](mailto:gonzalo.gilces@hotmail.com)
#####  Segundo Molina Abeniacar - Padrón: 99306 - email: [segun_m@hotmail.com](mailto:segun_m@hotmail.com)
#### Tutor:
##### Edgardo Marchi - email: [edgardo.marchi@gmail.com](mailto:edgardo.marchi@gmail.com)

#### Índice
* [Resumen](https://github.com/Ronapa/Circuitos2/blob/master/README.md#resumen)  
* [Diseño](https://github.com/Ronapa/Circuitos2/blob/master/README.md#diseño)  
  * [Polarización y funcionamiento del circuito](https://github.com/Ronapa/Circuitos2/blob/master/README.md#polarización-y-funcionamiento-del-circuito)  
    * [Etapa de entrada](https://github.com/Ronapa/Circuitos2/blob/master/README.md#etapa-de-entrada)  
    * [Etapa VAS y multiplicador de Vbe](https://github.com/Ronapa/Circuitos2/blob/master/README.md#etapa-vas-y-multiplicador-de-vbe)
    * [Etapa de salida](https://github.com/Ronapa/Circuitos2/blob/master/README.md#etapa-de-salida)
    * [Switches y conmutación](https://github.com/Ronapa/Circuitos2/blob/master/README.md#switches-y-conmutación)
  * [Elección de las tencologías utilizadas](https://github.com/Ronapa/Circuitos2/blob/master/README.md#elección-de-las-tecnologías-utilizadas)
  * [Compensación del circuito y calidad de la señal](https://github.com/Ronapa/Circuitos2/blob/master/README.md#compensación-del-circuito-y-calidad-de-la-señal)
  * [Potencia y eficiencia](https://github.com/Ronapa/Circuitos2/blob/master/README.md#potencia-y-eficiencia)
    * [Potencia en la carga](https://github.com/Ronapa/Circuitos2/blob/master/README.md#potencia-en-la-carga)
    * [Eficiencia del circuito](https://github.com/Ronapa/Circuitos2/blob/master/README.md#eficiencia-del-circuito)
    * [Potencia disipada en los transistores](https://github.com/Ronapa/Circuitos2/blob/master/README.md#potencia-disipada-en-los-transistores)
  * [Ensayos variando la carga](https://github.com/Ronapa/Circuitos2/blob/master/README.md#ensayos-variando-la-carga)
  * [Mediciones sobre el amplificador](https://github.com/Ronapa/Circuitos2/blob/master/README.md#mediciones-sobre-el-amplificador)
    * [Impedancia de entrada y salida](https://github.com/Ronapa/Circuitos2/blob/master/README.md#impedancia-de-entrada-y-salida)
    * [Limitación de corriente y protecciones](https://github.com/Ronapa/Circuitos2/blob/master/README.md#limitación-de-corriente-y-protecciones)
    * [Slew Rate](https://github.com/Ronapa/Circuitos2/blob/master/README.md#slew-rate)
    * [Rechazo de ruido de la fuente](https://github.com/Ronapa/Circuitos2/blob/master/README.md#rechazo-de-ruido-de-la-fuente)
    * [Limitaciones sobre los valores de alimentación](https://github.com/Ronapa/Circuitos2/blob/master/README.md#limitaciones-sobre-los-valores-de-alimentación)
  * [Diseño del PCB](https://github.com/Ronapa/Circuitos2/blob/master/README.md#diseño-del-pcb)
* [Construcción](https://github.com/Ronapa/Circuitos2/blob/master/README.md#construcción)

### Resumen
El objetivo de este trabajo fue diseñar un amplificador de audio clase G con la etapa de salida conmutada en paralelo. Se logró diseñar un circuito que amplifica una señal de 1Vp a una de 25Vp para una carga de hasta 8Ohms. El circuito simulado entrega una potencia máxima al parlante de 78 Watts, con una distorsión menor al 0.1%. El circuito cuenta con un LED de encendido, una limitación de corriente que protege al circuito y a la carga y protecciones que protegen al circuito de una conexión de tensión a la salida. La tensión de salida con entrada nula se simuló y se obtuvo un valor de 3mV. Al tener los transistores de la etapa de potencia en paralelo se logró aumentar la eficiencia, logrando que se conduzca un solo transistor a la vez. Esto se logró con un circuito de control que conmuta los transistores según una tensión de referencia. El circuito del amplificador es alimentado con una fuente de ±30V y una de ±15V. Estos valores de tensión se logran con una fuente de laboratorio de ±30V y una fuente switching que otorga los valores de ±15 necesarios. 

## Diseño
El circuito fue diseñado en base al amplificador [Kenwood KA-7X](http://materias.fi.uba.ar/6610/Manuales%20de%20servicio%20tecnico/Clase%20G%20y%20H/Kenwood/KA-7X/hfe_kenwood_ka_7x_service.pdf). Este es un amplificador _stereo_ clase G con etapa de salida en paralelo. Nuestro amplificador, en cambio, es de tipo _mono_. A continuación en la imagen se muestra una parte de la etapa de salida del amplificador Kenwood.

![Amplificador Kenwood][kenwood]


Nuestro circuito simulado se encuentra a continuación:

![Circuito LTSpice][Nuestro Circuito]

Elegimos hacer un circuito clase G porque con este logra una elevada eficiencia en amplificación de audio. Esto se debe a que, en audio, los transistores de mayor tensión funcionan en intervalos cortos de tiempo, siendo los de menor señal los que conducen la mayor parte. Esto se debe a que las señales de audio que se reproducen generalmente tienen un alto cociente de valor pico a valor medio, esto es, los momentos de señales altas son breves y pocos. Además, al plantear la etapa de salida con los transistores en paralelo, logramos aumentar aún más la eficiencia, ya que en cualquier momento solo está conduciendo un transistor. En cambio, si la salida estuviera en serie, en los momentos de alta señal conducirían los dos transistores de salida. 

### Polarización y funcionamiento del circuito
#### Etapa de entrada
La etapa de entrada del circuito es un amplificador diferencial con carga activa, estabilizado mediante realimentación por emisor para las diferencias entre los _betas_ de los transistores. Cada rama del par diferencial está polarizada con 500uA, y por Q5 circula una corriente de 1mA. El capacitor C1 se encuentra para filtrar la tensión de continua de la entrada, de este modo el circuito solo amplfica tensiones de señal.  
En una base del diferencial se encuentra la señal de entrada mientras que en la otra se encuentra la realimentación dada por R39 que se conecta a la salida. La elección del valor de R39 se basó en un compromiso de amplificación contra calidad de señal. Con valores más altos de resistencia la amplificación era mayor pero se reducía el ancho de banda y empeoraba la distorsión. Por otro lado, con valores menores de resistencia la realimentación era mayor y se lograba una menor distorsión pero también una menor amplificación. Finalmente logramos reducir la distorsión compensando al circuito y amplificamos para tener una señal máxima que no produjera recorte a la salida.

![Etapa de entrada][Etapa Entrada]

#### Etapa VAS y multiplicador de Vbe
Uno de los primeros problemas con que nos planteamos fue que la etapa de salida estaba pidiendo demasiada corriente de la etapa de entrada, lo que causaba que los transistores de la segunda etapa entraran en corte y distorsionaran la señal. Resolvimos esto poniendo un Darlinton a la salida del par diferencial, de este modo consumía menos corriente de la etapa de entrada. Por la malla del multiplicador de Vbe y por Q8 circula una corriente de 43mA de polarización. En señal, esta corriente no varía mucho, ya que la corriente que entrega la señal a etapa de salida está dada por los dos seguidores formados por Q25-Q26 y Q24-Q27.  


![Etapa de VAS][Etapa VAS]

#### Etapa de salida
La etapa de salida, como se ve en la imagen, es simétrica para el camino de señal del semicilco positivo y del negativo. A continuación se hará un analisis del camino de señal positiva de esta etapa siendo los resultados análogos para el camino de señal negativa.  
Primero, Q11 y Q12 funcionan como llaves, controladas por los circuitos de conmutación. Q11 permanece sin conducir hasta que se lo indica la señal _S\_HIGH_. Una vez que Q11 conduce la tensión en su colector es mayor a la del emisor de Q12, por lo que Q12 entra en corto. De este modo solo conduce una rama a la vez. Esta topología de tener los transistores en paralelo mejora la efciencia, ya que no se tiene conduciendo constantemente al transistor de baja tensión como en un amplificador clase G serie.   
Para la salida de tensión alta se planteó un Darlington, esto es porque sino los transistores de tensión alta estaban demandando demasiada corriente sobre por su base, lo que producía que los transistores de etapas anteriores distorsionaran la señal. Esto se debe a que la corriente de colector de estos transistores es grande y su _beta_ es chico. Planteando el darlington se resuelve esta problemática.   
Para la entrada sin señal se obtuvo un valor de -5mV a la salida. Si bien este valor debería ser cero, consideramos que entra dentro de las especificaciones planteadas.  
Por otro lado, el transistor Q29 funciona como un limitador de corriente. Este mide la corriente que va a la carga mediante R15 y cuando ésta toma un valor suficientemente elevado el transistor desvía corriente que iría a la etapa de salida. El valor de esta corriente máxima se encuentra más adelante en la sección de ensayos con distintas cargas. 


![Etapa de Salida][Etapa salida]


#### Switches y conmutación
A continuación se encuentra el circuito que controla la conmutación entre el camino de señal alta y el de baja. La salida de este circuito se conecta a la base de Q11 como se planteó en la sección anterior. 

explico como funciona y pongo una imagen que muestre como conmuta y se apaga uno y conduce el otro. explicar tension de referencia

![Etapa conmutacion][Etapa Switches]

Primero se plantea un divisor resistivo con R17 y R18, de donde se obtiene una tensión de referencia. La tensión de referencia simulada es de 11.95V. Elegimos este valor para que la señal no se acerque demasiado a 15V antes de conmutar, dado que esto podía producir que algún transistor sature deformando la señal. Cuando la salida supera este valor, Q13 entra en conducción. Cuando esto ocurre, empieza a circular una corriente sobre  R19 y R20, que a su vez polariza a Q14. Este último es el que toma corriente de la base del switch y así cambiando el circuito de señal.  
La resistencia R26 es un realimentador que muestrea la tensión de la referencia y entrega corriente al transistor Q14 para estabilizar la señal. Finalmente, la rama de C3 y R43 filtra componentes de alta frecuencia dadas por la conmutación rápida. 

![Conmutacion][Conmutación circuitos]

### Elección de las tecnologías utilizadas.
Nuestro primer requisito para seleccionar los transistores necesarios fue que la etapa de entrada tenía que ser de montaje superficial y que los transistores de salida debían poder aguantar la corriente a entregar a la carga y pudieran ser montados en un disipador.  
* [**MMBTA56**](https://www.mouser.com/ds/2/149/MMBTA56-889761.pdf) y [**MMBTA06**](https://www.diodes.com/assets/Datasheets/ds30037.pdf)  
  Para la etapa de entrada elegimos los transistores **MMBTA56**(PNP) y **MMBTA06**(NPN) de Fairchild. Estos son transistores de montaje superficial que cumplen con los requerimientos de corriente y tensión planteados en el circuito. Si bien estos transistores son de propósito general, son de bajo ruido, requisito indispensable para la etapa de entrada. Además, por su capacidad de conmutar entre conducción y corte rapidamente fueron también elegidos para los _switches_ que conmutan los transistores de salida. El _beta_ mínimo de estos transistores es 100, pero el fabricante no otorga valores típicos o máximos. Para asegurarnos un buen funcionamiento se tuvo que realimentar por emisor en el par diferencia y en al carga activa para apaciguar posibles diferencias en los _betas_. Se ensayaron simulaciones con diferencias de los _beta_ de hasta un 50% e igual se cumplieron las especificaciones planteadas. No obstante, en el armado del circuito se le medirá los valores de _beta_ y se acoplaran aquellos que tengan las menores diferencias. 

* [**2N3055** y **MJ2955**](https://www.onsemi.com/pub/Collateral/2N3055-D.PDF)  
  Para los tranistores de potencia se buscó unos que fueran montados en un disipador y que soportaran los valores de corriente que entrega el circuito. Finalmente elegimos los transistores **2N3055**(NPN) y **MJ2955**(PNP). La elección de estos transistores se basó en que cumplian las especificaciones, su modelo de Spice era accesible y se podía simular, se puede comprar en el país, y su precio no era tan elevado como otros transistores de aplicación especifica para audio. Antes de decidirnos por esta tecnología simulamos el circuito y comprobamos que cumpliera el circuito con las especificaciones. 


* [**MJE340**](https://www.onsemi.com/pub/Collateral/MJE340-D.PDF) y  [**MJE350**](https://www.onsemi.com/pub/Collateral/MJE350-D.PDF)  
  Para el multiplicador de Vbe, que debe estar acoplado térmicamente a los transistores de salida, se eligió un transistor que pudiera ser montado en un disipador. Para este propósito se eligió el transistor **MJE340**(NPN). Además, se eligió la misma tecnología para los transistores que forman el la base del Darlington a la salida de alta potencia. Si bien para este último propósito se podría haber utilizado los transistores de potencia, los transistores elegidos cumplieron con los requerimientos a un costo menor que los de potencia. Esto se debe a que la corriente que manejan estos es mucho menor a la que entregan los transistores de potencia. 


### Compensación del circuito y calidad de la señal
Como compensamos y rta en frecuencia

### Potencia y eficiencia
#### Potencia en la carga
Las especificaciones de potencia planteadas en un prinicpio era de 100Watts sobre una carga de 8Ohms. Para lograr esto tendríamos que  llegar a una excursión de 28Vp sobre la carga. Con las simulaciones, la amplitud máxima a la salida antes que se recortara la señal que logramos es de 25V. Esto significa una potencia de 78Watts. Si bien no es lo que nos planteamos en un principio consideramos que es un valor adecuado para un amplificador de audio. Sin embargo, una diferencia del 22% en el valor de la potencia equivale a 1dB de diferencia, por lo que al oído esto no presenta una grán distancia.   

![Potencia 1k][Potencia 1k]
#### Eficiencia del circuito
La eficiencia a la que llegamos, como era de esperarse, depende de la amplitud a la salida. Con la amplitud máxima a la salida logramos una eficiencia de 75%. Sin embargo, como planteamos antes, los picos de señal en audio son de corta duración, por lo que en la mayor parte de la amplificación los valores serán menores a estos. De este modo se llega a una eficicencia aún mayor.    

![Eficiencia amplitud maxima][Eficiencia amplitud maxima]  

La eficiencia máxima se logra con una señal de salida de xxVp, y esta vale xx%, y se muestra a continuación.

![Eficiencia maxima][Eficiencia maxima]

Por otro lado la eficiencia con la salida en un valor más bajo empeora. A modo de ejemplo se simuló la eficiencia con la salida a 5Vp, que se muestra a continuación.

![Eficiencia 5Vp][Eficiencia 5Vp]

Las tres imágenes anteriores fueron simuladas con una señal de entrada senoidal de 1kHz. Fueron simuladas otras frecuencias y los resultados fueron equivalentes. 

#### Potencia disipada por los transistores

### Ensayos variando la carga. 


### Diseño del PCB
  
[kenwood]: Imagenes/kenwood.png
[Nuestro Circuito]: Imagenes/Circuito_completo.png
[Etapa Entrada]: Imagenes/Etapa_entrada.png
[Etapa VAS]: Imagenes/Etapa_VAS.png
[Etapa Salida]: Imagenes/Etapa_salida.png
[Etapa Switches]: Imagenes/switch.png
[Conmutación circuitos]: Imagenes/Conmutacion.png
[Eficiencia amplitud maxima]: Imagenes/Amplitud_maxima.png
[Potencia 1k]: Imagenes/Potencia_1k.png
[Eficiencia 5Vp]: Imagenes/Eficiencia_5vp.png
[Eficiencia maxima]: Imagenes/Eficiencia_maxima.png
