> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/usar-el-sensor-de-temperatura-interno-de-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## usar-el-sensor-de-temperatura-interno-de-arduino
```cpp
void setup()
{
  Serial.begin(9600);

  Serial.println(F("Internal Temperature Sensor"));
}

void loop()
{
  Serial.println(GetTemp(),1);
  delay(1000);
}

double GetTemp(void)
{
  unsigned int wADC;
  double t;

  ADMUX = (_BV(REFS1) | _BV(REFS0) | _BV(MUX3));
  ADCSRA |= _BV(ADEN);
  delay(20);
  ADCSRA |= _BV(ADSC);
  while (bit_is_set(ADCSRA,ADSC));
  wADC = ADCW;

  // Esto es la función a calibrar.
  t = (wADC - 324.31 ) / 1.22;
  return (t);
}
```


