> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-teclado-matricial/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Sin librería
```cpp
const unsigned long period = 50;
unsigned long prevMillis = 0;

byte iRow = 0, iCol = 0;
const byte countRows = 4;
const byte countColumns = 4;

const byte rowsPins[countRows] = { 11, 10, 9, 8 };
const byte columnsPins[countColumns] = { 7, 6, 5, 4 };

char keys[countRows][countColumns] = {
  { '1','2','3', 'A' },
  { '4','5','6', 'B' },
  { '7','8','9', 'C' },
  { '#','0','*', 'D' }
};

// Leer el estado del teclado
bool readKeypad()
{
  bool rst = false;

  // Barrido de columnas
  for (byte c = 0; c < countColumns; c++)
  {
    // Poner columna a LOW
    pinMode(columnsPins[c],OUTPUT);
    digitalWrite(columnsPins[c], LOW);
    
    // Barrer todas las filas comprobando pulsaciones
    for (byte r = 0; r < countRows; r++)
    {
      if (digitalRead(columnsPins[r]) == LOW)   
      {
        // Pulsacion detectada, guardar fila y columna
        iRow = r;
        iCol = c;
        rst = true; 
      }
    }
    // Devolver la columna a alta impedancia
    digitalWrite(columnsPins[c], HIGH);
    pinMode(columnsPins[c], INPUT);
  }
  return rst;
}

// Inicializacion
void setup()
{
  Serial.begin(9600);

  // Columnas en alta impedancia
  for (byte c = 0; c < countColumns; c++)
  {
    pinMode(columnsPins[c], INPUT);
    digitalWrite(columnsPins[c], HIGH);
  }

  // Filas en pullup
  for (byte r = 0; r < countRows; r++)
  {
    pinMode(rowsPins[r], INPUT_PULLUP);
  }
}

void loop()
{
  if (millis() - prevMillis > period)   // Espera no bloqueante
  {
    prevMillis = millis();
    if (readKeypad())   // Detección de tecla pulsada
    {
      Serial.println(keys[iRow][iCol]);   // Mostrar tecla
    }
  }
}
```


## Con librería
```cpp
#include <Keypad.h>
 
const byte rowsCount = 4;
const byte columsCount = 4;
 
char keys[rowsCount][columsCount] = {
   { '1','2','3', 'A' },
   { '4','5','6', 'B' },
   { '7','8','9', 'C' },
   { '#','0','*', 'D' }
};
 
const byte rowPins[rowsCount] = { 11, 10, 9, 8 };
const byte columnPins[columsCount] = { 7, 6, 5, 4 };
 
Keypad keypad = Keypad(makeKeymap(keys), rowPins, columnPins, rowsCount, columsCount);
 
void setup() {
   Serial.begin(9600);
}
 
void loop() {
   char key = keypad.getKey();
 
   if (key) {
      Serial.println(key);
   }
}
```


