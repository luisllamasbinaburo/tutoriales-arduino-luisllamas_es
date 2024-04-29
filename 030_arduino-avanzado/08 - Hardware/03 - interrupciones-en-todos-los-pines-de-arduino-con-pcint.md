> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/interrupciones-en-todos-los-pines-de-arduino-con-pcint/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## PCINT ejemplo de código
```cpp
// Activar PCINT en un PIN
void pciSetup(byte pin)
{
    *digitalPinToPCMSK(pin) |= bit (digitalPinToPCMSKbit(pin));  // activar pin en PCMSK
    PCIFR  |= bit (digitalPinToPCICRbit(pin)); // limpiar flag de la interrupcion en PCIFR
    PCICR  |= bit (digitalPinToPCICRbit(pin)); // activar interrupcion para el grupo en PCICR
}

// Definir ISR para cada puerto
ISR (PCINT0_vect) 
{    
    // gestionar para PCINT para D8 a D13
}

ISR (PCINT1_vect) 
{
    // gestionar PCINT para A0 a A5
}  

ISR (PCINT2_vect) 
{
    // gestionar PCINT para D0 a D7
}  

void setup() 
{  
  // Activar las PCINT para distintos pins
  pciSetup(7);
  pciSetup(8);
  pciSetup(9);
  pciSetup(A0);
}

void loop() 
{
}
```


## PCINT en una librería
```cpp
#define PCINT_PIN A5

#include <YetAnotherPcInt.h>

void pinChanged(const char* message, bool pinstate) {
  Serial.print(message);
  Serial.println(pinstate ? "HIGH" : "LOW");
}

void setup() {
  Serial.begin(115200);
  pinMode(PCINT_PIN, INPUT_PULLUP);
  PcInt::attachInterrupt(PCINT_PIN, pinChanged, "Pin has changed to ", CHANGE);
}

void loop() {}
```


