> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/como-emular-un-teclado-o-un-raton-con-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Arduino como teclado
```cpp
// Inicia y finaliza el teclado virtual
Keyboard.begin()
Keyboard.end()

// Escribe un texto usando el teclado
Keyboard.print()
Keyboard.println()
Keyboard.write()

// Pulsar y soltar una tecla
Keyboard.press()
Keyboard.release()
Keyboard.releaseAll()
```


## Ejemplo teclado sencillo
```cpp
#include <Keyboard.h>

void setup() {
  Keyboard.begin();
  delay(5000);
}

void loop() {
  Keyboard.println("Hola mundo!");
  delay(1000);
}
```


## Ejemplo teclado con combinaciones de teclas
```cpp
#include <Keyboard.h>

void setup() {
  Keyboard.begin();
}

void loop() {
  Keyboard.press(KEY_LEFT_CTRL);
  Keyboard.press('n');
  delay(100);
  Keyboard.releaseAll();
  
  // otra forma de hacer el release, tecla a tecla
  //Keyboard.release(KEY_LEFT_CTRL);
  //Keyboard.release('n');
  
  delay(1000);
}
```


## Ejemplo teclado abrir notepad
```cpp
#include <Keyboard.h>

void setup() {
  Keyboard.begin();
}

void loop() {
  Keyboard.press(KEY_RIGHT_GUI);
  Keyboard.press('r');
  delay(100);
  Keyboard.releaseAll();  
  delay(1000);
  
  Keyboard.println("notepad");
  
  Keyboard.press(KEY_RETURN);
  delay(100);
  Keyboard.releaseAll();
  
  Keyboard.print("Hola mundo!");
}
```


## Emular ratón con Arduino
```cpp
// Iniciar y detener el ratón virtual
Mouse.begin()
Mouse.end()

// Movimiento relativo del ratón
Mouse.move()

// Hacer click con el ratón
Mouse.click()
Mouse.press()
Mouse.release()
Mouse.isPressed()
```


## Ejemplo ratón sencillo
```cpp
#include "Mouse.h"

void setup() {
  Mouse.begin();
}

void loop() {
  Mouse.move(10, 10, 0);
  Mouse.click();
  delay(100);
}
```


