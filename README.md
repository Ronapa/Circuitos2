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
    * [Fuentes switching](https://github.com/Ronapa/Circuitos2/#fuentes-switching)
    * [Análisis de la amplificación](https://github.com/Ronapa/Circuitos2/#análisis-de-la-amplificación)
  * [Elección de las tencologías utilizadas](https://github.com/Ronapa/Circuitos2/#elección-de-las-tecnologías-utilizadas)
  * [Compensación del circuito y respuesta en frecuencia](https://github.com/Ronapa/Circuitos2/#compensación-del-circuito-y-respuesta-en-frecuencia)
  * [Potencia y eficiencia](https://github.com/Ronapa/Circuitos2/#potencia-y-eficiencia)
    * [Potencia en la carga](https://github.com/Ronapa/Circuitos2/#potencia-en-la-carga)
    * [Eficiencia del circuito](https://github.com/Ronapa/Circuitos2/#eficiencia-del-circuito)
    * [Potencia disipada en los transistores](https://github.com/Ronapa/Circuitos2/#potencia-disipada-en-los-transistores)
  * [Ensayos variando la carga](https://github.com/Ronapa/Circuitos2/#ensayos-variando-la-carga)
  * [Simulaciones sobre el amplificador](https://github.com/Ronapa/Circuitos2/#simulaciones-sobre-el-amplificador)
    * [Impedancia de entrada y salida](https://github.com/Ronapa/Circuitos2/#impedancia-de-entrada-y-salida)
    * [Limitación de corriente y protecciones](https://github.com/Ronapa/Circuitos2/#limitación-de-corriente-y-protecciones)
    * [Rechazo de ruido de la fuente](https://github.com/Ronapa/Circuitos2/#rechazo-de-ruido-de-la-fuente)
    * [Limitaciones sobre los valores de alimentación](https://github.com/Ronapa/Circuitos2/#limitaciones-sobre-los-valores-de-alimentación)
  * [Diseño del PCB](https://github.com/Ronapa/Circuitos2/#diseño-del-pcb)
* [Especificaciones](https://github.com/Ronapa/Circuitos2/#especificaciones)
* [Construcción](https://github.com/Ronapa/Circuitos2/#construcción)
  * [Implementación de la etapa de entrada](https://github.com/Ronapa/Circuitos2/#implementación-de-la-etapa-de-entrada)
* [Problemas y plan](https://github.com/Ronapa/Circuitos2/#problemas-y-plan)  
### Resumen
El objetivo de este trabajo fue diseñar un amplificador de audio clase G con la etapa de salida conmutada en paralelo. La principal motivación fué la de mejorar la excursión del amplificador ya que el amplificador clase G clásico, posee los dos transistores de salida en serie, lo que provoca que cuando está activada la etapa del riel de mayor tensión, la señal también debe circular a través del otro transistor, generando disipaciones de potencia innecesaria y pérdida de excursión al tener la caída de colector-emisor de dos transistores. Se puso como objetivo diseñar un circuito que amplifica una señal de audio estándar 1VRMS a la máxima tensión posible para cargas de 4 y 8 Ohms. Otra especificación fue la de la potencia, la cual se estimó alrededor de los 45-50W para una carga de 8Ohms. El amplificador es alimentado con una fuente de ±30V y una de ±15V. Estos valores de tensión se logran con una fuente de laboratorio de ±30V y una fuente switching que otorga los valores de ±15 necesarios. El circuito cuenta con un LED de encendido, y una protección por sobrecorriente. 

## Diseño
El circuito fue diseñado en base al amplificador [Kenwood KA-7X](http://materias.fi.uba.ar/6610/Manuales%20de%20servicio%20tecnico/Clase%20G%20y%20H/Kenwood/KA-7X/hfe_kenwood_ka_7x_service.pdf). Este es un amplificador _stereo_ clase G con etapa de salida en paralelo. Nuestro amplificador, en cambio, es de tipo _mono_. El circuito diseñado está basado en etapas definidas por el circuito en bloques de la siguiente figura.

![Circuito en bloques][Circuito en bloques]


Nuestro circuito simulado se encuentra a continuación:

![Circuito LTSpice][Nuestro Circuito]

Se optó hacer un circuito clase G porque con este logra una elevada eficiencia en amplificación de audio. Esto se debe a que, en audio, los transistores de mayor tensión funcionan en intervalos cortos de tiempo, siendo los de menor señal los que conducen la mayor parte. Esto se debe a que las señales de audio que se reproducen generalmente tienen un alto cociente de valor pico a valor medio, esto es, los momentos de señales altas son breves y pocos. Además, al plantear la etapa de salida con los transistores en paralelo, se logró aumentar aún más la eficiencia, ya que en cualquier momento solo está conduciendo un transistor. En cambio, si la salida estuviera en serie, en los momentos de alta señal conducirían los dos transistores de salida. 

### Polarización y funcionamiento del circuito
#### Etapa de entrada
La etapa de entrada del circuito es un amplificador diferencial con carga activa, estabilizado mediante realimentación por emisor para las diferencias entre los _betas_ de los transistores. Cada rama del par diferencial está polarizada con 500uA, y por Q5 circula una corriente de 1mA. El capacitor C1 se encuentra para filtrar la tensión de continua de la entrada, de este modo el circuito solo amplfica tensiones de señal.  

El rol de la etapa de entrada es obtener la diferencia entre la entrada y la realimentación y generar la señal error que se amplifica en el circuito hasta llegar a la salida. Esta es una etapa de amplificación de transconductancia, que según la entrada diferencial impone una corriente en la base de Q23, la etapa de VAS. Es primordial que en esta etapa no se distorsione la señal, y para esto tenemos poder linealizar respecto de nuestro punto de polarización. Con esto en mente justificamos la carga activa para asegurar corrientes iguales de polarización en el par. Si bien existen mejores modelos de fuentes espejo, el resultado obtenido con una fuente espejo simple hacía que no se justificara implementar modelos que dificultarían su implementación y aumentaría el costo del proyecto.  

En una base del diferencial se encuentra la señal de entrada mientras que en la otra se encuentra R39 que se conecta a la salida. Esta resistencia cumple dos funciones, por un lado polariza al circuito de entrada y por otro junto con R9 tiene realimentación para señal. El análisis de realimentación se encuentra en la sección [Análisis de la amplificación](https://github.com/Ronapa/Circuitos2/#análisis-de-la-amplificación). 

La etapa de entrada esta cargada con la etapa del VAS, conectado mediante la base de Q23. Este transistor está polarizado con 721uA, lo del cual obtenemos 

![equation](https://latex.codecogs.com/gif.latex?r_%7Bpi%7D%20%3D%20%5Cfrac%7B100%5Ccdot%2026mV%7D%7B721%5Cmu%20A%7D%20%3D%203600%20%5COmega) , 

la cual está en serie con el paralelo de R42 y r_{pi} de Q6 multiplicado por beta, aproximadamente 4kOhms. Finalmente, la etapa de entrada está cargada con 7.6kOhms. 

![Etapa de entrada][Etapa Entrada]

#### Etapa VAS y multiplicador de Vbe
Una vez definida preliminarmente la etapa de salida se prosiguió con el VAS, o Voltage Amplifier Stage. Uno de los primeros inconvenientes encontrados fue que la etapa de salida consumía una corriente excesiva de la etapa de entrada, lo que causaba que los transistores de la segunda etapa entraran en corte y distorsionaran la señal. La solución planteada fue agregar un Darlinton a la salida del par diferencial, de este modo consumía menos corriente de la etapa de entrada. Por la malla del multiplicador de Vbe y por Q8 circula una corriente de 4.4mA de polarización. En señal, esta corriente varía en menos de 20uA, ya que la corriente que entrega la señal a la  etapa de salida está dada por los dos seguidores formados por Q25-Q26 y Q24-Q27. 


![Etapa de VAS][Etapa VAS]

La tensión a la salida de esta etapa, tiene un desnivel de 2Vbe por la caida en los transistores de los seguidores, sin embargo, ajustando el valor de la resistencia R12 logramos que el multiplicador de Vbe equipare este desnivel. Esto se ve en la siguiente imagen, donde se aprecia que esta etapa amplifica tensión y copia la señal a las dos ramas de la salida, con la diferencia de los Vbe necesarios para obtener una señal sin distorsión. 


![Diferencia_VAS][Diferencia_VAS]

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
La resistencia R26 es un realimentador que muestrea la tensión de la referencia y entrega corriente al transistor Q14 para estabilizar la señal. Finalmente, la rama de C3 filtra componentes de alta frecuencia dadas por la conmutación rápida. 

![Conmutacion][Conmutación circuitos]

#### Fuentes switching

Las fuentes switching están planteadas para entregar potencia a los transistores de menor potencia. Se eligió esta tecnología por la eficicencia que tiene para bajar la tensión de 30V de la fuente de laboratorio a los 15V que necesitamos en el circuito. En la siguiente imagen se muestra el circuito que compone a la fuente switching, el cual fue simulado en el entorno [Webench Power Designer de Texas Instruments.](https://webench.ti.com/power-designer/switching-regulator)

![Fuente switching][fuente switching]

En las siguientes imagenes se puede ver como al conmutar se tiene un transitorio que se extingue dejando finalmente una señal triangular, al rededor del valor que queremos obtener. 


![switching load transient][switching load transient]


![switching input transient][switching input transient]

La siguiente imagen muestra el estado estacionario de la fuente conmutada, donde se ve una señal triangular de 60mVpico. 

![switching steady][switching steady]

Este ruido en la fuente podría ser reflejado en la señal de salida, sin embargo las entradas de las fuentes switching al circuito tienen un capacitor que filtra las componentes de alta frecuencia. Finalmente, para la señal restante que entra al circuito, si bien no es perfectamente plana, mediante simulaciones se comprobó que variaciones menores a 1V en las fuentes no se reflejan en la salida. 

A estas especificaciones, además, se le agregó la consideración del costo. Armar una fuente switching es más barato y eficiente que circuitos más complejos que modulen la señal de modo de no tener nada de variaciones en la salida. 

#### Análisis de la amplificación

Para hacer un análisis de la amplificación, primero separamos por etapas, según el diagrama en bloques de la [figura](https://github.com/Ronapa/Circuitos2#dise%C3%B1o). De acuerdo a esto obtuvimos la amplificación de las distintas etapas. Para empezar, la etapa de salida como gana corriente la amplificación en tensión es aproximadamente 1. La amplificación de la etapa de entrada se calcula como 

![equation](https://latex.codecogs.com/gif.latex?a_1%20%3D%20-gm%5Ccdot%20Rca) 

donde Rca es la resistencia que carga a la etapa de entrada. De acuerdo a esto la amplificación de la etapa de entrada es |a1|=230.

La segunda etapa por otro lado, debería estar cargada por la etapa de salida. Sin embargo, como se mencionó en la sección de [Etapa VAS y multiplicador de Vbe](https://github.com/Ronapa/Circuitos2/#etapa-vas-y-multiplicador-de-vbe) se agregaron los circuitos seguidores entre estas etapas para que la etapa de salida no demandara demasiada corriente de esta etapa. De acuerdo a esto, tomando a la impedancia que presenta el multiplicador de Vbe como muy baja, se obtiene que esta etapa está cargada con la *ro* de la fuente de corriente dada por Q8.  La amplificación de esta etapa está dada por

![equation](https://latex.codecogs.com/gif.latex?a_2%20%3D%20-gm%5Ccdot%20Rca)  

donde la resistencia que de carga es ![equation](https://latex.codecogs.com/gif.latex?r_o%20%3D%20%5Cfrac%7BVA%7D%7BI_%7BCQB%7D%7D). Finalmente obtenemos que el valor de la amplificación de la etapa del VAS es |a2| = 1080. 

Luego, la amplificación a lazo abierto vale ![equation](https://latex.codecogs.com/gif.latex?%7Ca%7C%20%3D%20%7Ca1%7C%5Ccdot%20%7Ca2%7C%20%5Ccdot%20%7Ca3%7C%20%3D%20230%20%5Ccdot%201080%20%5Ccdot%201%20%3D%20248400%20%3D%20107dB)

La realimentación planteada en nuestro circuito es serie-paralelo, esto es, mide tensión y suma tensión. El realimentador consta de una resistencia conectada directamente a la salida, que se conecta con la terminal negativa del amplificador diferencial a través de un divisor resistivo. 

![Realimentador][Realimentador]

La elección de usar este tipo de realimentación fue por la motivación de querer estabilizar el valor de la tensión a la salida. De acuerdo a esto, podemos introducirle cargas al circuito menores a una carga límite y mantener una excursión de señal máxima y con la distorsión limitada. Los valores de la realimentación fueron planteado de la siguiente manera: Para empezar, R39 y R9 debían formar un divisor resistivo de modo de copiar el valor de la salida a la entrada con la misma amplitud. Además, para polarizar el circuito debía estar balanceado en las bases de los transistores. Finalmente, los valores de resistencia debían ser valores comerciales. Con esto planteado encontramos que la mejor solución fue elegir una resistencia de 19kOhms con una de 1.2kOhms. Con estos valores, y una salida máxima de 24V, llegamos a que el valor realimentado es de

![equation](https://latex.codecogs.com/gif.latex?24V%20%5Ccdot%20%5Cfrac%7B1.2k%5COmega%7D%7B1.2k%5COmega&plus;19k%5COmega%7D%20%3D%201.42V)  .

Finalmente, tenemos que la amplificación es 1/f, donde f es el valor de la realimentación, dada por ![equation](https://latex.codecogs.com/gif.latex?f%20%3D%20%5Cfrac%7B1.2k%5COmega%7D%7B1.2k%5COmega%20&plus;19k%5COmega%7D%20%3D%200.0594) . Por lo que tenemos una amplificación de 16.83.

### Elección de las tecnologías utilizadas.
Nuestro primer requisito para seleccionar los transistores necesarios fue que la etapa de entrada tenía que ser de montaje superficial y que los transistores de salida debían poder soportar la corriente a entregar a la carga y pudieran ser montados en un disipador.  
* [**MMBTA56**](https://www.mouser.com/ds/2/149/MMBTA56-889761.pdf) y [**MMBTA06**](https://www.diodes.com/assets/Datasheets/ds30037.pdf)  
  Para la etapa de entrada se eligieron los transistores **MMBTA56**(PNP) y **MMBTA06**(NPN) de Fairchild. Estos son transistores de montaje superficial que cumplen con los requerimientos de corriente y tensión planteados en el circuito. Si bien estos transistores son de propósito general, son de bajo ruido, requisito indispensable para la etapa de entrada. Además, por su capacidad de conmutar entre conducción y corte rapidamente fueron también elegidos para los _switches_ que conmutan los transistores de salida. El _beta_ mínimo de estos transistores es 100, pero el fabricante no otorga valores típicos o máximos. Para asegurar un buen funcionamiento se tuvo que realimentar por emisor en el par diferencia y en al carga activa para apaciguar posibles diferencias en los _betas_. Se ensayaron simulaciones con diferencias de los _beta_ de hasta un 50% e igual se cumplieron las especificaciones planteadas. No obstante, en el armado del circuito se le medirá los valores de _beta_ y se acoplaran aquellos que tengan las menores diferencias. 

* [**2SC5200**](https://github.com/Ronapa/Circuitos2/blob/master/Bibliografia/2SC5200_datasheet_en_20160107.pdf) y [**2SA1943**](https://github.com/Ronapa/Circuitos2/blob/master/Bibliografia/2SA1943_datasheet_en_20160107.pdf)  
  Para los tranistores de potencia se buscó unos que fueran montados en un disipador y que soportaran los valores de corriente que entrega el circuito. Finalmente elegimos los transistores **2SC5200**(NPN) y **2SA1943**(PNP). La elección de estos transistores se basó en que cumplian las especificaciones, su modelo de Spice era accesible y se podía simular, se puede comprar en el país, y su precio no era tan elevado como otros transistores de aplicación especifica para audio. Antes de decidirnos por esta tecnología simulamos el circuito y comprobamos que cumpliera el circuito con las especificaciones. 


* [**MJE340**](https://www.onsemi.com/pub/Collateral/MJE340-D.PDF) y  [**MJE350**](https://www.onsemi.com/pub/Collateral/MJE350-D.PDF)  
  Para el multiplicador de Vbe, que debe estar acoplado térmicamente a los transistores de salida, se eligió un transistor que pudiera ser montado en un disipador. Para este propósito se eligió el transistor **MJE340**(NPN). Además, se eligió la misma tecnología para los transistores que forman el la base del Darlington a la salida de alta potencia. Si bien para este último propósito se podría haber utilizado los transistores de potencia, los transistores elegidos cumplieron con los requerimientos a un costo menor que los de potencia. Esto se debe a que la corriente que manejan estos es mucho menor a la que entregan los transistores de potencia. 

* **Resistores**
 Los resistores con los que implementaremos este circuito, salvo los resistores de 0.1Ohms, son del tipo montaje superficial, o SMD, ya que estos nos permiten tener un diseño compacto y cumplen con las especificaciones de potencia necesaria. Los resistores de 0,1Ohm, como tienen que soportar mayores potencias deben ser resistores de cerámica cementada y son componentes *through-hole*.
 
* **Capacitores**
 Para la elección de los capacitores, se intentó poner capacitores de montaje superficial electrolíticos donde se pudo. Sin embargo, valores de capacitancias muy altas hacían que se elevaran mucho los costos para capacitores superficiales, por lo que se optó por capacitores *through-hole*. Los capacitores que no se eligieron como superficiales son el que desacopla la continua de la entrada ([C1=10uF](https://github.com/Ronapa/Circuitos2/#etapa-de-entrada) ), y el que acopla la alterna para la realimentación ([C2=47uF](https://github.com/Ronapa/Circuitos2/#etapa-de-entrada) ).
 
* **LEDs**
 En nuestro circuito se implementan dos tipos de LEDS, uno es un LED de encendido que indica cuando se energiza el circuito. Los otros dos LEDs se encienden cuando entra en acción la limitación de corriente. El LED de encendido es un LED *through-hole* por el que caen 2,7V. En cambio, los otros son LEDs de montaje superficial por los que caen 1,5V cuando se encienden. 
 
 * **Diodos**
  Los diodos que implementamos en los circuitos que se encargan de conmutar entre las dos vías son diodos Shotky de montaje superficial. Se eligió este tipo de diodos por la menor caída de tensión en directa y, más importante, por la velocidad de conmutar entre conducción y modo reverso. 

### Compensación del circuito y respuesta en frecuencia
Para la compensación del circuito se abrió el lazo realimentador y se colocó una inductancia de un valor muy grande al igual que un capacitor de gran valor en serie con la fuente de prueba. A partir de un analisis de frecuencia se obtuvo la ganancia de lazo, la cual permitió determinar si el sistema era estable. A continuación se presenta el esquemático detallando lo anterior:

![Circuito_compensacion][Circuito_compensacion]

En la figura siguiente se muestra el resultado obtenido y la respuesta al escalón. 

![Graf_sin_compensar][Graf_sin_compensar]

![Circuito_descompensado][Circuito_descompensado]

De estos gráfico se observa que el circuito no está bien compensado. Para compensar, primero desplazamos el primer polo hacia la izquierda agregando un capacitor en el VAS, como se indica en las siguientes figuras.


![Circuito_compensacion1][Circuito_compensacion1]

![Graf_compensado1][Graf_compensado1] 

Agregando ese capacitor desplazamos el primer polo una década, de 1kHz a 100Hz, de este modo logrando obtener un márgen de fase de 90 grados. A continuación se muestra la respuesta al escalón del circuito con este cambio:


![Circuito_compensado1][Circuito_compensado1]

Este márgen de fase todavía puede mejorarse, y se logra compensando al circuito con una capacitancia en paralelo a la resistencia de la realimentación. Esta capacitancia crea un polo en la frecuencia de corte de manera de desplazar la fase del sistema, permitiendonos obtener un márgen de fase distinto. La frecuencia de este polo está dada por https://latex.codecogs.com/gif.latex?f%20%3D%20%5Cfrac%7B1%7D%7B2%5Cpi%20RC%7D. Con R=19kOhm y C=4.7pF obtenemos f= 1.8MHz. Esto desplaza el ángulo en 300kHz unos 12 grados. 
Con esto obtenemos un márgen de fase de 78 grados.

![Circuito_compensacion2][Circuito_compensacion2]


![Graf_compensado2][Graf_compensado2]  

### Potencia y eficiencia

#### Potencia en la carga
Las especificaciones de potencia planteadas en un prinicpio eran de alrededor de 50Watts sobre una carga de 8Ohms. Para lograr esto tendríamos que  llegar a una excursión de 28Vp sobre la carga. Luego del desarrollo de las etapas y por medio de las simulaciones, la amplitud máxima a la salida previo al recorte de la señal que se logró fue de 24V. Esto equivale a una potencia de 36Watts. Si bien no es lo que nos planteamos en un principio consideramos que es un valor adecuado para un amplificador de audio, más aún teniendo en cuenta que una diferencia del 22% en el valor de la potencia equivale a 1dB de diferencia, por lo que al oído esto no será tan perceptible como uno creería.  

![Potencia 1k][Potencia 1k]

Por otro lado, el circuito consume aproximadamente 500mW cuando no hay senal a la entrada. 


![Potencia 0V][Potencia 0V]

Una limitación en cuanto a la potencia que podía entregar el circuito está dada por el instrumental del laboratorio. Las fuentes utilizadas entregan hasta 300Watts en cualquier instante, lo cual limita nuestra corriente total que puede circular por el circuito a 10A. Esto limita la carga máxima que podemos introducirle al circuito a 2,5Ohms. Sin embargo, dentro de las especificaciones planteamos que la carga máxima tenía que ser de 4Ohms, y el circuito limita corriente para valores de carga más altos.

#### Eficiencia del circuito
La eficiencia a la que llegamos, como era de esperarse, depende de la amplitud a la salida y al valor de la carga. Con la amplitud máxima a la salida y una carga de 8Ohms logramos una eficiencia de 77%. Sin embargo, como planteamos antes, los picos de señal en audio son de corta duración, por lo que en la mayor parte de la amplificación los valores serán menores a estos. De este modo se llega a una eficicencia aún mayor. Para una senal senoidal pura, se logra la eficiencia m'axima cuando la amplitud es maxima a la salida, donde se aprovecha la conmutacion entre las dos v'ias de ditinta tension.    

Por otro lado la eficiencia con la salida en un valor más bajo empeora. A modo de ejemplo se simuló la eficiencia con la salida a 5Vp, que se muestra a continuación.

![Eficiencia 5Vp][Eficiencia 5Vp]

Las tres imágenes anteriores fueron simuladas con una señal de entrada senoidal de 1kHz. Fueron simuladas otras frecuencias y los resultados fueron equivalentes. 

#### Potencia disipada en los transistores
Según la hoja de datos de los transistores de potencia, estos pueden aguantar hasta 150 grados centígrados antes de quemarse, y una potencia máxima de colector de 150W. Con una temperatura ambiente de 25°C, obtenemos una resistencia térmica de juntura a carcasa R_{t}  = (Tj_{max}-Tamb)/P_{max} = (150°C-25°C)/150W = 0.833°C/W. Para asegurar de no quemar los transistores tenemos que disipar la potencia, para esto debemos agregar disipadores térmicos que logren disipar la potencia necesaria en todos los casos. El caso crítico, cuando más potencia disipan los transistores, es cuando la carga es de 4Ohms y la tensión de salida vale ![equation](https://latex.codecogs.com/gif.latex?%5Cfrac%7B2%5Ccdot%20Vcc%7D%7B%5Cpi%7D). De este caso obtuvimos las siguientes imágenes. Primero la disipación en cada transistor de potencia, 

![potencia max2][potencia max2]

y luego la disipación total por todos los transistores. 

![potencia max1][potencia max1]

Se tiene, entonces, para el caso más extremo los transistores de mayor potencia disipan picos de 55 W. Asumiendo el peor caso en el que disipen pulsos de máximo valor en un ciclo de trabajo del 50%, podemos tomar que éstos disipan 27.5W. Sin embargo, como se analizará posteriormente, cuando el amplificador se encuentra con un corto en la salida, la potencia que disipa es alrededor de 40 W, por lo que se dimensionarán los disipadores para esta situación. Con este valor, para lograr obtener una temperatura menor a 150°C en la juntura, y asumiendo una temperatura de ambiente máxima de 45°C, se necesita una resistencia térmica de carcasa a ambiente menor a

![equation](https://latex.codecogs.com/gif.latex?R_%7Bca%7D%20%3D%20%5Cfrac%7B%28150%5Cdegree%20C-%2030W%5Ccdot%200.833%5Cdegree%20C/W%20-%2040%5Cdegree%20C%29%7D%7B30W%7D%20%3D%202.83%5Cdegree%20C/W) .

Este valor incluye la pasta térmica, la arandela dieléctrica y el disipador. De acuerdo a esto, tomando una resistencia térmica de la pasta de [0,48°C/W](http://disipadores.com/accesorios.php#) obtenemos que el valor de los disipadores debe ser menor a 1,3°C/W. Se determinó que el disipador que satisface estas necesidades es el [4625-ZD-2E](http://disipadores.com/media_potencia.php), el cual para una altura de 75mm posee una resistencia térmica de 1,2°C/W.  


A continuación en las siguientes imágenes se encuentran la potencia entregada por la fuente en función de la amplitud de la señal de salida, de cero al máximo, y la eficiencia del circuito en función de la señal de salida. La potencia sobre la carga en función de la señal de salida no se muestra ya que crece linealmente.
 
![Potencia_total_f_ampl][Potencia_total_f_ampl]

![Eficiencia_funcion_entrada][Eficiencia_funcion_entrada]

En la segunda imagen se puede apreciar la curva característica de un amplificador clase G, donde hay un quiebre en la eficiencia y luego esta sigue creciendo. En nuestro circuito se llega a la eficiencia máxima cuando cuando se tiene la máxima excursión en la salida definida en las especificaciones. 

Finalmente, en la siguiente imagen se encuentra la potencia entregada por las fuentes en función de la potencia entregada a la carga. 

![Potencia_f_potencia][Potencia_f_potencia]


## Simulaciones sobre el amplificador

### Distorsión

Otro parámetro muy importante en un amplificador es la distorsión armónica que presenta. Par ellos se utilizó el comando del LTSpice .FOUR en el cual se le especifica la frecuencia y la cantidad de armónicas que son tenidas en cuenta para el cálculo de la distorsión. El comando arroja dos valores, de los cuales en todos los casos se tomó el de mayor valor. El valor más bajo corresponde solo a la distorsión armónica mientras que el otro tiene en cuenta todas las otras componentes ajenas a la señal de entrada que son amplificadas. A continuación se encuentran los valores obtenidos para 2 valores de carga y 2 frecuencias distintas. Las potencias indicadas son RMS para la señal pico indicada como Vin.


4 Ohms:

| Vin   | Vout  | THD 1kHz | THD 10kHz | P   |
|-------|-------|----------|-----------|-----|
| 167mV | 2V    | 0.012%   | 0.017%    | 1W  |
| 526mV | 9V    | 0.012%   | 0.036%    | 10W |
| 832mV | 14V   | 0.072%   | 0.23%     | 25W |
| 1.27V | 21.3V | 0.063%   | 0.18%     | 58W |
| 1.41V | 23.7V | 0.056%   | 0.16%     | 70W |

8 Ohms:

| Vin   | Vout  | THD 1kHz | THD 10kHz | P     |
|-------|-------|----------|-----------|-------|
| 167mV | 2V    | 0.012%   | 0.015%    | 500mW |
| 526mV | 9V    | 0.012%   | 0.05%     | 5W    |
| 832mV | 14V   | 0.074%   | 0.21%     | 12.5W |
| 1.27V | 21.3V | 0.061%   | 0.17%     | 29W   |
| 1.41V | 23.7V | 0.056%   | 0.16%     | 35W   |

En todos los casos se puede observar que para una tensión de entrada de 830mV aproximadamente la distorsión aumenta notablemente. Esto se debe a que comienza a actuar la conmutación de la etapa de salida. También se aprecia que a medida que aumenta la frecuencia la distorsión introducida por este subcircuito es mayor, ya que se producen más cantidad de transiciones entre los transistores. Se trabajó para eliminar lo más posible los transitorios de conmutación pero no se pudo mejorar más. Es posible que si se hubiese tenido en cuenta una tecnología MOS para la conmutación se pudieran haber obtenido mejores resultados.

#### Impedancia de entrada y salida

Para obtener la impedancia de entrada del amplificador se decidió realizar un barrido en frecuencia y obtener la impedancia de entrada para todas las frecuencias. En la siguiente figura se observa la impedancia de entrada para frecuencias entre 0.1Hz y 10GHz. Se puede apreciar que la impedancia de entrada es 20 kOhms para todo el rango entre los 10 y los 100kHz, lo cual abarca toda la banda de audio. Resulta importante destacar que es una característica deseada en un amplificador de audio.

![Rin_circuito](/Imagenes/Rin_circuito.PNG)

![Impedancia de entrada](/Imagenes/Rin_frecuencia.PNG)

Para el caso de la impedancia de salida, el método fue muy similar al de la impedancia de entrada, solo que esta vez se conectó una fuente de corriente en paralelo con la carga. Se simuló para distintos casos en los cuales se conectaron cargas de 4 u 8 Omhs y también con el amplificador sin carga. Debido a que el amplificador se encuentra realimentado, no hubo grandes variaciones. En este caso la impedancia mantiene un valor de aproximadamente 10 mOhms entre los 20Hz y 1kHz, y luego aumenta a 340mOhms para los 20 kHz.

![Rout_circuito](/Imagenes/Rout_circuito.PNG)

![Impedancia de salida](/Imagenes/Rout_frecuencia.PNG)


#### Limitación de corriente y protecciones

Para el dimensionamiento de la protección de limitación de corrriente se debió considerar las condiciones sobre las que debía operar el amplificador. Como el diseño debe funcinar tanto para 4 como para 8 Omhs, la protección no debía interferir con estas condiciones. Como para una carga de 4Omhs se generaban corrientes de alrededor de 6A, se consideró como valor apropiado una limitación de corriente de 7,5 u 8 A. Como ya se mencionó anteriormente esto se logró polarizando un transistor que toma corriente desde la base de la etapa de salida. A continuación se muestra la corriente que circula por la carga para un valor de 2 Omhs.

![Limitacion corriente](/Imagenes/limitador_corriente_2_omhs.PNG)

Efectivamente se verifica que el limitador de corriente funciona para casos donde la impedancia es menor a la esperada.

Para el caso de que se produzca un corto en la salida se aprecia que la corriente máxima es mayor a 7A, aunque no de manera considerable. Igualmente para estos casos se colocará un fusible en serie con la fuente de manera de proteger el equipo, ya que los transistores se encontrarán disipando potencia de manera constante. Cabe destacar que para que el limitador funcione debe existir una señal a la entrada que provoque en la salida una corriente de alrededor de 8A, ya que en ausencia de señal no circulará corriente a la salida.


![Limitacion corriente corto](/Imagenes/limitador_corriente_corto.PNG)

#### Rechazo de ruido de la fuente

Para el rechazo de ruido de la fuente se agregaron en serie con cada una de las fuentes una tensión de riple de 1V para las fuentes de 30V y 0.5V para el caso de las fuentes de 15. Este sería un caso muy desfavorable debido a que una variación en la tensión de de alimentación de la fuente switching de 1V se reflejaría como un ripple mucho menor a la salida. Sin embargo, las simulaciones para estos casos mostraron un ripple menor a 1.6mV, lo cual para valores de señal se podría considerar despreciable. Especialmente si la señal hiciera trabajar al amplificador solo con la fuente de baja tensión por lo mencionado anteriormente.

#### Limitaciones sobre los valores de alimentación

En las simulaciones variamos los valores de la alimentación del circuito, para conocer lo límites del funcionamiento de nuestro amplificador. Por un lado, para la alimentación de ±15V encontramos que el funcionamiento del circuito no varía de manera significativa cuando esta varía. Si bien esta alimentación es la que alimenta a los transistores de potencia y aporta a la polarización de los ciruitos switches, cuando cambia este valor lo único que produce es que se cambie la tensión a la que conmutan los transistores de potencia. El problema de bajar mucho o subir mucho el valor de esta tensión produce que el circuito funcione más como un amplificador Clase B, bajando la eficiencia. 

Por otro lado, cambiando los valores de las fuentes de ±30V sí afecta a la polarización de nuestro circuito, y el circuito deja de comportarse como especificado. Por un lado, bajando los valores de estas fuentes produce que entren en corte los transistores de potencia. En las simulaciones, con las fuentes en ±27.5V se producía un recorte a la salida antes de llegar a los 24V, lo cual distorsionaba la señal. Si bien no es un comportamiento destructivo, causa que el amplificador no cumpla las especificaciones de amplitud máxima a la salida, pero puede funcionar si a la entrada se baja la amplitud de la señal. Reduciendo aún más el valor de esta tensión sigue andando el circuito, cortando la señal a la salida siempre en aproximadamente 3V antes de llegar a el valor de esta tensión.

Por otro lado, al incrementar el valor de estas fuentes se corre el riesgo de que alguno de los componentes del circuito se queme. Los transistores que más se exigen, fuera de los de potencia, son los de los pares Darlington que entregan la señal a los de mayor potencia, y los de los seguidores, que unen la etapa de salida con la del VAS. Los transistores que forman parte de los Darlington de la etapa de salida son los MJE340, que soportan potencias de hasta 20W, mucho mayor que las potencias que entregan estos transistores. Los de la etapa de seguidor, los MMBTA06/56, en cambio, están elegidos para soportar la potencia debida cuando se los polariza con 30V. Estos transistores pueden disipar hasta 310mW, y con las fuentes en 30V disipan 150mW. Sin embargo, con las fuentes en ±40V estos disipan 250mW. 
Por otro lado, viendo a los transistores de potencia, vemos que estos son mucho más sensibles a los cambios en los valores de la fuente. Cambiando los valores de las fuentes a ±40V se ve que estos transistores disipan casi el doble (180%), para lo cual, si bien funcionaría, habría que cambiar los disipadores para evitar que se queme. Por estas razones, proponemos que la fuentes funcionen en ±30V, con una desviación menor a los 2V para evitar problemas de distorsión o peor, que se queme el circuito. 


### Diseño del PCB

Para el diseño del PCB se debió pasar el esquemático realizado en LTSpice a el programa usado para diseñar el PCB; en este caso el Kicad. En este diseño se agregaron los conectores para las fuentes de alimentación así como también para lo bornes de salida para el parlante.

![PCB_diseño](/Imagenes/PCB_diseño.png)

Una vez importandos todos los componentes y habiendo elegido los modelos fisicos correspondientes se buscó la posición adecuada para cada uno de los mismos para el posterior ruteo. Se comenzó por ubicar los transistores de la etapa de potencia en un borde de la placa de manera de poder conectar el disipador sobre el borde de la placa. Posteriormente se ubicaron las resistencias para el envalamiento térmico y las limitaciones de corriente para ambos semiciclos. Luego se busco armar un subconjunto con los subcircuitos encargados de realizar la conmutación, así como también montar en forma cercana la etapa sobre la que actuaban los conmutadores o switches. Finalmente se armó el par diferencial junto con el VAS y se buscó comprimir lo más posible el tamaño del PCB.

La desición de poner las borneras sobre la cara inferior de la placa se basó en que en esa zona se encontraría el disipador de los transistores, por lo que si se hubieran querido posicionar sobre la cara superior se deberían haber posicionado cerca de la etapa de entrada. Esto hubiera traído como consecuencia mayores pérdidas debido a que la etapa de potencia se encontraría más alejada de la alimentación. Además se tendrían lineas por la que circularía alta corriente sobre las pistas de señal que se encuentran en la cara contraria, lo cual no es lo más recomendable. También se realizó un esfuerzo por mantener la mayor parte de las pistas de señal de un solo lado, manteniendo las pistas de potencia del otro lado. En relación a esto, se debieron trazar pistas por entre algunos de los transistores SMD, especialmente en la etapa del semiciclo negativo, lo cual compromete en cierta medida el diseño debido a que el ancho de las pistas es de 0.3mm. Si bien las placas que se pueden fabricar en la Facultad admiten pistas de hasta 12mills de ancho con la misma distancia de separación, se estaría en el límite de esta cota.

Otro aspecto fue el del diseño fue el ancho de las pistas encargadas de transportar la corriente para la etapa de salida, así como también las pistas hacia los bornes de salida. Mediante la calculadora provista por Kicad, se buscó un valor acorde para las pistas de potencia. En los casos que fuera posible se podrían utilizar pistas más anchas pero el objetivo era obtener una cota mínima. Teniendo en cuenta que la máxima corriente que puede circular es de alrededor de 7 amperes en condiciones normales de operación, se tomó como un valor conservador una corriente pico de 8A. Teniendo en cuenta que los cálculos de potencia se asumen para una señal senoidal y aplicando el factor para obtener el valor eficaz, la corriente eficaz sería de 5.7A. Para una variación térmica de 12°C se obtuvo un espesor de pista de 3mm, el cual fue el valor utilizado. 

A continuación se muestra una imágen del PCB diseñado, con las pistas en rojo correspondientes a la etapa de entrada, VAS y la etapa de salida de baja potencia, y en verde la etapa de alta corriente. Finalmente se tuvieron en cuenta los puntos de montaje, los cuales además de tener uno en cada esquina, también se agrego uno en la mitad de la placa aproximadamente y otro cerca de los transistores de potencia para aumentar la rigidez ya que en esta zona se encontrarán los disipadores.

![PCB](/Imagenes/PCB.PNG)

Adicionalmente se agregó un render de como podría verse la placa una vez construida.

![PCB_render](/Imagenes/PCB_render.PNG)

Una consideración importante que no se evidencia en las imágenes del PCB es que el transistor del multiplicador de Vbe se encontrará conectado por cables y acoplado térmicamente a los transistores de salida uniendoló al mismo disipador. Esta desición se tomó debido a que se consideró que no se justificaba complicar el diseño del PCB para acercar este transistor a la etapa de salida.

## Especificaciones

En el comienzo de este diseño se definieron ciertas especificaciones que el circuito debía cumplir. De acuerdo a estas especificaciones, y con las limitaciones que se plantearon, se basó el objetivo en diseñar el mejor circuito posible. 

Las especificaciones planteadas y el resultado al que llegamos se encuentran a continuación, en todos los casos se polarizó al circuito con una fuente de 30V y la salida de baja potencia con 15V.

| Parámetro                 | Especificación | Valor simulado | Condición                            |
|---------------------------|----------------|----------------|--------------------------------------|
| THD                       |     <0.05%     |     0.056%     | señal de 1kHz, 1Vrms, carga de 8Ohms |
| Vout con Vin=0            |     <10mV      |      1.6mV     |  carga de 4Ohms, 8Ohms y sin carga   |
| Potencia en la carga      |    90W(45W)    |    70W(35W)    |         carga de 4Ohms(8Ohms)        |
| Eficiencia                |      >75%      |       77%      |         señal de 1kHz, 1Vrms         |
| Impedancia de entrada     |    >10kOhms    |     20kOhms    |       señales de 10Hz a 100kHz       |
| Impedancia de salida      |    <0.4Ohms    |     10mOhms    |        señales de 10Hz a 20kHz       |
| Valores posibles de carga |     >4Ohms     |     >4Ohms     |            señales ≤ 1Vrm            |
| Limitación de corriente   |       7A       |      7,5A      |            carga de 2Ohms            |
| Potencia máxima disipada  |      <30W      |      27,5W     | señal de 1kHz, 1Vrms, carga de 4Ohms | 

Las especificaciones que no llegamos a cumplir son las de distorsión armónica, potencia en la carga y limitación de corriente.

Por un lado, la causa de tener esta distorsión se encuentra en el hecho de que se tiene la salida de un amplificador clase G-paralelo. Esta configuración mejora la eficiencia respecto de un clase G-serie ya que solo se encuentra en conducción un solo transistor a la vez, pero a la vez introduce una pequeña deformación en la señal al conmutar entre un camino y otro. Sin embargo, esto ocurre cuando la señal sobrepasa el valor de conmutación. Como se planteó antes, en las señales de audio que comunmente se le aplican a este tipo de amplificadores tienen períodos cortos de tiempo en el que la señal es máxima, situandose la mayor parte del tiempo debajo del promedio. Con esto en mente, y considerando que el valor obtenido es cercano al valor planteado inicialmente, se concluye que es un valor válido.

En el caso de la potencia en la carga, el planteo inicial surgió de suponer una caída máxima de 3V en los transistores de potencia, de los cuales se obtenía una excursión máxima de 27Vpico. Esto sin embargo no fue posible con los transistores seleccionados, y logramos una extensión de 24Vpico. El valor de esta tensión pico se debió a que la fuente con la que se energiza el circuito es de 30V, y porque los costos de transistores más eficientes excedían los valores iniciales y encarecían demasiado el circuito completo. 

Por otro lado, en el caso de la corriente de limitación, el valor debía ser tal que no limitara en el caso de corriente máxima, carga de 4Ohms y tensión máxima de 24Vpico. Se planteó, entonces, como caso limite una carga de 3,5Ohms y una tensión máxima a la salida. De acuerdo a esto se tenía que los picos de corriente llegaban a casi 7A. En el circuito logramos limitar la corriente en 7,5A, lo cual se considera un valor aceptable ya que está en condiciones a las que el amplificador no debería llegar. Sin embargo, en el peor de los casos en que se tengan cargas mayores en la salida, y que además se tenga una señal en la entrada, los fusibles en la entrada se introdujeron para proteger al circuito. 


## Construcción

### Implementación de la etapa de entrada

Una vez finalizado el diseño del amplificador se realizó la implementación de la etapa de entrada y el VAS en una placa expertimental para hacer una validación parcial del diseño. Para ello se compraron los componentes equivalentes a los SMD en su versión discreta y se analizó la polarización del circuito. A continuación se presenta la imagen de la placa experimental sobre las que se realizaron algunas mediciones.

![Etapa_entrada_PCB](/Imagenes/etapa_entrada_PCB.png)

Como primera medición se verificó la tensión en algunos nodos del par diferencial para verificar su funcionamiento y se obtuvieron lecturas dentro de lo esperado. Luego se midieron las tensiones sobre los resistores que se encontraban en la etapa VAS. A partir de los valores de tensión y conociendo el valor de los resistores se estimó la corriente de polarización. Según la simulación esta corriente era de alrededor de 45mA.

La imagen a continuación muestra una tensión de 470mV, lo cual teniendo en cuenta que el resistor era de 10 Omhs, la corriente de polarización resultaba ser de 47mA. También se corroboró esto con la medición de la tensión sobre la resistencia de emisor de la fuente de corriente del VAS, la cual arrojó un valor de 2.27V. En este caso la resistencia era de 47 Omhs por lo que la corriente también rondaba los 48mA.

![Corriente_VAS](/Imagenes/Etapa_entrada_medicion_corrienteVAS.jpg)

![Corriente_VAS2](/Imagenes/Etapa_entrada_medicion_corrienteVAS2.jpg)

Una vez verificada la polarización se colocó un generador de señales a la entrada del amplificador para verificar el valor de la ganancia total. A partir del analisis de realimentación se obtuvo que la ganancia del amplificador era de 16.66, por lo que frente a la señal de entrada de 200mV pico se debería obtener una tensión a la salida de 3.35V. Esto se verificó mediante las mediciones a continuación.

![Corriente_VAS](/Imagenes/Etapa_entrada_medicion_Vin.png)

![Corriente_VAS2](/Imagenes/Etapa_entrada_medicion_Vout.png)

Si bien en la medición el valor obtenido es de alrededor de 3.5V, se verificó que el funcionamiento de la etapa de entrada y VAS era acorde a lo esperado.

La implementación de esta etapa de entrada nos permitió percatarnos de un error en el dimensionamiento de los transistores del VAS, los cuales no eran capaces de disipar lo necesario. Esto finalmente se solucionó para el modelo definitivo disminuyendo la corriente de polarización de esta etapa, ya que la diferencia de distorsión no justificaba mantener una corriente de polarización de 45mA. Esta corriente de polarización se modificó mediante el reemplazo del resistor de emisor de la fuente de corriente por uno de 470 Omhs.
  
  
## Problemas y Plan 
 * Temas del PCB: 
 
    * Falta potenciometro en multiplicador de Vbe -> No soldamos R12 de [acá](https://github.com/Ronapa/Circuitos2/blob/master/Imagenes/PCB_dise%C3%B1o.png) y ponemo un pote colgando en el aire 
    * No entran lo capacitores de acople de las fuentes en la placa -> Les colgamos unos cables y los soldamos de abajo sobresalidos (si o sí tienen que ir de abajo por el disipador) 
    * El modelo del plug jack no coincide el comprado con el de la placa -> Mandamos cables para que ande, si hace falta con la pistola de plástico lo pegamos a la placa, es más decorativo que otra cosa a esta altura el jack 
    * Se redujo la Re de la fuente de corriente a 470Ohm -> hay que tener en cuenta este cambio al soldar 
    * Tuvimos en cuenta dónde se conectan las switching? 
  
 * Falta comprar:  
 
    * Diodos shotky, fijarse bien el código que sea el BAT54, no el BAT54A 
    * NPN de potencia, 2SA1943, o similar 
    * Disipadores 
    * Un capacitor de acople de fuentes más 
 
 * Opciones de como seguir: 
 
    * *Nueva placa:* Si hacemos una nueva placa hay que tener en cuenta los capacitores de acople, el footprint del jack y agregar el potenciometro en el xVbe. Si es barato hacerlo en francia yo lo cambio y lo mando a hacer. Habría que poner jumpers para ir soldando y probando de a partes, así si algo no funca lo vemos en el momento.  También le podemos agregar el coso ese para modular que ya era un circuito integrado, y un control de volumen. 
    * *Seguimos con la que tenemos:* Si seguimos con la que tenemos tenemos que resolver un par de temas que seguro van a ser con cables en el aire. Se nos va a complicar un poco el tema de debuggear si hay algún problema al estar todo junto pero podemos ir haciendo como ya hicimos, vemos las distintas partes en "prototipos" que vamos armando en una proto y rezamos que se repita en la placa. No podemos controlar el volumen desde el ampli pero tampoco es algo necesario eso. 
    
 * Fuentes Switching:
  Las hacemos?, no las hacemos?, ya el circuito en sí nos conmuta así que el enunciado lo cumplimos... De cualquier manera se puede dejar para el final y plantear todo el circuito con fuentes de laboratorio y conectarlas al final. Necesitariamos una de +15 y otra de -15, podemos pedir prestadas? tenemos simulado [acá](https://github.com/Ronapa/Circuitos2#fuentes-switching) la de +15.
  


[kenwood]: Imagenes/kenwood.png
[Nuestro Circuito]: Imagenes/Circuito_completo.png
[Etapa Entrada]: Imagenes/Etapa_entrada.png
[Etapa VAS]: Imagenes/Etapa_VAS.png
[Etapa Salida]: Imagenes/Etapa_salida.png
[Etapa Switches]: Imagenes/switch.png
[Conmutación circuitos]: Imagenes/Conmutacion.PNG
[Eficiencia amplitud maxima]: Imagenes/Amplitud_maxima.png
[Potencia 1k]: Imagenes/Potencia1k.PNG
[Eficiencia 5Vp]: Imagenes/Potencia_5V.PNG
[Eficiencia maxima]: Imagenes/Eficiencia_maxima.png
[Circuito en bloques]: Imagenes/Bloques.png
[Potencia 0V]: Imagenes/Potencia_0V.PNG
[potencia max1]: Imagenes/Potencia_max1.PNG
[potencia max2]: Imagenes/Potencia_max2.PNG
[Realimentador]: Imagenes/Realimentador.png
[Fuente switching]: Imagenes/Fuente_Switching.PNG
[switching load transient]: Imagenes/switching_load_transient.PNG
[switching input transient]: Imagenes/switching_input_transient.PNG
[switching steady]: Imagenes/switching_steady.PNG
[Diferencia_VAS]: Imagenes/Diferencia_VAS.PNG
[Potencia_total_f_ampl]: Imagenes/Potencia_total_f_ampl.PNG
[Eficiencia_funcion_entrada]: Imagenes/Eficiencia_funcion_entrada.PNG
[Potencia_f_potencia]: Imagenes/Potencia_f_potencia.PNG
[Circuito_compensacion]: Imagenes/Circuito_compensacion.PNG
[Graf_sin_compensar]: Imagenes/Graf_sin_compensar.PNG
[Circuito_compensacion1]: Imagenes/Circuito_compensacion_1.PNG
[Graf_compensado1]: Imagenes/Graf_compensado1.PNG
[Circuito_compensacion2]: Imagenes/Circuito_compensacion_2.PNG
[Graf_compensado2]: Imagenes/Graf_compensado2.PNG
[Circuito_descompensado]: Imagenes/Circuito_descompensado.PNG
[Circuito_compensado1]: Imagenes/Circuito_compensado1.PNG


