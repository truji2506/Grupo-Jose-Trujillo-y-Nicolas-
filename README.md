# Grupo-Jose-Trujillo-y-Nicolas-

## Ejercicio 1


Un sistema operativo (SO) es un software esencial que actúa como intermediario entre el hardware de la computadora y los programas de usuario. Su principal función es gestionar y 4
coordinar los recursos de hardware (como el procesador, memoria, dispositivos de almacenamiento, y dispositivos de entrada/salida) y los recursos de software (como los programas y aplicaciones)
de forma eficiente y segura.
Además de esta administración, el sistema operativo también proporciona una serie de servicios que los programas y usuarios pueden utilizar para interactuar con el hardware, como:

Gestión de procesos: Permite iniciar, pausar, reiniciar y terminar programas en ejecución, además de manejar la comunicación entre ellos.

Gestión de memoria: Controla cómo se asigna y utiliza la memoria, asegurando que cada proceso tenga acceso suficiente sin interferir con otros.

Gestión de archivos: Proporciona estructura y organización a los archivos almacenados, permitiendo la creación, lectura, escritura y eliminación de archivos.

Gestión de dispositivos: Administra el acceso a dispositivos de entrada y salida, facilitando la interacción con el hardware.

Seguridad y control de acceso: Protege los datos y recursos del sistema mediante autenticación y permisos de acceso.

En Linux, estos servicios están optimizados para la flexibilidad y seguridad, y te resultarán esenciales para explorar los conceptos y procesos operativos a lo largo de este ejercicio. 
Para profundizar en el tema, Operating Systems: Three Easy Pieces es un excelente libro introductorio que explica de forma accesible estos y otros aspectos de los sistemas operativos.

## Ejercicio 2

Un programa es un conjunto de instrucciones almacenadas en un archivo, diseñadas para realizar una tarea específica. Un proceso es la ejecución de esas instrucciones por parte de la CPU. 
Mientras que el programa es una entidad pasiva en el almacenamiento, el proceso es una entidad activa en ejecución 

Sí, es posible tener múltiples procesos ejecutando el mismo programa simultáneamente. Por ejemplo, en sistemas operativos multitarea, varios usuarios pueden abrir y utilizar la misma aplicación al mismo tiempo, 
cada uno en un proceso separado 

El stack (pila) de un proceso es una estructura de datos que sigue el principio LIFO (Last-In, First-Out). Se utiliza para almacenar información sobre las llamadas a funciones,
permitiendo que cada función en ejecución tenga su propio espacio de variables locales y parámetros. Cuando una función termina, su información se elimina de la pila, asegurando que las variables 
locales no interfieran entre sí 

El heap (montón) de un proceso es una estructura de datos que se utiliza para gestionar la memoria dinámica. Permite almacenar y recuperar datos de manera eficiente, 
especialmente en algoritmos de ordenación y en la gestión de colas de prioridad 

La zona de texto de un proceso es la sección de la memoria donde se almacenan las instrucciones ejecutables del programa. 
Es una de las cuatro secciones principales de la memoria de un proceso, junto con la zona de datos, la pila y el montón.

Las variables globales inicializadas se almacenan en la zona de datos del proceso, específicamente en el segmento de datos inicializados. 
Este segmento contiene todas las variables globales y estáticas que tienen un valor inicial definido en el programa.

Las variables globales no inicializadas se almacenan en la zona de datos del proceso, pero en un segmento diferente llamado BSS (Block Started by Symbol). 
Este segmento contiene las variables globales y estáticas que no tienen un valor inicial definido y que se inicializan a cero o a un valor nulo por defecto.

Los estados de un proceso pueden variar según el sistema operativo, pero en general incluyen:
Nuevo: El proceso está en creación.
Listo: El proceso está preparado para ejecutarse.
Ejecución: El proceso está en ejecución.
Espera: El proceso está esperando un evento o recurso.
Terminado: El proceso ha finalizado su ejecución.
Estos estados pueden variar en número y definición según el sistema operativo y su gestión de procesos 

## Ejercicio 4

1. Este programa tiene dos hilos:
Hilo principal: Ejecuta un bucle infinito que imprime "o".
Hilo creado: Ejecuta la función imprime_x() en un bucle infinito que imprime "x".

2.Funcionamiento
Al iniciar, el hilo principal crea un nuevo hilo con pthread_create(). Ambos hilos comparten el mismo espacio de memoria del proceso, 
pero cada uno tiene su propio stack para variables locales. El sistema operativo asigna tiempo de CPU a cada hilo, alternando entre ellos, 
por lo que en la consola se verá una mezcla de "x" y "o" imprimiéndose indefinidamente.

## Ejercicio 5

Al ejecutar el programa, es posible que no se vea ninguna salida en la consola o que se observe un resultado inconsistente. Esto ocurre porque, 
aunque los hilos se crean con pthread_create, el programa principal (hilo principal) termina casi inmediatamente debido a exit(EXIT_SUCCESS);, 
lo cual finaliza el proceso y detiene ambos hilos antes de que logren ejecutar el código de impresión.
Hipotesis
El problema es que el hilo principal termina su ejecución antes de que los hilos creados completen su tarea. Esto se puede solucionar utilizando pthread_join() 
para hacer que el hilo principal espere a que ambos hilos terminen de ejecutar su función antes de finalizar el programa.

## Ejercicio 6

Para esperar a que un hilo en particular termine, debes usar la función pthread_join(). Esta función bloquea la ejecución del hilo principal hasta que el hilo especificado complete su tarea. 
En el código anterior:
```c
pthread_join(threadID1, NULL);
pthread_join(threadID2, NULL);
```
Estas líneas aseguran que el hilo principal espere a que los hilos threadID1 y threadID2 terminen antes de que el programa finalice.
