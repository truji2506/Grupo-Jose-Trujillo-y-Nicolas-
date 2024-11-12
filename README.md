# Grupo-Jose-Trujillo-y-Nicolas-Quintero

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

2. Funcionamiento
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

La diferencia entre los codigos dos codigos de fragmentos de codigo esta en el orden que son creados y esperados
primer parte del codigo 
```c
pthread_create(&threadID1, NULL, &imprime, &threadParam1);
pthread_join(threadID1, NULL);
pthread_create(&threadID2, NULL, &imprime, &threadParam2);
pthread_join(threadID2, NULL);
```
Crea el primer hilo treadID1 y espera a que termine antes de crear el segundo hilo threadID2

Por lo que evidencio es que lo hilos se ejecutan uno despues del otro, de forma secuencial. threadID2 no comenzará hasta que threadID1 haya terminado completamente.

Segunda parte del codigo 
```c
pthread_create(&threadID1,NULL,&imprime, &threadParam1);
pthread_create(&threadID2,NULL,&imprime, &threadParam2);
pthread_join(threadID1,NULL);
pthread_join(threadID2,NULL);
```
Crea ambos hilos (threadID1 y threadID2) casi al mismo tiempo y luego espera que ambos terminen.
Aquí, los hilos pueden ejecutarse en simultaneo, permitiendo que ambas tareas se realicen de manera coincidente.

## Ejercicio 7
¿Cómo se crea un hilo?

La creación de un hilo en SDL2

En SDL2, los hilos se crean utilizando la función SDL_CreateThread(), que inicia un nuevo hilo de ejecución.​

Ejemplo que se use un hilo de SDL2

```c
#include <SDL2/SDL.h>
#include <stdio.h>

// Función que será ejecutada en un hilo separado​
int hiloFuncion(void* data) {
    printf("Hola desde el hilo!\n");
    return 0;
}

int main(int argc, char* argv[]) {​
    // Inicializar SDL
    if (SDL_Init(SDL_INIT_VIDEO) != 0) {
        printf("Error al inicializar SDL: %s\n", SDL_GetError());​
        return 1;
    }

    // Crear el hilo
    SDL_Thread* hilo = SDL_CreateThread(hiloFuncion, "HiloEjemplo", NULL);​
    if (hilo == NULL) {
        printf("Error al crear el hilo: %s\n", SDL_GetError());
        SDL_Quit();​
        return 1;
    }

    // Esperar a que el hilo termine
    int resultado;
    SDL_WaitThread(hilo, &resultado);​

    printf("Hilo terminado con resultado: %d\n", resultado);

    SDL_Quit();
    return 0;
}
```
¿Cuál es el equivalente de join en la API de SDL2?
El equivalente de join en la API de SDL2 es la función SDL_WaitThread. Esta función permite que el hilo principal (o cualquier otro hilo) espere a que un hilo específico termine su ejecución

¿Para qué sirven los semáforos en SDL2?
Los semáforos en SDL2 se utilizan para controlar el acceso a recursos compartidos entre hilos, evitando condiciones de carrera y asegurando la sincronización.​

## ACTIVIDAD 8 

Se creo un proyecto nuevo en el visual para poder correr el programa planteado 

![image](https://github.com/user-attachments/assets/78d7a6e9-7285-40e0-976e-829dedfed756)

Además se le creo las constantes en el constant.c y corre de esta forma 

![image](https://github.com/user-attachments/assets/318595a1-4a73-409c-bf77-578e25bb33df)


![image](https://github.com/user-attachments/assets/316385f2-6ea8-4ca4-9c3a-5d876e8dd4e9)

Se añadio el sonido .WAV a la carpeta de visual al momento de reproducirlo con la letra P sale esto 

![image](https://github.com/user-attachments/assets/70eb8a52-2c30-49f9-b0b7-285a0329dc20)

A la final pudimmos hacer que reproduzca el sonido, nos aseguramos que el archivo se llamara tap y se llevo al directorio ppal del proyecto, nos aseguramos que todo el codigo que estuviera comentado no lo estuviera como la parte del audioSpec, no dimos cuenta que en el mismo codigo decia que para reproducir usar la letra P, Pero a momento de reproducir el sonido una segunda vez se queda cargando 

![image](https://github.com/user-attachments/assets/954d3eaf-8357-49fb-96f7-54ef4ba7c215)

Realizando la respectiva verificacion del codigo, nos damos cuenta que el audio se reproduce una vez, pero si se vuelve a presionar P antes de que termine la reproducción actual, el programa intenta reproducir el mismo archivo en paralelo, lo que puede causar errores como el que nos paso de que se queda cargando el programa

Por ende se añadio un semaforo AudioSemaphore para controlar la reproducción de audio y evitar que se reproduzca varias veces el sonido y se pueda controlar
Inicializamos el semáforo en initialize_window() con un valor inicial de 1, lo que nos permite acceso a una única reproducción de audio a la vez
```c
audioSemaphore = SDL_CreateSemaphore(1);
```
Ademas se realizo una modificación en Play_audio() para que antes de iniciar la reproducción de audio, el semaforo es bloqueado usando SDL_SemWait(audioSemaphore), esto nos ayuda a controlar otros audios mientras uno se encuentre en reproducción 

y utilizandoo el semaforo se libera con SDL_SemPost(audioSemaphore), permitiendo que el audio este disponible para la proxima reproduccion
```c
SDL_SemWait(audioSemaphore); // Espera hasta que el audio esté disponible para reproducir
SDL_SemPost(audioSemaphore); // Señal de que el audio está libre
```

Tuvimos algunos inconvenientes en la creacion del hilo y tuvimos que realizar una consulta a chatgpt para que la reproduccion del audio sea independiente y que no bloquee el resto del juego y nos ayudo con esto 
Se modifica el bloque process_input() para lanzar play_audio() en un hilo cuando se presiona la tecla P, pero solo si el semáforo indica que el audio está libre (SDL_SemValue(audioSemaphore) > 0).
```c
if (SDL_SemValue(audioSemaphore) > 0) {
    SDL_CreateThread((SDL_ThreadFunction)play_audio, "AudioThread", NULL);
}
```
y ya en destroy_window() se agrega la destruccion del semaforo para liberar recursos cuando el programa termina
```c
SDL_DestroySemaphore(audioSemaphore);
```
![image](https://github.com/user-attachments/assets/561bc615-2609-4057-9644-c20b75d01b4b)

## ACTIVIDAD 9

Analizando el ejemplo presentado en esta actividad, nos damos cuenta que hubo una declaración de variable shared donde mas adelante del codigo se usaran el los hilos y su valor inicial es 0
Se definio la funcion function la cual se ejecutara cuando los hilos se inicien, lo que obtuvimos en la consulta que realizamos fue que la unica funcion que hace es incrementar el valor de shared en 1
despues vemos los hilos como thr1 y thr2 usando pthread_create y cada hilo ejecutara function
despues de la creación de los hilos pthread_join asegura que el programa espere a que thr1 y thr2 terminen de ejecutarse antes de avanzar, sin pthread_join el programa podria terminar antes de que los hilos completen sus tarea y finalmente se imprime el valor de shared, el valor de shared debería ser 2, ya que cada hilo incrementa shared en 1

## ACTIVIDAD 10

Cuando agregamos el bucle en la función function, incrementando la variable compartida shared dentro de un ciclo de ITERATIONS (100 en este caso), el problema de la condición de carrera es más evidente. Esto es porque ahora cada hilo intenta modificar shared muchas veces, aumentando la probabilidad de que se interrumpan mutuamente y causen resultados inesperados.
Cada hilo realizará shared++ 100 veces, por lo que esperaríamos que el valor final de shared sea 200 (100 incrementos por cada uno de los dos hilos). Sin embargo, al incrementar el número de iteraciones, puede que el valor impreso no sea 200 sino algo menor. Esto es debido a que los hilos acceden y modifican shared sin ninguna sincronización, provocando una race condition.
La instrucción shared++ implica varias operaciones a nivel de máquina:

Leer el valor actual de shared.
Incrementar el valor leído en 1.
Escribir el valor incrementado de vuelta en shared.
Cuando no existe sincronización, los dos hilos pueden leer el mismo valor de shared casi al mismo tiempo, incrementar ese mismo valor y escribirlo, provocando que algunas actualizaciones se pierdan.

## ACTIVIDAD 11

Este programa en C crea dos hilos (thr1 y thr2) que acceden a un recurso compartido, la variable global shared. Cada hilo incrementa esta variable en cada iteración de un bucle de 100 iteraciones. Para evitar condiciones de carrera (race conditions), el programa utiliza un mutex (mxShared) que asegura que solo un hilo a la vez pueda modificar shared.

Análisis del código
Recurso compartido: La variable global shared se usa como recurso compartido que ambos hilos intentarán incrementar.

Exclusión mutua con mutex: El mutex (mxShared) se inicializa en el main() y se utiliza en la función function() que ejecutan ambos hilos. Dentro del bucle de la función, el mutex asegura la exclusión mutua:

pthread_mutex_lock(&mxShared); bloquea el mutex antes de que un hilo incremente la variable shared.
pthread_mutex_unlock(&mxShared); libera el mutex después de que el hilo haya incrementado la variable, permitiendo que el otro hilo pueda acceder al recurso compartido.
Ejecución y sincronización de hilos:

Se crean los hilos thr1 y thr2 con pthread_create, que ejecutan la función function.
pthread_join() asegura que el main() espere a que ambos hilos terminen su ejecución antes de imprimir el valor de shared.
Destrucción del mutex: Después de que ambos hilos han terminado, el mutex es destruido con pthread_mutex_destroy(&mxShared);.

Salida esperada
La variable shared debe tener el valor 200 al final (100 incrementos por cada hilo) ya que la exclusión mutua evita que los incrementos se pierdan debido a una race condition.

Conclusión
Gracias al uso del mutex, este programa evita condiciones de carrera, asegurando que el recurso compartido shared se incremente de forma consistente y segura por ambos hilos.

## ACTIVIDAD 12

En este programa, se usa un semáforo (semMessage) para controlar el acceso de múltiples hilos a una sección crítica en lugar de un mutex. Vamos a analizar cómo funciona este mecanismo y en qué se diferencia del uso de un mutex.

Análisis del código
Declaración e inicialización del semáforo:

Se declara un semáforo (sem_t semMessage) que controla el acceso a la sección crítica.
sem_init(&semMessage, 0, 4); inicializa el semáforo con un valor de 4, lo que permite que hasta 4 hilos puedan ejecutar la sección crítica simultáneamente. La inicialización del semáforo con un valor mayor a 1 es una característica distintiva de los semáforos, en contraste con los mutex, que solo permiten un acceso a la vez.
Función routine:

Dentro de routine, cada hilo llama a sem_wait(&semMessage); antes de entrar en la sección crítica. Esto decrementa el contador del semáforo. Si el contador es mayor que 0, el hilo entra en la sección crítica; si es 0, el hilo se bloquea hasta que otro hilo libere el semáforo.
La línea sem_post(&semMessage); se ejecuta al final de la sección crítica para incrementar el contador del semáforo y liberar el acceso, permitiendo que otros hilos entren en la sección crítica.
Esta función también libera la memoria asignada para el identificador del hilo (free(args);).
Creación y ejecución de hilos:

El main crea 4 hilos (THREAD_NUM = 4) y les pasa un identificador único como argumento.
pthread_create(&th[i], NULL, &routine, a); inicia cada hilo con la función routine.
Luego, pthread_join(th[i], NULL); asegura que el main espere a que todos los hilos terminen antes de continuar y destruir el semáforo.
Destrucción del semáforo:

Después de que todos los hilos terminan, sem_destroy(&semMessage); libera los recursos asociados con el semáforo.
Diferencias clave entre mutex y semáforos
Control de acceso múltiple:

Un mutex solo permite que un hilo a la vez acceda a una sección crítica. Es ideal cuando solo un hilo debe tener acceso exclusivo.
Un semáforo puede configurarse para permitir que múltiples hilos accedan a la sección crítica al mismo tiempo, dependiendo del valor de inicialización. En este caso, el semáforo permite que hasta 4 hilos accedan simultáneamente.
Uso en sincronización:

Los mutexes se usan más comúnmente para sincronizar el acceso a recursos compartidos, asegurando que solo un hilo entre a la vez.
Los semáforos pueden usarse también para la sincronización de múltiples hilos, permitiendo o restringiendo el acceso simultáneo en función de su valor.
Salida esperada
Dado que el semáforo permite que 4 hilos accedan simultáneamente, los mensajes de "Hello from thread X" pueden aparecer en cualquier orden, ya que todos los hilos tienen permiso para acceder sin esperar. Sin embargo, todos imprimirán su mensaje sin problemas de acceso simultáneo a la misma sección crítica.

Conclusión
El uso de un semáforo con un valor mayor a 1 permite sincronizar hilos sin restringirlos tanto como lo haría un mutex, proporcionando flexibilidad para control de concurrencia en sistemas multihilo.

## CODIGO 
### main.c
```c
#include "game.h"

int main(int argc, char* argv[]) {
    Game game;
    if (!Game_Init(&game)) return -1;

    while (game.running) {
        Game_HandleEvents(&game);
        Game_Update(&game);
        Game_Render(&game);
    }

    Game_Clean(&game);
    return 0;
}
```
### game.h
```c
#ifndef GAME_H
#define GAME_H

#include <SDL.h>
#include "player.h"
#include "enemy.h"
#include "powerup.h"
#include "audio.h"

typedef struct {
    SDL_Window* window;
    SDL_Renderer* renderer;
    int running;
    Player player;
    Enemy enemies[4];
    PowerUp powerUp;
} Game;

int Game_Init(Game* game);
void Game_HandleEvents(Game* game);
void Game_Update(Game* game);
void Game_Render(Game* game);
void Game_Clean(Game* game);

#endif
```
### game.c
```c
#include "game.h"
#include "audio.h"
#include <stdlib.h>

int Game_Init(Game* game) {
    if (SDL_Init(SDL_INIT_VIDEO | SDL_INIT_AUDIO) != 0) {
        SDL_Log("Error inicializando SDL: %s", SDL_GetError());
        return 0;
    }

    game->window = SDL_CreateWindow("Juego de SDL", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, 800, 600, 0);
    if (!game->window) {
        SDL_Log("Error creando ventana: %s", SDL_GetError());
        SDL_Quit();
        return 0;
    }

    game->renderer = SDL_CreateRenderer(game->window, -1, SDL_RENDERER_ACCELERATED);
    if (!game->renderer) {
        SDL_Log("Error creando renderer: %s", SDL_GetError());
        SDL_DestroyWindow(game->window);
        SDL_Quit();
        return 0;
    }

    if (InitAudio("sounds/background.wav") != 0) {
        SDL_Log("Error inicializando audio");
        return 0;
    }
    PlayGameStartSound();

    Player_Init(&game->player, game->renderer, 400, 300);
    for (int i = 0; i < 4; i++) {
        Enemy_Init(&game->enemies[i], game->renderer, (i % 2) * 750 + 50, (i / 2) * 550 + 50);
    }
    PowerUp_Init(&game->powerUp, game->renderer, rand() % 750 + 25, rand() % 550 + 25);

    game->running = 1;
    return 1;
}

void Game_HandleEvents(Game* game) {
    SDL_Event event;
    while (SDL_PollEvent(&event)) {
        if (event.type == SDL_QUIT) {
            game->running = 0;
        }
        Player_HandleInput(&game->player, event);
    }
}

void Game_Update(Game* game) {
    Player_Update(&game->player);

    for (int i = 0; i < 4; i++) {
        Enemy_Update(&game->enemies[i], &game->player);
        if (Enemy_IsAlive(&game->enemies[i]) && CheckCollision(&game->player.rect, &game->enemies[i].rect)) {
            PlayCollisionSound();
            Enemy_TakeDamage(&game->enemies[i]);
        }
    }

    if (Player_CollectsPowerUp(&game->player, &game->powerUp)) {
        PlayPowerUpSound();
        Player_Upgrade(&game->player);
        PowerUp_Init(&game->powerUp, game->renderer, rand() % 750 + 25, rand() % 550 + 25);
    }

    if (game->player.health <= 0) {
        PlayGameOverSound();
        game->running = 0;
    }
}

void Game_Render(Game* game) {
    SDL_SetRenderDrawColor(game->renderer, 0, 0, 0, 255);
    SDL_RenderClear(game->renderer);

    Player_Render(&game->player);
    for (int i = 0; i < 4; i++) {
        Enemy_Render(&game->enemies[i]);
    }
    PowerUp_Render(&game->powerUp);

    SDL_RenderPresent(game->renderer);
}

void Game_Clean(Game* game) {
    CloseAudio();
    SDL_DestroyRenderer(game->renderer);
    SDL_DestroyWindow(game->window);
    SDL_Quit();
}
```
### player.h
```c
#ifndef PLAYER_H
#define PLAYER_H

#include <SDL.h>
#include "bullet.h"
#include "powerup.h"

typedef struct {
    SDL_Renderer* renderer;
    SDL_Rect rect;
    int health;
    int speed;
    int powerLevel;
} Player;

void Player_Init(Player* player, SDL_Renderer* renderer, int x, int y);
void Player_HandleInput(Player* player, SDL_Event event);
void Player_Update(Player* player);
void Player_Render(Player* player);
void Player_Upgrade(Player* player);
int Player_CollectsPowerUp(Player* player, PowerUp* powerUp);

#endif
```
### player.c
```c
#include "player.h"
#include <math.h>

void Player_Init(Player* player, SDL_Renderer* renderer, int x, int y) {
    player->renderer = renderer;
    player->rect.x = x;
    player->rect.y = y;
    player->rect.w = 50;
    player->rect.h = 50;
    player->powerLevel = 1;
    player->speed = 5;
}

void Player_HandleInput(Player* player, SDL_Event event, Bullet bullets[], int maxBullets) {
    const Uint8* keystate = SDL_GetKeyboardState(NULL);

    // Movimiento con teclas W, A, S, D
    if (keystate[SDL_SCANCODE_W]) player->rect.y -= player->speed;
    if (keystate[SDL_SCANCODE_S]) player->rect.y += player->speed;
    if (keystate[SDL_SCANCODE_A]) player->rect.x -= player->speed;
    if (keystate[SDL_SCANCODE_D]) player->rect.x += player->speed;

    // Disparo con clic izquierdo en dirección al mouse
    if (event.type == SDL_MOUSEBUTTONDOWN && event.button.button == SDL_BUTTON_LEFT) {
        int mouseX, mouseY;
        SDL_GetMouseState(&mouseX, &mouseY);

        float dx = mouseX - (player->rect.x + player->rect.w / 2);
        float dy = mouseY - (player->rect.y + player->rect.h / 2);
        float length = sqrtf(dx * dx + dy * dy);

        if (length != 0) {
            dx /= length;
            dy /= length;
        }

        for (int i = 0; i < maxBullets; i++) {
            if (!bullets[i].active) {
                Bullet_Init(&bullets[i], player->renderer, player->rect.x + player->rect.w / 2, player->rect.y + player->rect.h / 2, dx, dy);
                break;
            }
        }
    }
}

void Player_Update(Player* player) {
    // Limitamos al jugador dentro de la ventana
    if (player->rect.x < 0) player->rect.x = 0;
    if (player->rect.x > 750) player->rect.x = 750;
    if (player->rect.y < 0) player->rect.y = 0;
    if (player->rect.y > 550) player->rect.y = 550;
}

void Player_Render(Player* player) {
    SDL_SetRenderDrawColor(player->renderer, 0, 255, 0, 255);
    SDL_RenderFillRect(player->renderer, &player->rect);
}

int Player_CollidesWithPowerUp(Player* player, PowerUp* powerUp) {
    return SDL_HasIntersection(&player->rect, &powerUp->rect);
}

void Player_Upgrade(Player* player) {
    player->powerLevel++;
    player->speed++;
}
```
### Enemy.h
```c
#ifndef ENEMY_H
#define ENEMY_H

#include <SDL.h>
#include "player.h"

typedef struct {
    SDL_Renderer* renderer;
    SDL_Rect rect;
    int health;
    int speed;
} Enemy;

void Enemy_Init(Enemy* enemy, SDL_Renderer* renderer, int x, int y);
void Enemy_Update(Enemy* enemy, Player* player);
void Enemy_Render(Enemy* enemy);
int Enemy_IsAlive(Enemy* enemy);
void Enemy_TakeDamage(Enemy* enemy);

#endif
```
### Enemy.c
```c
#ifndef ENEMY_H
#define ENEMY_H

#include <SDL.h>
#include "player.h"

typedef struct {
    SDL_Renderer* renderer;
    SDL_Rect rect;
    int health;
    int speed;
} Enemy;

void Enemy_Init(Enemy* enemy, SDL_Renderer* renderer, int x, int y);
void Enemy_Update(Enemy* enemy, Player* player);
void Enemy_Render(Enemy* enemy);
int Enemy_IsAlive(Enemy* enemy);
void Enemy_TakeDamage(Enemy* enemy);

#endif
#include "enemy.h"
#include <math.h>

void Enemy_Init(Enemy* enemy, SDL_Renderer* renderer, int x, int y) {
    enemy->renderer = renderer;
    enemy->rect.x = x;
    enemy->rect.y = y;
    enemy->rect.w = 40;
    enemy->rect.h = 40;
    enemy->health = 3;
    enemy->speed = 1;  // Velocidad ajustada para seguir al jugador suavemente
}

void Enemy_Update(Enemy* enemy, Player* player) {
    if (enemy->health > 0) {
        float dx = player->rect.x - enemy->rect.x;
        float dy = player->rect.y - enemy->rect.y;
        float distance = sqrtf(dx * dx + dy * dy);

        if (distance != 0) {
            dx = (dx / distance) * enemy->speed;
            dy = (dy / distance) * enemy->speed;

            enemy->rect.x += (int)dx;
            enemy->rect.y += (int)dy;
        }
    }
}

void Enemy_Render(Enemy* enemy) {
    if (enemy->health > 0 && enemy->renderer) {
        SDL_SetRenderDrawColor(enemy->renderer, 255, 0, 0, 255);
        SDL_RenderFillRect(enemy->renderer, &enemy->rect);
    }
}

int Enemy_IsAlive(Enemy* enemy) {
    return enemy->health > 0;
}

void Enemy_TakeDamage(Enemy* enemy) {
    if (enemy->health > 0) {
        enemy->health--;
    }
}
```
### powerup.h
```c
#ifndef POWERUP_H
#define POWERUP_H

#include <SDL.h>

typedef struct {
    SDL_Renderer* renderer;
    SDL_Rect rect;
    int active;
} PowerUp;

void PowerUp_Init(PowerUp* powerUp, SDL_Renderer* renderer, int x, int y);
void PowerUp_Render(PowerUp* powerUp);
void PowerUp_Deactivate(PowerUp* powerUp);

#endif
```
### powerup.c
```c
#include "powerup.h"

void PowerUp_Init(PowerUp* powerUp, SDL_Renderer* renderer, int x, int y) {
    powerUp->renderer = renderer;
    powerUp->rect.x = x;
    powerUp->rect.y = y;
    powerUp->rect.w = 20;
    powerUp->rect.h = 20;
    powerUp->active = 1;
}

void PowerUp_Render(PowerUp* powerUp) {
    if (powerUp->active && powerUp->renderer) {
        SDL_SetRenderDrawColor(powerUp->renderer, 0, 0, 255, 255);
        SDL_RenderFillRect(powerUp->renderer, &powerUp->rect);
    }
}

void PowerUp_Deactivate(PowerUp* powerUp) {
    powerUp->active = 0;
}
```
### Bullet.c
```c
#include "bullet.h"

void Bullet_Init(Bullet* bullet, SDL_Renderer* renderer, int x, int y, float dirX, float dirY) {
    bullet->renderer = renderer;
    bullet->rect.x = x;
    bullet->rect.y = y;
    bullet->rect.w = 5;
    bullet->rect.h = 10;
    bullet->active = 1;
    bullet->speed = 10;
    bullet->dirX = dirX;
    bullet->dirY = dirY;
}

void Bullet_Update(Bullet* bullet) {
    if (bullet->active) {
        bullet->rect.x += (int)(bullet->speed * bullet->dirX);
        bullet->rect.y += (int)(bullet->speed * bullet->dirY);

        if (bullet->rect.x < 0  bullet->rect.x > 800  bullet->rect.y < 0 || bullet->rect.y > 600) {
            Bullet_Deactivate(bullet);
        }
    }
}

void Bullet_Render(Bullet* bullet) {
    if (bullet->active && bullet->renderer && bullet->rect.w > 0 && bullet->rect.h > 0) {
        SDL_SetRenderDrawColor(bullet->renderer, 255, 255, 0, 255);
        SDL_RenderFillRect(bullet->renderer, &bullet->rect);
    }
}

void Bullet_Deactivate(Bullet* bullet) {
    bullet->active = 0;
}

int Bullet_IsActive(Bullet* bullet) {
    return bullet->active;
}

int Bullet_CollidesWith(Bullet* bullet, Enemy* enemy) {
    if (bullet->active && Enemy_IsAlive(enemy) && SDL_HasIntersection(&bullet->rect, &enemy->rect)) {
        Bullet_Deactivate(bullet);
        Enemy_TakeDamage(enemy);
        return 1;
    }
    return 0;
}
```
### Bullet.h
```c
#ifndef BULLET_H
#define BULLET_H

#include <SDL.h>
#include "enemy.h"

typedef struct {
    SDL_Renderer* renderer;
    SDL_Rect rect;
    int active;
    int speed;
    float dirX;
    float dirY;
} Bullet;

void Bullet_Init(Bullet* bullet, SDL_Renderer* renderer, int x, int y, float dirX, float dirY);
void Bullet_Update(Bullet* bullet);
void Bullet_Render(Bullet* bullet);
void Bullet_Deactivate(Bullet* bullet);
int Bullet_IsActive(Bullet* bullet);
int Bullet_CollidesWith(Bullet* bullet, Enemy* enemy);

#endif
```
### Audio.h
```c
#ifndef AUDIO_H
#define AUDIO_H

int InitAudio(const char* file);
void PlaySound(const char* file);
void CloseAudio();

void PlayGameStartSound();
void PlayPowerUpSound();
void PlayCollisionSound();
void PlayGameOverSound();
void PlayBackgroundSound();

#endif
```
### Audio.c
```c
#include <SDL.h>
#include "audio.h"

SDL_sem* audioSemaphore;
Uint8* audioBuffer;
Uint32 audioLength;
SDL_AudioSpec audioSpec;

void AudioCallback(void* userdata, Uint8* stream, int len) {
    if (audioLength == 0) {
        SDL_SemPost(audioSemaphore);
        return;
    }

    len = (len > audioLength ? audioLength : len);
    SDL_memcpy(stream, audioBuffer, len);
    audioBuffer += len;
    audioLength -= len;
}

int InitAudio(const char* file) {
    if (SDL_LoadWAV(file, &audioSpec, &audioBuffer, &audioLength) == NULL) {
        SDL_Log("Error cargando archivo de sonido: %s", SDL_GetError());
        return -1;
    }

    audioSpec.callback = AudioCallback;
    if (SDL_OpenAudio(&audioSpec, NULL) < 0) {
        SDL_Log("Error inicializando audio: %s", SDL_GetError());
        return -1;
    }

    audioSemaphore = SDL_CreateSemaphore(1);
    return 0;
}

void PlaySound(const char* file) {
    SDL_SemWait(audioSemaphore);

    if (SDL_LoadWAV(file, &audioSpec, &audioBuffer, &audioLength) == NULL) {
        SDL_Log("Error cargando archivo de sonido: %s", SDL_GetError());
        return;
    }

    SDL_PauseAudio(0);
}

void CloseAudio() {
    SDL_CloseAudio();
    SDL_FreeWAV(audioBuffer);
    SDL_DestroySemaphore(audioSemaphore);
}

void PlayGameStartSound() { PlaySound("sounds/start.wav"); }
void PlayPowerUpSound() { PlaySound("sounds/powerup.wav"); }
void PlayCollisionSound() { PlaySound("sounds/collision.wav"); }
void PlayGameOverSound() { PlaySound("sounds/gameover.wav"); }
void PlayBackgroundSound() { PlaySound("sounds/background.wav"); }
```


