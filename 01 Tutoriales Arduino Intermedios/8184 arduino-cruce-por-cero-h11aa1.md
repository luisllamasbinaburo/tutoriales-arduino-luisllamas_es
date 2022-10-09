> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-cruce-por-cero-h11aa1/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int inputPin = 2;
 
int value = 0;
 
void setup() {
  Serial.begin(9600);
  pinMode(inputPin, INPUT_PULLUP);
}
 
void loop(){
  value = digitalRead(inputPin);  //lectura digital de pin
 
  //mandar mensaje a puerto serie en función del valor leido
  if (value == HIGH) {
      Serial.println("Encendido");
  }
  else {
      Serial.println("Apagado");
  }
  delay(1000);
}
```

```cpp
// period of pulse accumulation and serial output, milliseconds
const int inputPin = 2;
const int MainPeriod = 100;
long previousMillis = 0; // will store last time of the cycle end

volatile unsigned long previousMicros=0;
volatile unsigned long duration=0; // accumulates pulse width
volatile unsigned int pulsecount=0;

// interrupt handler
void freqCounterCallback() 
{
  unsigned long currentMicros = micros();
  duration += currentMicros - previousMicros;
  previousMicros = currentMicros;
  pulsecount++;
}

void reportFrequency()
{
    float freq = 1e6 / float(duration) * (float)pulsecount;
    Serial.print("Frec:");
    Serial.print(freq);
    Serial.println(" Hz"); 

     // clear counters
    duration = 0;
    pulsecount = 0;
}

void setup()
{
  Serial.begin(19200); 
  pinMode(inputPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(inputPin), freqCounterCallback, RISING);
}

void loop()
{
  unsigned long currentMillis = millis();
  if (currentMillis - previousMillis >= MainPeriod) 
  {
    previousMillis = currentMillis;    
    reportFrequency();
  }
}
```