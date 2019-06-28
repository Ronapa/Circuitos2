# Trabajo Práctico de Diseño
## Diseño de Circuitos Electronicos (86.10)
### Diseño de un Circuito amplificador clase G paralelo.
####  Rodrigo Nahuel Parra
####  Gonzalo Gilces Duran
####  Segundo Molina Abeniacar


##### Resumen
El objetivo de este trabajo fue diseñar un amplificador de audio clase G con la etapa de salida conmutada en paralelo. Se logró diseñar un circuito que amplifica una señal de 1Vp a una de 25Vp para una carga de hasta 8Ohms. El circuito simulado entrega una potencia máxima al parlante de 75 Watts, con una distorsión menor al 0.1%. El circuito cuenta con un LED de encendido, una limitación de corriente que protege al circuito y a la carga y protecciones que protegen al circuito de una conexión de tensión a la salida. La tensión de salida con entrada nula se simuló y se obtuvo un valor de 3mV. Al tener los transistores de la etapa de potencia en paralelo se logró aumentar la eficiencia, logrando que se conduzca un solo transistor a la vez. Esto se logró con un circuito de control que conmuta los transistores según una tensión de referencia. 

#### Desarrollo
El circuito fue diseñado en base al amplificador [Kenwood KA-7X](http://materias.fi.uba.ar/6610/Manuales%20de%20servicio%20tecnico/Clase%20G%20y%20H/Kenwood/KA-7X/hfe_kenwood_ka_7x_service.pdf). Este es un amplificador stereo clase G con etapa de salida en paralelo. Sin embargo nuestro amplificador es de tipo mono. A continuación en la imagen se muestra una parte de la etapa de salida del amplificador Kenwood.
![Amplificador Kenwood][kenwood]


Nuestro circuito simulado se encuentra a continuación:
![Circuito LTSpice][Nuestro Circuito]

##### Elección de las tecnologías utilizadas.


[kenwood]: 
[Nuestro Circuito]:
