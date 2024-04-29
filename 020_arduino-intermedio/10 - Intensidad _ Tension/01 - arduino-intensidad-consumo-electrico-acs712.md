> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-intensidad-consumo-electrico-acs712/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Ejemplos de código
```cpp
// Sensibilidad del sensor en V/A
float SENSIBILITY = 0.185;   // Modelo 5A
//float SENSIBILITY = 0.100; // Modelo 20A
//float SENSIBILITY = 0.066; // Modelo 30A

int SAMPLESNUMBER = 100;

void setup() 
{
  Serial.begin(9600);
}

void printMeasure(String prefix, float value, String postfix)
{
  Serial.print(prefix);
  Serial.print(value, 3);
  Serial.println(postfix);
}

void loop()
{
  float current = getCorriente(SAMPLESNUMBER);
  float currentRMS = 0.707 * current;
  float power = 230.0 * currentRMS;

  printMeasure("Intensidad: ", current, "A ,");
  printMeasure("Irms: ", currentRMS, "A ,");
  printMeasure("Potencia: ", power, "W");
  delay(1000);
}

float getCorriente(int samplesNumber)
{
  float voltage;
  float corrienteSum = 0;
  for (int i = 0; i < samplesNumber; i++)
  {
    voltage = analogRead(A0) * 5.0 / 1023.0;
    corrienteSum += (voltage - 2.5) / SENSIBILITY;
  }
  return(corrienteSum / samplesNumber);
}
```


