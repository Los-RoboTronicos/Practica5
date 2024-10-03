# Practica4

## Integrantes
- Uriel Vladimir Alvarez Tapia 20121191
- Miguel Angel Castañeda Garcia 20120015
- Diana Alejandra Mendoza Mendoza 20121226

## Introducción 
En la robótica industrial, el control preciso de los movimientos es esencial para lograr eficiencia en tareas repetitivas y de alta precisión. La programación de trayectorias complejas, como la escritura de caracteres, requiere una combinación de comandos avanzados que permitan optimizar tanto el tiempo de ejecución como el uso de recursos. Los sistemas como el brazo robótico utilizado están diseñados para ejecutar movimientos finos con alta exactitud, empleando comandos como Go, Move, Jump, Jump3cp, Arc, y Arc3. Estos comandos permiten diseñar trayectorias eficientes que minimizan el desgaste del equipo y aumentan la productividad en aplicaciones industriales de alto rendimiento.

## Marco teórico
Comandos para definir trayectorias

1. Move
Un comando para que el robot se mueva al punto de destino. Es posible establecer el destino
o la velocidad de movimiento.
Sintaxis: Move Velocidad Punto

2. Jump3CF
Salta a un punto con un movimiento 3D en una ruta continua.
Sintaxis: Jump3CF Punto1 Punto2 PuntoFinal

3. Arc3
Mueve el brazo en 3D con interpolación circular.
Sintaxis: Arc3 PuntoMedio PuntoFinal

4. Pass
Ejecuta un movimiento de punto a punto simultáneamente en cuatro articulaciones, que pasa cerca pero no a través de los puntos especificados.
Sintaxis: Pass Punto1 Punto2


## Objetivo
Escribir una palabra de al menos 4 letras utilizando el menor numero de puntos por letra y optimizando los comandos.

## Desarrollo
Antes de programar el brazo robótico para escribir la palabra, se delimita un espacio específico donde el robot deberá operar. Esto ayuda a asegurar que las letras caben dentro de un marco determinado, evitando que el brazo salga de los límites o realice movimientos innecesarios (Figura 1).
La selección de un espacio de 30mm x 15mm no solo reduce el consumo de tiempo en la escritura, sino que también simula las limitaciones que suelen existir en la robótica industrial, donde se trabaja con áreas pequeñas y específicas.

![image](https://github.com/user-attachments/assets/f3356bac-1ecf-4e13-b2b5-4bdb16f92df9)

En la figura 2 se muestra la palabra que el brazo robótico debe escribir, junto con los puntos de control asignados a cada letra. Cada uno de estos puntos es una coordenada específica que el robot debe alcanzar para formar las letras de manera precisa.
Los puntos de control se han colocado de manera estratégica para minimizar el número de movimientos necesarios y maximizar la eficiencia del proceso. En lugar de realizar movimientos extra, el brazo se desplaza únicamente a estos puntos, lo que ahorra tiempo y energía.
También se muestra el espacio pensado entre cada letra.

![image](https://github.com/user-attachments/assets/2e7872f6-c60f-4072-b357-999d7f6600a5)

Posteriormente, en la figura 3 se tiene el código realizado:

1. Inicio y posición previa a escribir:
Home: El comando mueve el brazo robótico a la posición inicial o base.
Go Wsup: Establece la posición de inicio antes de comenzar a escribir, conocida como Word Start Up Position.

2. Escribir la letra F:
Jump3CF WordStart, Fmedium, EndFmedium: Utiliza el comando Jump3CF, que permite realizar un movimiento con interpolación en tres puntos para llevar el brazo a la posición inicial de la letra "F" y hacer el trazado medio de la letra.
Move EndFmedium +Z(10): Después de escribir la parte principal de la "F", el brazo se desplaza hacia arriba en el eje Z para evitar colisiones al reposicionarse.
Go Fmedium +Z(10): Mueve el brazo a otra parte de la letra.
Move Fmedium: Realiza el siguiente trazado de la letra "F".

3. Escribir la letra U:
Jump3CF Fmedium, Fupper, Endfup: Usa tres puntos para moverse al principio de la "U".
Jump3CF Endfup +Z(10), Up1 +Z(10), Up2 +Z(10), Uup1: Describe la trayectoria de la "U" utilizando movimientos en el eje Z para evitar colisiones.

4. Escribir la letra C:
Arc3 Umedium, Uup2: Utiliza un movimiento circular para completar la parte superior de la "U" y posicionarse para comenzar la "C".
Jump3CF Uup2 +Z(10), Cup +Z(10), Cup: Similar a la "F", utiliza tres puntos para formar la "C".

5. Escribir la letra K:
Pass Cdown +Z(10): Prepara la trayectoria para la "K".
Go Kupl +Z(10): Mueve el brazo al inicio de la letra "K".
Jump3CF Kupl, Kmedium, Kdigup: Este comando permite realizar el trazado de la letra "K" en tres puntos.

![image](https://github.com/user-attachments/assets/a22f3a62-ca08-484f-80e0-3d2a86723627)

Finalmente, en la figura 4 se muestra la tabla de posiciones exactas de cada punto.

![image](https://github.com/user-attachments/assets/48878fd9-58f3-4d1d-9fc4-885e1b9da6ff)

## Resultados
Tras la simulacion del codigo se procedio a su implementacion fisica logrando el objetivo (Figura 5).

![image](https://github.com/user-attachments/assets/4a46b816-d5dd-438d-a9cd-214ce6c91344)

El objetivo se logró exitosamente, sin embargo, cabe aclarar que el espacio pensado entre letras fue muy pequeño, dando la impresión de que no había, y el grosor del plumón no permitió la correcta apreciación de las letras.

## Conclusiones
- **Uriel Vladimir Alvarez Tapia:** Esta practica permitió evidenciar la utilidad de comandos de movimientos distintos a l comando Go,  reduciendo el tamaño de los códigos involucrados en las soluciones.
Sin embargo, es necesario considerar la sintaxis y aún más importante, el comportamiento de los respectivos comandos, así como las condiciones físicas del entorno. Ya que una mala comprensión de los movimientos propicia el riesgo del equipo y de los operadores.
Lo anterior se evidencia en que las letras en las palabras no contaban con la separación planeada ya que esta era inferior a la requerida por causa de la punta del plumón. De igual forma, la curva descrita por el comando pass  describe trayectorias que se aproximan a una circunferencia y que, por lo tanto, se encuentran fuera del espacio designado a cada letra, haciendo especial evidencia de esto en la letra U.
- **Miguel Angel Castañeda Garcia:** El conocimiento de las trayectorias que define cada uno de los comandos que se mencionaron resultó de suma importancia para lograr definir de manera correcta las direcciones que debía seguir el robot para escribir cada letra de la palabra. 
Por otro lado, el ajuste del marcador sobre la pizarra generó ciertos inconvenientes al momento de realizar el trazado, además de que el espacio considerado para cada una de las letras de la palabra fue demasiado pequeño, sin embargo, se estuvo un buen resultado sobre la palabra planteada. 

- **Diana Alejandra Mendoza Mendoza:** En esta práctica, se utilizaron comandos un poco más complejos permitiendo la realización de una actividad más elaborada, el tener la libertad ceativa de la palabra nos llevó a desafiarnos y poner a prueba la experiencia que ya se tenía de prácticas anteriores, buscando siempre la manera más sencilla de realizar el objetivo. Surgieron dificultades y cosas no previstas en el transcurso de la práctica, como el espacio reducido entre letras o la falta de presición del plumón, pero finalmente logrando la meta tras ajustes.


