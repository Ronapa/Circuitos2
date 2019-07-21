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
  * [Compensación del circuito y respuesta en frecuencia](https://github.com/Ronapa/Circuitos2/#compensación-del-circuito-y-respuesta-en-frecuencia)
  * [Potencia y eficiencia](https://github.com/Ronapa/Circuitos2/#potencia-y-eficiencia)
    * [Potencia en la carga](https://github.com/Ronapa/Circuitos2/#potencia-en-la-carga)
    * [Eficiencia del circuito](https://github.com/Ronapa/Circuitos2/#eficiencia-del-circuito)
    * [Potencia disipada en los transistores](https://github.com/Ronapa/Circuitos2/#potencia-disipada-en-los-transistores)
  * [Ensayos variando la carga](https://github.com/Ronapa/Circuitos2/#ensayos-variando-la-carga)
  * [Mediciones sobre el amplificador](https://github.com/Ronapa/Circuitos2/#mediciones-sobre-el-amplificador)
    * [Impedancia de entrada y salida](https://github.com/Ronapa/Circuitos2/#impedancia-de-entrada-y-salida)
    * [Limitación de corriente y protecciones](https://github.com/Ronapa/Circuitos2/#limitación-de-corriente-y-protecciones)
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

El rol de la etapa de entrada es obtener la diferencia entre la entrada y la realimentación y generar la señal error que se amplifica en el circuito hasta llegar a la salida. Esta es una etapa de amplificación de transconductancia, que según la entrada diferencial impone una corriente en la base de Q23, la etapa de VAS. Es primordial que en esta etapa no se distorsione la señal, y para esto tenemos poder linealizar respecto de nuestro punto de polarización. Con esto en mente justificamos la carga activa para asegurar corrientes iguales de polarización en el par. Si bien existen mejores modelos de fuentes espejo, el resultado obtenido con una fuente espejo simple hacía que no se justificara implementar modelos que dificultarían su implementación y aumentaría el costo del proyecto.  

En una base del diferencial se encuentra la señal de entrada mientras que en la otra se encuentra la realimentación dada por R39 que se conecta a la salida. La elección del valor de R39 se basó en un compromiso de amplificación contra calidad de señal. Con valores más altos de resistencia la amplificación era mayor pero se reducía el ancho de banda y empeoraba la distorsión. Por otro lado, con valores menores de resistencia la realimentación era mayor y se lograba una menor distorsión pero también una menor amplificación. Finalmente se logró reducir la distorsión compensando al circuito y amplificamos para tener una señal máxima que no produjera recorte a la salida.

La etapa de entrada esta cargada con la etapa del VAS, conectado mediante la base de Q23. Este transistor está polarizado con 721uA, lo del cual obtenemos una r_{pi} = 100x26mV/721uA = 3600 Ohms, la cual está en serie con el paralelo de R42 y r_{pi} de Q6. Finalmente, la etapa de entrada está cargada con 4kOhms. 

![Etapa de entrada][Etapa Entrada]

#### Etapa VAS y multiplicador de Vbe
Una vez definida preliminarmente la etapa de salida se prosiguió con el VAS, o Voltage Amplifier Stage. Uno de los primeros inconvenientes encontrados fue que la etapa de salida consumía una corriente excesiva de la etapa de entrada, lo que causaba que los transistores de la segunda etapa entraran en corte y distorsionaran la señal. La solución planteada fue agregar un Darlinton a la salida del par diferencial, de este modo consumía menos corriente de la etapa de entrada. Por la malla del multiplicador de Vbe y por Q8 circula una corriente de 4.4mA de polarización. En señal, esta corriente varía en menos de 20uA, ya que la corriente que entrega la señal a la  etapa de salida está dada por los dos seguidores formados por Q25-Q26 y Q24-Q27. 


![Etapa de VAS][Etapa VAS]

La tensión a la salida de esta etapa, tiene un desnivel de 2Vbe por la caida en los transistores de los seguidores, sin embargo, ajustando el valor de la resistencia R12 logramos que el multiplicador de Vbe equipare este desnivel. Esto se ve en la siguiente imagen, donde se aprecia que esta etapa amplifica tensión y copia la señal a las dos ramas de la salida, con la diferencia de los Vbe necesarios para obtener una señal sin distorsión. 

#### Etapa de salida
La etapa de salida, como se ve en la imagen, es simétrica para el camino de señal del semicilco positivo y del negativo. A continuación se hará un analisis del camino de señal positiva de esta etapa siendo los resultados análogos para el camino de señal negativa.  
Primero, Q11 y Q12 funcionan como llaves, controladas por los circuitos de conmutación. Q11 permanece sin conducir hasta que se lo indica la señal _S\_HIGH_. Una vez que Q11 conduce la tensión en su colector es mayor a la del emisor de Q12, por lo que Q12 entra en corto. De este modo solo conduce una rama a la vez. Esta topología de tener los transistores en paralelo mejora la efciencia, ya que no se tiene conduciendo constantemente al transistor de baja tensión como en un amplificador clase G serie.   
Para la salida de tensión alta se planteó un Darlington, de manera de reducir la corriente consumida por la etapa de salida, lo que producía que los transistores de etapas anteriores distorsionaran la señal. Esto se debe a que la corriente de colector de estos transistores es grande y su _beta_ es chico, del orden de 50, ya que se trata de transistores de potencia. Planteando el darlington se resuelve esta problemática.   
Para la entrada sin señal se obtuvo un valor de -1.6mV a la salida, lo cual es más que aceptable.

Una vez determinada la etapa de salida y con un resultado dentro de los márgenes esperados se agregó una protección de sobrecorriente para proteger a los transistores de corrientes elevadas. Esta protección sensa la tensión sobre el resistor R15 y al superar el valor de aproximadamente 0.7V se enciende el transistor Q29, tomande corriente de la base de la etapa de salida, disminuyendo la corriente de base y por lo tanto la que se dirige a la carga. Como el amplificador esta diseñado para cargas de 4 y 8 Omhs se buscó limitar la corriente a alrededor de 8A. Además el transistor se encuentra polarizado con un LED lo que permite tener un indicador visual de si la protección esta siendo activada. Como se mencionó anteriormente, la etapa del semiciclo negativo fuinciona de manera análoga.

Si bien esta protección resulta útil para proteger los transistores frente a una carga de bajo valor, como por ejemplo 2 Omhs, no resulta del todo eficiente para un corto a la salida. El objetivo es que si se coloca un amplificador de una impedancia incorrecta, o en su defecto 2 amplificadores de 4 Omhs en paralelo, el amplificador no trate de entregar toda la potencia a la carga quemando la etapa de salida. Por como se diseño la protección, frente a un corto en la salida se produce una limitación a una corriente de 16A, lo cual es destructivo para el transistor. Sin embargo, según la hoja de datos el transistor puede tolerar picos de hasta 30A por menos de 1ms. Es por esto que además se colocará en la alimentación un fusible de manera de que llegado el caso de que se produzca un corto a la salida el amplificador no se destruya. Una mejora posible sería plantear un circuito con un rele que abra el circuito de manera de no tener que cambiar un fusible.

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
  Para los tranistores de potencia se buscó unos que fueran montados en un disipador y que soportaran los valores de corriente que entrega el circuito. Finalmente elegimos los transistores **2SC5200**(NPN) y **2SA1943**(PNP). La elección de estos transistores se basó en que cumplian las especificaciones, su modelo de Spice era accesible y se podía simular, se puede comprar en el país, y su precio no era tan elevado como otros transistores de aplicación especifica para audio. Antes de decidirnos por esta tecnología simulamos el circuito y comprobamos que cumpliera el circuito con las especificaciones. 


* [**MJE340**](https://www.onsemi.com/pub/Collateral/MJE340-D.PDF) y  [**MJE350**](https://www.onsemi.com/pub/Collateral/MJE350-D.PDF)  
  Para el multiplicador de Vbe, que debe estar acoplado térmicamente a los transistores de salida, se eligió un transistor que pudiera ser montado en un disipador. Para este propósito se eligió el transistor **MJE340**(NPN). Además, se eligió la misma tecnología para los transistores que forman el la base del Darlington a la salida de alta potencia. Si bien para este último propósito se podría haber utilizado los transistores de potencia, los transistores elegidos cumplieron con los requerimientos a un costo menor que los de potencia. Esto se debe a que la corriente que manejan estos es mucho menor a la que entregan los transistores de potencia. 


### Compensación del circuito y respuesta en frecuencia
Para la compensación del circuito se abrió el lazo realimentador y se clocó una inductancia de un valor muy grande al igual que un capacitor de gran valor en serie con la uente. A partir de un analisis de recuencia se obtuvo la ganancia de lazo, la cual permitió determinar si el sistema era estable. A continuación se presenta el esquemático detallando lo anterior:

![Esquematico margen fase](/Imagenes/esquematico_margen_fase.PNG)

El gráfico obtenido es el siguiente:

![grafico compensacion](/Imagenes/grafico_compensacion.PNG)

Posteriormente se graficó la respuesta en frecuencia para el amplificador diseñado. Se realizó un ACSweep con la fuente de señal configurada con 1V AC. Como se puede ver en el gráfico, el amplificador presenta una respuesta plana entre los 20Hz y los 200kHz para el caso de la ganancia. En cuanto a la fase, en los 20 Hz se produce un adelanto de fase de 10° lo cual si bien no es ideal, debido a que se trata de bajas frecuancias este adelanto de fase no representaría mayores inconvenientes. Para el otro extremo de la banda de audio, para los 20kHz se produce un atraso de fase de menos de 10°, el cual es considerablemente bajo por lo que no se percibirá.

![respuesta en frecuencia](/Imagenes/respuesta_en_frecuencia.PNG)

En resumen, la respuesta en frecuencia resulta adecuada para la aplicación que se planteó.


### Potencia y eficiencia
#### Potencia en la carga
Las especificaciones de potencia planteadas en un prinicpio eran de alrededor de 50Watts sobre una carga de 8Ohms. Para lograr esto tendríamos que  llegar a una excursión de 28Vp sobre la carga. Luego del desarrollo de las etapas y por medio de las simulaciones, la amplitud máxima a la salida previo al recorte de la señal que se logró fue de 24V. Esto equivale a una potencia de 36Watts. Si bien no es lo que nos planteamos en un principio consideramos que es un valor adecuado para un amplificador de audio, más aún teniendo en cuenta que una diferencia del 22% en el valor de la potencia equivale a 1dB de diferencia, por lo que al oído esto no será tan perceptible como uno creería.  

![Potencia 1k][Potencia 1k]

Por otro lado, el circuito consume aproximadamente 500mW cuando no hay senal a la entrada. 

![Potencia 0V][Potencia 0V]
#### Eficiencia del circuito
La eficiencia a la que llegamos, como era de esperarse, depende de la amplitud a la salida y al valor de la carga. Con la amplitud máxima a la salida y una carga de 8Ohms logramos una eficiencia de 77%. Sin embargo, como planteamos antes, los picos de señal en audio son de corta duración, por lo que en la mayor parte de la amplificación los valores serán menores a estos. De este modo se llega a una eficicencia aún mayor. Para una senal senoidal pura, se logra la eficiencia m'axima cuando la amplitud es maxima a la salida, donde se aprovecha la conmutacion entre las dos v'ias de ditinta tension.     

![Eficiencia amplitud maxima][Eficiencia amplitud maxima]  



Por otro lado la eficiencia con la salida en un valor más bajo empeora. A modo de ejemplo se simuló la eficiencia con la salida a 5Vp, que se muestra a continuación.

![Eficiencia 5Vp][Eficiencia 5Vp]

Las tres imágenes anteriores fueron simuladas con una señal de entrada senoidal de 1kHz. Fueron simuladas otras frecuencias y los resultados fueron equivalentes. 

#### Potencia disipada en los transistores

### Ensayos variando la carga. 

### Mediciones sobres el amplificador

### Distorsión

Otro parámetro muy importante en un amplificador es la distorsión armónica que presenta. Par ellos se utilizó el comando del LTSpice .FOUR en el cual se le especifica la frecuencia y la cantidad de armónicas que son tenidas en cuenta para el cálculo de la distorsión. El comando arroja dos valores, de los cuales en todos los casos se tomó el de mayor valor. El valor más bajo corresponde solo a la distorsión armónica mientras que el otro tiene en cuenta todas las otras componentes ajenas a la señal de entrada que son amplificadas. A continuación se encuentran los valores obtenidos para 2 valores de carga y 2 frecuencias distintas. Las potencias indicadas son RMS para la señal pico indicada como Vin.


4 Ohms 

Vin = 167mV | Vout = 2V | THD 1kHz= 0.012% | THD 10kHz= 0.017% | P = 1W

Vin = 526mV | Vout = 9V | THD 1kHz= 0.012% | THD 10kHz= 0.036% | P = 10W

Vin = 832mV |Vout = 14V | THD 1kHz= 0.072% | THD 10kHz= 0.23% | P = 25W

Vin = 1.27V | Vout = 21.3V | THD 1kHz= 0.063% | THD 10kHz= 0.18% | P = 58W

Vin = 1.41V | Vout = 23.65V | THD 1kHz= 0.058% | THD 10kHz= 0.16% | P = 70W


8 Ohms 

Vin = 167mV | Vout = 2V | THD 1kHz= 0.012% | THD 10kHz= 0.015% | P = 0.5W

Vin = 526mV | Vout = 9V | THD 1kHz= 0.012% | THD 10kHz= 0.005% | P = 5W

Vin = 832mV |Vout = 14V | THD 1kHz= 0.074% | THD 10kHz= 0.21% | P = 12.5W

Vin = 1.27V | Vout = 21.3V | THD 1kHz= 0.061% | THD 10kHz= 0.17% | P = 29W

Vin = 1.41V | Vout = 23.65V | THD 1kHz= 0.056% | THD 10kHz= 0.16% | P = 35W

En todos los casos se puede observar que para una tensión de entrada de 830mV aproximadamente la distorsión aumenta notablemente. Esto se debe a que comienza a actuar la conmutación de la etapa de salida. También se aprecia que a medida que aumenta la frecuencia la distorsión introducida por este subcircuito es mayor, ya que se producen más cantidad de transiciones entre los transistores. Se trabajó para eliminar lo más posible los transitorios de conmutación pero no se pudo mejorar más. Es posible que si se hubiese tenido en cuenta una tecnología MOS para la conmutación se pudieran haber obtenido mejores resultados.

#### Impedancia de entrada y salida

Para obtener la impedancia de entrada del amplificador se decidió realizar un barrido en frecuencia y obtener la impedancia de entrada para todas las frecuencias. En la siguiente figura se observa la impedancia de entrada para frecuencias entre 0.1Hz y 10GHz. Se puede apreciar que la impedancia de entrada es 20 kOhms para todo el rango entre los 10 y los 100kHz, lo cual abarca toda la banda de audio. Resulta importante destacar que es una característica deseada en un amplificador de audio.

![Impedancia de entrada](/Imagenes/Rin_frecuencia.PNG)

Para el caso de la impedancia de salida, el método fue muy similar al de la impedancia de entrada. Se simuló para distintos casos en los cuales se conectaron cargas de 4 u 8 Omhs y también con el amplificador sin carga. Debido a que el amplificador se encuentra realimentado, no hubo grandes variaciones. En este caso la impedancia no mantiene un valor constante sino que decae con la frecuencia, desde los 5mOmhs a 20Hz hasta unos 5uOmhs para 20kHz.

![Impedancia de entrada](/Imagenes/Rout_frecuencia.PNG)

#### Limitación de corriente y protecciones

Para el dimensionamiento de la protección de limitación de corrriente se debió considerar las condiciones sobre las que debía operar el amplificador. Como el diseño debe funcinar tanto para 4 como para 8 Omhs, la protección no debía interferir con estas condiciones. Como para una carga de 4Omhs se generaban corrientes de alrededor de 6A, se consideró como valor apropiado una limitación de corriente de 7,5 u 8 A. Como ya se mencionó anteriormente esto se logró polarizando un transistor que toma corriente desde la base de la etapa de salida. A continuación se muestra la corriente que circula por la carga para un valor de 2 Omhs.

![Limitacion corriente](/Imagenes/limitador_corriente_2_omhs.PNG)

Efectivamente se verifica que el limitador de corriente funciona para casos donde la impedancia es menor a la esperada.

Para el caso de que se produzca un corto en la salida se aprecia que la corriente máxima es mayor a 7A, aunque no de manera considerable. Igualmente para estos casos se colocará un fusible en serie con la fuente de manera de proteger el equipo, ya que los transistores se encontrarán disipando potencia de manera constante. Cabe destacar que para que el limitador funcione debe existir una señal a la entrada que provoque en la salida una corriente de alrededor de 8A, ya que en ausencia de señal no circulará corriente a la salida.


![Limitacion corriente corto](/Imagenes/limitador_corriente_corto.PNG)

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
