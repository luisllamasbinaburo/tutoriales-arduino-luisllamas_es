> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/como-cambiar-la-frecuencia-de-un-pwm-en-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Atmega 328p (Arduino Uno, Nano)
```cpp
//Atmega 328p (Arduino Uno, Nano)
// Frecuencias
// 5, 6   Timer0  62500 Hz
// 9, 10   Timer1  31250 Hz
// 3, 11   Timer2  31250 Hz

// Prescalers
// 5, 6   Timer0  1 8 64 256 1024
// 9, 10   Timer1  1 8 64 256 1024
// 3, 11   Timer2  1 8 32 64 128 256 1024

// Valores por defecto
// 5, 6   Timer0 64  977Hz
// 9, 10   Timer1 64  490Hz
// 3, 11   Timer2 64  490Hz

// Consecuencias
// 5, 6   Timer0  delay() y millis()
// 9, 10   Timer1  Librería servo
// 3, 11   Timer2  

void setPWMPrescaler(uint8_t pin, uint16_t prescale) {
  
  byte mode;
  
  if(pin == 5 || pin == 6 || pin == 9 || pin == 10) {
    switch(prescale) {
      case    1: mode = 0b001; break;
      case    8: mode = 0b010; break;
      case   64: mode = 0b011; break; 
      case  256: mode = 0b100; break;
      case 1024: mode = 0b101; break;
      default: return;
    }
    
  } else if(pin == 3 || pin == 11) {
    switch(prescale) {
      case    1: mode = 0b001; break;
      case    8: mode = 0b010; break;
      case   32: mode = 0b011; break; 
      case   64: mode = 0b100; break; 
      case  128: mode = 0b101; break;
      case  256: mode = 0b110; break;
      case 1024: mode = 0b111; break;
      default: return;
    }
  }
  
  if(pin==5 || pin==6) {
    TCCR0B = TCCR0B & 0b11111000 | mode;
  } else if (pin==9 || pin==10) {
    TCCR1B = TCCR1B & 0b11111000 | mode;
  } else if (pin==3 || pin==11) {
    TCCR2B = TCCR2B & 0b11111000 | mode;
  }
}
```


## Atmega 32U (Micro y Leonardo)
```cpp
// Atmega 32U
// Frecuencias
// 3, 11  Timer0  64500Hz
// 9, 10  Timer1  31250Hz
// 5  Timer3  31250Hz
// 6, 13  Timer4  31250Hz

// Prescalers
// 3, 11  Timer0  64  1 8 64 256 1024
// 9, 10  Timer1  64  1 8 64 256 1024
// 5  Timer3  64  1 8 64 256 1024
// 6, 13  Timer4  64  1 2 4 8 16 32 64 128 256 512 1024 2048 4096 8192 16384

// Valores por defecto
// 3, 11  Timer0  64  977Hz
// 9, 10  Timer1  64  490Hz
// 5  Timer3  64  490Hz
// 6, 13  Timer4  64  490Hz

// Conescuencias
// 3, 11  millis(), delay()
// 9, 10
// 5
// 6, 13

void setPWMPrescaler(uint8_t pin, uint16_t prescale) 
{ 
  byte mode;
  
  if(pin==3 || pin==5 || pin==9 || pin==10 || pin==11) { 
    switch(prescale) {
      case    1: mode = 0b001; break;
      case    8: mode = 0b010; break;
      case   64: mode = 0b011; break; 
      case  256: mode = 0b100; break;
      case 1024: mode = 0b101; break;
      default: return;
    }
    
    
  } else if(pin==6 || pin==13) {
    switch(prescale) {
      case     1: mode = 0b0001; break;
      case     2: mode = 0b0010; break;
      case     4: mode = 0b0011; break;
      case     8: mode = 0b0100; break;
      case    16: mode = 0b0101; break;
      case    32: mode = 0b0110; break;
      case    64: mode = 0b0111; break;
      case   128: mode = 0b1000; break;
      case   256: mode = 0b1001; break;
      case   512: mode = 0b1010; break;
      case  1024: mode = 0b1011; break;
      case  2048: mode = 0b1100; break;
      case  4096: mode = 0b1101; break;
      case  8192: mode = 0b1110; break;
      case 16384: mode = 0b1111; break;
      default: return;
    }
  }
  
  if(pin==3 || pin==11) {
    TCCR0B = TCCR1B & 0b11111000 | mode;
  } else if (pin==9 || pin==10) {
    TCCR1B = TCCR1B & 0b11111000 | mode;
  } else if (pin==5) {
    TCCR3B = TCCR3B & 0b11111000 | mode;
  } else if (pin==6 || pin==13) {
    TCCR4B = TCCR4B & 0b11110000 | mode;
  }
}
```


