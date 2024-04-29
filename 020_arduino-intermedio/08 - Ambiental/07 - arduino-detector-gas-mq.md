> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-detector-gas-mq/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Lectura digital
```cpp
const int MQ_PIN = 2;
const int MQ_DELAY = 2000;

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  bool state= digitalRead(MQ_PIN);

  if (!state)
  {
    Serial.println("Deteccion");
  }
  else
  {
    Serial.println("No detectado");
  }
  delay(MQ_DELAY);
}
```


## Lectura analógica
```cpp
const int MQ_PIN = A0;
const int MQ_DELAY = 2000;

void setup()
{
  Serial.begin(9600);
}

void loop() 
{
  int raw_adc = analogRead(MQ_PIN);
  float value_adc = raw_adc * (5.0 / 1023.0);

  Serial.print("Raw:");
  Serial.print(raw_adc);
  Serial.print("    Tension:");
  Serial.println(value_adc);

  delay(MQ_DELAY);
}
```


## Leer la concentración
```cpp
const int MQ_PIN = A0;    // Pin del sensor
const int RL_VALUE = 5;    // Resistencia RL del modulo en Kilo ohms
const int R0 = 10;          // Resistencia R0 del sensor en Kilo ohms

// Datos para lectura multiple
const int READ_SAMPLE_INTERVAL = 100;    // Tiempo entre muestras
const int READ_SAMPLE_TIMES = 5;     // Numero muestras

// Ajustar estos valores para vuestro sensor según el Datasheet
// (opcionalmente, según la calibración que hayáis realizado)
const float X0 = 200;
const float Y0 = 1.7;
const float X1 = 10000;
const float Y1 = 0.28;

// Puntos de la curva de concentración {X, Y}
const float punto0[] = { log10(X0), log10(Y0) };
const float punto1[] = { log10(X1), log10(Y1) };

// Calcular pendiente y coordenada abscisas
const float scope = (punto1[1] - punto0[1]) / (punto1[0] - punto0[0]);
const float coord = punto0[1] - punto0[0] * scope;

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  float rs_med = readMQ(MQ_PIN);    // Obtener la Rs promedio
  float concentration = getConcentration(rs_med/R0);  // Obtener la concentración
  
  // Mostrar el valor de la concentración por serial
  Serial.println("Concentración: ");
  Serial.println(concentration);
}

// Obtener la resistencia promedio en N muestras
float readMQ(int mq_pin)
{
  float rs = 0;
  for (int i = 0;i<READ_SAMPLE_TIMES;i++) {
    rs += getMQResistance(analogRead(mq_pin));
    delay(READ_SAMPLE_INTERVAL);
  }
  return rs / READ_SAMPLE_TIMES;
}

// Obtener resistencia a partir de la lectura analogica
float getMQResistance(int raw_adc)
{
  return (((float)RL_VALUE / 1000.0*(1023 - raw_adc) / raw_adc));
}

// Obtener concentracion 10^(coord + scope * log (rs/r0)
float getConcentration(float rs_ro_ratio)
{
  return pow(10, coord + scope * log(rs_ro_ratio));
}
```


