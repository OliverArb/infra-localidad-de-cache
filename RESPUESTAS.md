# Respuestas

Nombre y código: Oliver De Jesus Arboleda Baez - 202459684

## Parte 3: el paso

¿Hasta qué paso el tiempo baja menos que las operaciones, y a partir de cuál
baja al mismo ritmo? Relacione ese punto con el tamaño de un `int` y de una
línea de caché.
Del paso 1 al 32 el tiempo apenas baja (de 11,6 a 8,4 ms) aunque las operaciones se reducen a la mitad en cada paso, porque se siguen trayendo las mismas líneas de caché y el costo lo pone la memoria, no el cómputo. Desde el paso 64 el tiempo baja al ritmo de las operaciones (4,2 → 2,1 → 1,1 ms) y los ns por operación se estabilizan en ≈ 8 ns, el costo de traer una línea. Un int ocupa 4 bytes y una línea de 64 bytes guarda 16 enteros, así que con 64 bytes el cambio ocurre en el paso 16. En mi Mac (línea de 128 bytes, 32 enteros) ocurre en el paso 32.

## Parte 4: speedup, eficiencia y la ley de Amdahl

Con los tiempos de `amdahl.txt`:

| Hilos | Total medido | Speedup medido | Eficiencia | Speedup según Amdahl |
|---:|---:|---:|---:|---:|
| 1 | 2568.3 | 1,00 | 1,00 | 1,00 |
| 2 | 1500.2 | 1,71 | 0,86 | 1,71 |
| 4 | 994.0 | 2,58 | 0,65 | 2,66 |
| 8 | 821.5 | 3,13 | 0,39 | 3,67 |

Fracción paralelizable `p` (parte paralela sobre el total, con un hilo):

Techo del speedup con esa `p`, `1 / (1 - p)`:

¿Dónde se separa la columna medida de la que predice Amdahl, y qué lo
explica?

Con 2 y 4 hilos la medición sigue casi la curva de Amdahl. Se separa claramente en 8 hilos: mides ≈ 3,1 y la fórmula promete ≈ 3,7. Eso se explica por tres cosas:

Núcleos disponibles. Tu Mac tiene un número limitado de núcleos, y en Apple Silicon no todos son de rendimiento. Con 8 hilos, algunos caen en núcleos más lentos o se pelean por los mismos, así que no hay 8 unidades iguales trabajando.
Costo de crear y unir hilos, y de repartir el trabajo, que Amdahl no modela.
Memoria compartida. Todos los hilos leen el mismo arreglo de 10 millones de long y comparten ancho de banda y cachés, y ese factor crece con los hilos.

Amdahl supone que la parte paralela se divide perfectamente y sin costo, y que la secuencial es fija. Por eso la fórmula es un techo optimista, no una predicción exacta.