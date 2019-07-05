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
* [Resumen](https://github.com/Ronapa/Circuitos2/#resumen)  
* [Diseño](https://github.com/Ronapa/Circuitos2/#diseño)  
  * [Polarización y funcionamiento del circuito](https://github.com/Ronapa/Circuitos2/#polarización-y-funcionamiento-del-circuito)  
    * [Etapa de entrada](https://github.com/Ronapa/Circuitos2/#etapa-de-entrada)  
    * [Etapa VAS y multiplicador de Vbe](https://github.com/Ronapa/Circuitos2/#etapa-vas-y-multiplicador-de-vbe)
    * [Etapa de salida](https://github.com/Ronapa/Circuitos2/#etapa-de-salida)
    * [Switches y conmutación](https://github.com/Ronapa/Circuitos2/#switches-y-conmutación)
  * [Elección de las tencologías utilizadas](https://github.com/Ronapa/Circuitos2/#elección-de-las-tecnologías-utilizadas)
  * [Compensación del circuito y calidad de la señal](https://github.com/Ronapa/Circuitos2/#compensación-del-circuito-y-calidad-de-la-señal)
  * [Potencia y eficiencia](https://github.com/Ronapa/Circuitos2/#potencia-y-eficiencia)
    * [Potencia en la carga](https://github.com/Ronapa/Circuitos2/#potencia-en-la-carga)
    * [Eficiencia del circuito](https://github.com/Ronapa/Circuitos2/#eficiencia-del-circuito)
    * [Potencia disipada en los transistores](https://github.com/Ronapa/Circuitos2/#potencia-disipada-en-los-transistores)
  * [Ensayos variando la carga](https://github.com/Ronapa/Circuitos2/#ensayos-variando-la-carga)
  * [Mediciones sobre el amplificador](https://github.com/Ronapa/Circuitos2/#mediciones-sobre-el-amplificador)
    * [Impedancia de entrada y salida](https://github.com/Ronapa/Circuitos2/#impedancia-de-entrada-y-salida)
    * [Limitación de corriente y protecciones](https://github.com/Ronapa/Circuitos2/#limitación-de-corriente-y-protecciones)
    * [Slew Rate](https://github.com/Ronapa/Circuitos2/#slew-rate)
    * [Rechazo de ruido de la fuente](https://github.com/Ronapa/Circuitos2/#rechazo-de-ruido-de-la-fuente)
    * [Limitaciones sobre los valores de alimentación](https://github.com/Ronapa/Circuitos2/#limitaciones-sobre-los-valores-de-alimentación)
  * [Diseño del PCB](https://github.com/Ronapa/Circuitos2/#diseño-del-pcb)
* [Construcción](https://github.com/Ronapa/Circuitos2/#construcción)

### Resumen
El objetivo de este trabajo fue diseñar un amplificador de audio clase G con la etapa de salida conmutada en paralelo. La principal motivación fué la de mejorar la eficiencia del amplificador ya que el amplificador clase G clásico, posee los dos transistores de ssalida en serie, lo que provoca que cuando está activada la etapa del riel de mayor tensión, la señal también debe circular a través del otro transistor, generando pérdidas de potencia y excursión. Se puso como objetivo diseñar un circuito que amplifica una señal de audio estándar 1VRMS a la máxima tensión posible para cargas de 4 y 8 Ohms. Otra especificación fue la de la potencia, la cual se estimó alrededor de los 75W. El amplificador es alimentado con una fuente de ±30V y una de ±15V. Estos valores de tensión se logran con una fuente de laboratorio de ±30V y una fuente switching que otorga los valores de ±15 necesarios. El circuito cuenta con un LED de encendido, y una protección por sobrecorriente. 

## Diseño
El circuito fue diseñado en base al amplificador [Kenwood KA-7X](http://materias.fi.uba.ar/6610/Manuales%20de%20servicio%20tecnico/Clase%20G%20y%20H/Kenwood/KA-7X/hfe_kenwood_ka_7x_service.pdf). Este es un amplificador _stereo_ clase G con etapa de salida en paralelo. Nuestro amplificador, en cambio, es de tipo _mono_. El circuito diseñado está basado en etapas definidas por el circuito en bloques de la siguiente figura.

![Circuito en bloques][Circuito en bloques]

A continuación en la imagen se muestra una parte de la etapa de salida del amplificador Kenwood, del cual tomamos características circuitales para implementar en nuestro diseño. 

![Amplificador Kenwood][kenwood]


Nuestro circuito simulado se encuentra a continuación:

![Circuito LTSpice][Nuestro Circuito]

Se optó hacer un circuito clase G porque con este logra una elevada eficiencia en amplificación de audio. Esto se debe a que, en audio, los transistores de mayor tensión funcionan en intervalos cortos de tiempo, siendo los de menor señal los que conducen la mayor parte. Esto se debe a que las señales de audio que se reproducen generalmente tienen un alto cociente de valor pico a valor medio, esto es, los momentos de señales altas son breves y pocos. Además, al plantear la etapa de salida con los transistores en paralelo, se logró aumentar aún más la eficiencia, ya que en cualquier momento solo está conduciendo un transistor. En cambio, si la salida estuviera en serie, en los momentos de alta señal conducirían los dos transistores de salida. 

### Polarización y funcionamiento del circuito
#### Etapa de entrada
La etapa de entrada del circuito es un amplificador diferencial con carga activa, estabilizado mediante realimentación por emisor para las diferencias entre los _betas_ de los transistores. Cada rama del par diferencial está polarizada con 500uA, y por Q5 circula una corriente de 1mA. El capacitor C1 se encuentra para filtrar la tensión de continua de la entrada, de este modo el circuito solo amplfica tensiones de señal.  
En una base del diferencial se encuentra la señal de entrada mientras que en la otra se encuentra la realimentación dada por R39 que se conecta a la salida. La elección del valor de R39 se basó en un compromiso de amplificación contra calidad de señal. Con valores más altos de resistencia la amplificación era mayor pero se reducía el ancho de banda y empeoraba la distorsión. Por otro lado, con valores menores de resistencia la realimentación era mayor y se lograba una menor distorsión pero también una menor amplificación. Finalmente se logró reducir la distorsión compensando al circuito y amplificamos para tener una señal máxima que no produjera recorte a la salida.

![Etapa de entrada][Etapa Entrada]

#### Etapa VAS y multiplicador de Vbe
Una vez definida preliminarmente la etapa de salida se prosiguió con el VAS, o Voltage Amplifier Stage. Uno de los primeros inconvenientes encontrados fue que la etapa de salida consumía una corriente excesiva de la etapa de entrada, lo que causaba que los transistores de la segunda etapa entraran en corte y distorsionaran la señal. La solución planteada fue agregar un Darlinton a la salida del par diferencial, de este modo consumía menos corriente de la etapa de entrada. Por la malla del multiplicador de Vbe y por Q8 circula una corriente de 43mA de polarización. En señal, esta corriente no varía mucho, ya que la corriente que entrega la señal a etapa de salida está dada por los dos seguidores formados por Q25-Q26 y Q24-Q27.  


![Etapa de VAS][Etapa VAS]

La tensión a la salida de esta etapa, tiene un desnivel de 2Vbe por la caida en los transistores de los seguidores, sin embargo, ajustando el valor de la resistencia R12 logramos que el multiplicador de Vbe equipare este desnivel.

#### Etapa de salida
La etapa de salida, como se ve en la imagen, es simétrica para el camino de señal del semicilco positivo y del negativo. A continuación se hará un analisis del camino de señal positiva de esta etapa siendo los resultados análogos para el camino de señal negativa.  
Primero, Q11 y Q12 funcionan como llaves, controladas por los circuitos de conmutación. Q11 permanece sin conducir hasta que se lo indica la señal _S\_HIGH_. Una vez que Q11 conduce la tensión en su colector es mayor a la del emisor de Q12, por lo que Q12 entra en corto. De este modo solo conduce una rama a la vez. Esta topología de tener los transistores en paralelo mejora la efciencia, ya que no se tiene conduciendo constantemente al transistor de baja tensión como en un amplificador clase G serie.   
Para la salida de tensión alta se planteó un Darlington, de manera de reducir la corriente consumida por la etapa de salida, lo que producía que los transistores de etapas anteriores distorsionaran la señal. Esto se debe a que la corriente de colector de estos transistores es grande y su _beta_ es chico, del orden de 50, ya que se trata de transistores de potencia. Planteando el darlington se resuelve esta problemática.   
Para la entrada sin señal se obtuvo un valor de -3mV a la salida el cual es más que aceptable.

Una vez determinada la etapa de salida y con un resultado dentro de los márgenes esperados se agregó una protección de sobrecorriente para proteger a los transistores de corrientes elevadas. Esta protección sensa la tensión sobre el resistor R15 y al superar el valor de aproximadamente 0.7V se enciende el transistor Q29, tomande corriente de la base de la etapa de salida, disminuyendo la corriente de base y por lo tanto la que se dirige a la carga. Como el amplificador esta diseñado para cargas de 4 y 8 Omhs se buscó limitar la corriente a alrededor de 8A. Además el transistor se encuentra polarizado con un LED lo que permite tener un indicador visual de si la protección esta siendo activada. Como se mencionó anteriormente, la etapa del semiciclo negativo fuinciona de manera análoga.

Si bien esta protección resulta útil para proteger los transistores frente a una carga de bajo valor, como por ejemplo 2 Omhs, no resulta del todo eficiente para un corto a la salida. El objetivo es que si se coloca un amplificador de una impedancia incorrecta, o en su defecto 2 amplificadores de 4 Omhs en paralelo, el amplificador no trate de entregar toda la potencia a la carga quemando la etapa de salida. Por como se diseño la protección, frente a un corto en la salida se produce una limitación a una corriente de 16A, lo cual es destructivo para el transistor. Sin embargo, según la hoja de datos el transistor puede tolerar picos de hasta 30A por menos de 1ms. Es por esto que además se colocará en la alimentación un fusible de manera de que llegado el caso de que se produzca un corto a la salida el amplificador no se destruya. Una mejora posible sería plantear un circuito con un rele que habra el circuito de manera de no tener que cambiar un fusible.

![Etapa de Salida][Etapa salida]


#### Switches y conmutación
A continuación se encuentra el circuito que controla la conmutación entre el camino de señal alta y el de baja. La salida de este circuito se conecta a la base de Q11 como se planteó en la sección anterior. 


![Etapa conmutacion][Etapa Switches]

Primero se plantea un divisor resistivo con R17 y R18, de donde se obtiene una tensión de referencia. La tensión de referencia simulada es de 11.95V. Se eligió este valor para que la señal no se acerque demasiado a 15V antes de conmutar, dado que esto podía producir que algún transistor sature deformando la señal. Cuando la salida supera este valor, Q13 entra en conducción. Cuando esto ocurre, empieza a circular una corriente sobre  R19 y R20, que a su vez polariza a Q14. Este último es el que toma corriente de la base del switch y así cambiando el circuito de señal.  
La resistencia R26 es un realimentador que muestrea la tensión de la referencia y entrega corriente al transistor Q14 para estabilizar la señal. Finalmente, la rama de C3 y R43 filtra componentes de alta frecuencia dadas por la conmutación rápida. 

![Conmutacion][Conmutación circuitos]

### Elección de las tecnologías utilizadas.
Nuestro primer requisito para seleccionar los transistores necesarios fue que la etapa de entrada tenía que ser de montaje superficial y que los transistores de salida debían poder soportar la corriente a entregar a la carga y pudieran ser montados en un disipador.  
* [**MMBTA56**](https://www.mouser.com/ds/2/149/MMBTA56-889761.pdf) y [**MMBTA06**](https://www.diodes.com/assets/Datasheets/ds30037.pdf)  
  Para la etapa de entrada se eligieron los transistores **MMBTA56**(PNP) y **MMBTA06**(NPN) de Fairchild. Estos son transistores de montaje superficial que cumplen con los requerimientos de corriente y tensión planteados en el circuito. Si bien estos transistores son de propósito general, son de bajo ruido, requisito indispensable para la etapa de entrada. Además, por su capacidad de conmutar entre conducción y corte rapidamente fueron también elegidos para los _switches_ que conmutan los transistores de salida. El _beta_ mínimo de estos transistores es 100, pero el fabricante no otorga valores típicos o máximos. Para asegurar un buen funcionamiento se tuvo que realimentar por emisor en el par diferencia y en al carga activa para apaciguar posibles diferencias en los _betas_. Se ensayaron simulaciones con diferencias de los _beta_ de hasta un 50% e igual se cumplieron las especificaciones planteadas. No obstante, en el armado del circuito se le medirá los valores de _beta_ y se acoplaran aquellos que tengan las menores diferencias. 

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

La eficiencia máxima se logra con una señal de salida de 18Vp, y esta vale 83%, y se muestra a continuación.

![Eficiencia maxima][Eficiencia maxima]

Por otro lado la eficiencia con la salida en un valor más bajo empeora. A modo de ejemplo se simuló la eficiencia con la salida a 5Vp, que se muestra a continuación.

![Eficiencia 5Vp][Eficiencia 5Vp]

Las tres imágenes anteriores fueron simuladas con una señal de entrada senoidal de 1kHz. Fueron simuladas otras frecuencias y los resultados fueron equivalentes. 

#### Potencia disipada en los transistores

### Ensayos variando la carga. 

### Mediciones sobres el amplificador

#### Impedancia de entrada y salida

#### Limitación de corriente y protecciones

#### Slew Rate

#### Rechazo de ruido de la fuente

#### Limitaciones sobre los valores de alimentación


### Diseño del PCB
  
  
## Construcción


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
[Circuito en bloques]: Imagenes/Bloques.png
