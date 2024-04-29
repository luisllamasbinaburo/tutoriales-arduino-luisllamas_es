> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/conectar-emisora-radio-control-con-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Ejemplos de código
```cpp
int rcPins[6] = {5,6,7,8,9,10};
float chValue[6];

const int pulseInDelay = 20000;

void setup() 
{ 
  Serial.begin(9600);
}

float fmap(float x, float in_min, float in_max, float out_min, float out_max)
{
  return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}

void readChannel(int channel)
{
  int rawValue = pulseIn(rcPins[channel], HIGH, pulseInDelay);
  chValue[channel] = fmap( (float)rawValue, 1000.0, 2000.0, 0.0, 1.0);
  chValue[channel] = chValue[channel] < 0.0 ? 0.0 : chValue[channel]; 
  chValue[channel] = chValue[channel] > 1.0 ? 1.0 : chValue[channel];
}

void printChannel()
{
  for(int iChannel = 0; iChannel < 6; iChannel++)
  {
    Serial.print("Ch #");
    Serial.print(iChannel);
    Serial.print(": ");
    Serial.println(chValue[iChannel]);
  };
  Serial.println("------------");
}

void loop()
{
  for(int iChannel = 0; iChannel < 6; iChannel++)
  {
    readChannel(iChannel);
  }
  printChannel();
  delay(2500);
}
```


