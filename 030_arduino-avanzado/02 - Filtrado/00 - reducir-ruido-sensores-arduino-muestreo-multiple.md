> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/reducir-ruido-sensores-arduino-muestreo-multiple/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Adquisición por muestras constante
```cpp
float MeasureN(int samplesNumber, float (*funct)())
{
  float sum;
  for (int i = 0; i < samplesNumber; i++)
  {
    sum += funct();
  }

  return sum / samplesNumber;
}

//Funcion que simula la medicion de un sensor
float getMeasure()
{
  return 0.0;
}

void setup()
{
  float measureN = MeasureN(100, getMeasure);
}

void loop()
{

}
```


## Adquisición en tiempo constante
```cpp
float MeasureT(int msInterval, float(*funct)())
{
  float sum;
  long startTime = millis();
  int samplesNumber = 0;

  while (millis() - startTime < msInterval)
  {
    samplesNumber++;
    sum += getMeasure();
  }
  return sum / samplesNumber;
}

//Funcion que simula la medicion de un sensor
float getMeasure()
{
  return 0.0;
}

void setup()
{
  float measureT = MeasureT(50, getMeasure);
}

void loop()
{

}
```


## Añadir puntos Max, Min y Mid
```cpp
float min, max, mid;
float MeasureN(int samplesNumber, float(*funct)())
{
  float value;

  for (int i = 0; i < samplesNumber; i++)
  {
    value = funct();
    if (value > max) max = value;
    if (value < min) min = value;
  }

  mid = (max - min) / 2;
  return sum / samplesNumber;
}

float MeasureT(int msInterval, float(*funct)())
{
  float value;
  long startTime = millis();

  while (millis() - startTime < msInterval)
  {
    value = funct();
    if (value > max) max = value;
    if (value < min) min = value;
  }
  
  mid = (max - min) / 2;
  return sum / samplesNumber;
}

float getMeasure()
{
  return 0.0;
}

void setup()
{
  MeasureN(100, getMeasure);
  MeasureT(50, getMeasure);
}

void loop()
{

}
```


