> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/programar-arduino-con-eclipse/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Instalar Eclipse
```bash
sudo apt-get install eclipse
```



## Instalar avrdude
```bash
sudo apt-get install gcc-avr binutils-avr gdb-avr avr-libc avrdude  make
```



## Probando a programar Arduino con Eclipse
```cpp
#include <avr/io.h>
#include <util/delay.h>
 
int main(void) {
 
  DDRB = (1<<PB5); //Pin B5 Salida
 
  for(;;){ // while(1)
    PORTB |= (1<<PB5); //Ponemos el pin en alto
    _delay_ms(1000);
    PORTB &= ~(1<<PB5); //Ponemos el pin en bajo
    _delay_ms(1000);
  }
 
  return 0;
}
```


