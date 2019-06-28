# Trabajo Práctico de Diseño
## Diseño de Circuitos Electronicos (86.10)
## Diseño de un Circuito amplificador clase G paralelo.
#####  Rodrigo Nahuel Parra
#####  Gonzalo Gilces Duran
#####  Segundo Molina Abeniacar


### Resumen
El objetivo de este trabajo fue diseñar un amplificador de audio clase G con la etapa de salida conmutada en paralelo. Se logró diseñar un circuito que amplifica una señal de 1Vp a una de 25Vp para una carga de hasta 8Ohms. El circuito simulado entrega una potencia máxima al parlante de 75 Watts, con una distorsión menor al 0.1%. El circuito cuenta con un LED de encendido, una limitación de corriente que protege al circuito y a la carga y protecciones que protegen al circuito de una conexión de tensión a la salida. La tensión de salida con entrada nula se simuló y se obtuvo un valor de 3mV. Al tener los transistores de la etapa de potencia en paralelo se logró aumentar la eficiencia, logrando que se conduzca un solo transistor a la vez. Esto se logró con un circuito de control que conmuta los transistores según una tensión de referencia. El circuito del amplificador es alimentado con una fuente de ±30V y una de ±15V. Estos valores de tensión se logran con una fuente de laboratorio de ±30V y una fuente switching que otorga los valores de ±15 necesarios. 

## Desarrollo
El circuito fue diseñado en base al amplificador [Kenwood KA-7X](http://materias.fi.uba.ar/6610/Manuales%20de%20servicio%20tecnico/Clase%20G%20y%20H/Kenwood/KA-7X/hfe_kenwood_ka_7x_service.pdf). Este es un amplificador _stereo_ clase G con etapa de salida en paralelo. Nuestro amplificador, en cambio, es de tipo _mono_. A continuación en la imagen se muestra una parte de la etapa de salida del amplificador Kenwood.
![Amplificador Kenwood][kenwood]


Nuestro circuito simulado se encuentra a continuación:
![Circuito LTSpice][Nuestro Circuito]

Elegimos hacer un circuito clase G porque con este logra una elevada eficiencia en amplificación de audio. Esto se debe a que, en audio, los transistores de mayor tensión funcionan en intervalos cortos de tiempo, siendo los de menor señal los que conducen la mayor parte. Esto se debe a que las señales de audio que se reproducen generalmente tienen un alto cociente de valor pico a valor medio, esto es, los momentos de señales altas son breves y pocos. Además, al plantear la etapa de salida con los transistores en paralelo, logramos aumentar aún más la eficiencia, ya que en cualquier momento solo está conduciendo un transistor. En cambio, si la salida estuviera en serie, en los momentos de alta señal conducirían los dos transistores de salida. 

### Polarización y funcionamiento del circuito
#### Etapa de entrada del circuito
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

![Etapa de Salida][Conmutación circuitos]

### Elección de las tecnologías utilizadas.
Nuestro primer requisito para seleccionar los transistores necesarios fue que la etapa de entrada tenía que ser de montaje superficial y que los transistores de salida debían poder aguantar la corriente a entregar a la carga y pudieran ser montados en un disipador.  
* [**BC860C**](https://assets.nexperia.com/documents/data-sheet/BC849_BC850.pdf) y [**BC850C**](https://assets.nexperia.com/documents/data-sheet/BC859_BC860.pdf)  
  Para la etapa de entrada elegimos los transistores **BC860C**(PNP) y **BC850C**(NPN) de Phillips. Estos son transistores de montaje superficial que cumplen con los requerimientos de corriente y tensión planteados en el circuito. Si bien estos transistores son de propósito general, son de bajo ruido, requisito indispensable para la etapa de entrada. Además, por su capacidad de conmutar entre conducción y corte fueron también elegidos para los _switches_ que conmutan los transistores de salida. El _beta_ de estos transistores sin embargo puede variar entre 420 y 800, por lo que se tuvo que realimentar por emisor en el par diferencia y en al carga activa para apaciguar estas diferencias. Se ensayaron simulaciones con diferencias de los _beta_ de hasta un 50% e igual se cumplieron las especificaciones planteadas. No obstante, en el armado del circuito se le medirá los valores de _beta_ y se acoplaran aquellos que tengan las menores diferencias. 

* [**2N3055** y **MJ2955**](https://www.onsemi.com/pub/Collateral/2N3055-D.PDF)  
  Para los tranistores de potencia se buscó unos que fueran montados en un disipador y que soportaran los valores de corriente que entrega el circuito. Finalmente elegimos los transistores **2N3055**(NPN) y **MJ2955**(PNP). La elección de estos transistores se basó en que cumplian las especificaciones, su modelo de Spice era accesible y se podía simular, se puede comprar en el país, y su precio no era tan elevado como otros transistores de aplicación especifica para audio. Antes de decidirnos por esta tecnología simulamos el circuito y comprobamos que cumpliera el circuito con las especificaciones. 


* [**MJE340**](https://www.onsemi.com/pub/Collateral/MJE340-D.PDF) y  [**MJE350**](https://www.onsemi.com/pub/Collateral/MJE350-D.PDF)  
  Para el multiplicador de Vbe, que debe estar acoplado térmicamente a los transistores de salida, se eligió un transistor que pudiera ser montado en un disipador. Para este propósito se eligió el transistor **MJE340**(NPN). Además, se eligió la misma tecnología para los transistores que forman el la base del Darlington a la salida de alta potencia. Si bien para este último propósito se podría haber utilizado los transistores de potencia, los transistores elegidos cumplieron con los requerimientos a un costo menor que los de potencia. Esto se debe a que la corriente que manejan estos es mucho menor a la que entregan los transistores de potencia. 


### Compensación del circuito y calidad de la señal
Como compensamos y rta en frecuencia

### Potencia y eficiencia del amplificador

### Ensayos variando la carga. 


### Diseño del PCB
  
[kenwood]: Imagenes/kenwood.png
[Nuestro Circuito]: Imagenes/Circuito_completo.png
[Etapa Entrada]: Imagenes/Etapa_entrada.png
[Etapa VAS]: Imagenes/Etapa_VAS.png
[Etapa Salida]: Imagenes/Etapa_salida.png
[Etapa Swicthes]: Imagenes/Etapa_switches.png
[Conmutación circuitos]: Imagenes/Conmutacion.png
