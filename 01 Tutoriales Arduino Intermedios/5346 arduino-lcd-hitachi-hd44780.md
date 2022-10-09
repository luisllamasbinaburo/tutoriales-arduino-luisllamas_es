/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-lcd-hitachi-hd44780/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
/* Conexión
PIN2 - LCD4 (RS)
PIN3 - LCD5 (R/W)
PIN4 - LCD6 (E)

PIN8 - LCD11 (D4)
PIN9 - LCD12 (D5)
PIN10 - LCD13 (D6)
PIN11 - LCD14 (D7)
*/

#include <LiquidCrystal.h>			

LiquidCrystal lcd(2, 3, 4, 8, 9, 10, 11); //(RS,RW, E, D4,D5, D6, D7)

void setup()
{
	lcd.begin(16, 2);	// Inicia un LCD 16x02 (columnas,fila)
	lcd.setCursor(0, 0);	// Pone el cursor en las coordenadas (0,0)
	lcd.print("Linea 1");	// Escribe el LCD
	lcd.setCursor(0, 1);	// Ponemos el cursor en la segunda Fila
	lcd.print("Linea 2");   // Escribe el LCD
}

void loop()
{
	//Apagar cursor
	lcd.noCursor();
	delay(500);
	
	//Apagar encender cursor
	lcd.cursor();
	delay(500);
}
```

```cpp
/* Conexión
PIN2 - LCD4 (RS)
PIN3 - LCD5 (R/W)
PIN4 - LCD6 (E)

PIN8 - LCD11 (D4)
PIN9 - LCD12 (D5)
PIN10 - LCD13 (D6)
PIN11 - LCD14 (D7)
*/	
#include <LiquidCrystal.h>

LiquidCrystal lcd(2, 3, 4, 8, 9, 10, 11); //(RS,RW, E, D4,D5, D6, D7)

void setup() {
  lcd.begin(16, 2);
  
  lcd.print("¡Hola mundo!");
  delay(1000);
}

void loop() {
  
  //mover 13 posiciones (longitud del texto) a la izquierda
  //para sacar el texto de la pantalla por la izquierda
  for (int positionCounter = 0; positionCounter < 13; positionCounter++) {
    lcd.scrollDisplayLeft();
    delay(150);
  }

  //mover 29 posiciones (longitud del texto + ancho del display) a la derecha
  //para sacar el texto de la pantalla por la derecha
  for (int positionCounter = 0; positionCounter < 29; positionCounter++) {
    lcd.scrollDisplayRight();
    delay(150);
  }

  //mover 16 posiciones (29-13) a la izquierda
  //para devolver el texto a la posición inicial
  for (int positionCounter = 0; positionCounter < 16; positionCounter++) {
    lcd.scrollDisplayLeft();
    delay(150);
  }
  
  delay(1000);
}

```