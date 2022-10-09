> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-spi/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
SPI.begin();            // Inicia el bus SPI
SPI.transfer(c);        // Envía un byte
SPI.attachInterrupt();	// Activar la interrupción para recibir datos
```

```cpp
setBitOrder (LSBFIRST);   // least significant bit first
setBitOrder (MSBFIRST);   // more significant bit first
```

```cpp
setDataMode (SPI_MODE0);  // clock normalmente LOW, muestreo en flanco subida
setDataMode (SPI_MODE1);  // clock normalmente LOW, muestreo en flanco bajada
setDataMode (SPI_MODE2);  // clock normalmente HIGH, muestreo en flanco subida
setDataMode (SPI_MODE3);  // clock normalmente HIGH, muestreo en flanco bajada
```

```cpp
setClockDivider(SPI_CLOCK_DIV2);   //8 MHz (considerando un modelo de 16 Mhz)
setClockDivider(SPI_CLOCK_DIV4);   //4 MHz
setClockDivider(SPI_CLOCK_DIV8);   //2 MHz
setClockDivider(SPI_CLOCK_DIV16);  //1 MHz
setClockDivider(SPI_CLOCK_DIV32);  //500 KHz
setClockDivider(SPI_CLOCK_DIV64);  //250 KHz
setClockDivider(SPI_CLOCK_DIV128); //125 KHz
```

```cpp
SPI.beginTransaction (SPISettings (2000000, MSBFIRST, SPI_MODE0));  // 2 MHz clock, MSB first, mode 0
```