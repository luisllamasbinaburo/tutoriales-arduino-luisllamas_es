> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-encoder-rotativo/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Precisión simple o doble por pool
```cpp
const int channelPinA = 9;
const int channelPinB = 10;

unsigned char stateChannelA;
unsigned char stateChannelB;
unsigned char prevStateChannelA = 0;

const int maxSteps = 255;
int prevValue;
int value;

const int timeThreshold = 5; 
unsigned long currentTime;
unsigned long loopTime;

bool IsCW = true;

void setup() {
  Serial.begin(9600);
  pinMode(channelPinA, INPUT);
  pinMode(channelPinB, INPUT);
  currentTime = millis();
  loopTime = currentTime;
  value = 0;
  prevValue = 0;
}

void loop() {
  currentTime = millis();
  if (currentTime >= (loopTime + timeThreshold))
  {
    stateChannelA = digitalRead(channelPinA);
    stateChannelB = digitalRead(channelPinB);
    if (stateChannelA != prevStateChannelA)  // Para precision simple if((!stateChannelA) && (prevStateChannelA))
    {
      if (stateChannelB) // B es HIGH, es CW
      {
        bool IsCW = true;
        if (value + 1 <= maxSteps) value++; // Asegurar que no sobrepasamos maxSteps
      }
      else  // B es LOW, es CWW
      {
        bool IsCW = false;
        if (value - 1 >= 0) value = value--; // Asegurar que no tenemos negativos
      }

    }
    prevStateChannelA = stateChannelA;  // Guardar valores para siguiente

    // Si ha cambiado el valor, mostrarlo
    if (prevValue != value)
    {
      prevValue = value;
      Serial.print(value);

    }

    loopTime = currentTime;  // Actualizar tiempo
  }
  
  // Otras tareas
}
```


## Precisión doble con una interrupción
```cpp
const int channelPinA = 2;
const int channelPinB = 10;

const int timeThreshold = 5;
long timeCounter = 0;

const int maxSteps = 255;
volatile int ISRCounter = 0;
int counter = 0;

bool IsCW = true;

void setup()
{
  pinMode(channelPinA, INPUT_PULLUP);
  Serial.begin(9600);
  attachInterrupt(digitalPinToInterrupt(channelPinA), doEncode, CHANGE);
}

void loop()
{
  if (counter != ISRCounter)
  {
    counter = ISRCounter;
    Serial.println(counter);
  }
  delay(100);
}

void doEncode()
{
  if (millis() > timeCounter + timeThreshold)
  {
    if (digitalRead(channelPinA) == digitalRead(channelPinB))
    {
      IsCW = true;
      if (ISRCounter + 1 <= maxSteps) ISRCounter++;
    }
    else
    {
      IsCW = false;
      if (ISRCounter - 1 > 0) ISRCounter--;
    }
    timeCounter = millis();
  }
}
```


## Precisión cuádruple con dos interrupciones
```cpp
const int channelPinA = 2;
const int channelPinB = 3;

const int timeThreshold = 5;
long timeCounter = 0;

const int maxSteps = 255;
volatile int ISRCounter = 0;
int counter = 0;

bool IsCW = true;

void setup()
{
  pinMode(channelPinA, INPUT_PULLUP);
  pinMode(channelPinB, INPUT_PULLUP);
  Serial.begin(9600);
  attachInterrupt(digitalPinToInterrupt(channelPinA), doEncodeA, CHANGE);
  attachInterrupt(digitalPinToInterrupt(channelPinB), doEncodeB, CHANGE);
}

void loop()
{
  if (counter != ISRCounter)
  {
    counter = ISRCounter;
    Serial.println(counter);
  }
  delay(100);
}

void doEncodeA()
{
  if (millis() > timeCounter + timeThreshold)
  {
    if (digitalRead(channelPinA) == digitalRead(channelPinB))
    {
      IsCW = true;
      if (ISRCounter + 1 <= maxSteps) ISRCounter++;
    }
    else
    {
      IsCW = false;
      if (ISRCounter - 1 > 0) ISRCounter--;
    }
    timeCounter = millis();
  }
}

void doEncodeB()
{
  if (millis() > timeCounter + timeThreshold)
  {
    if (digitalRead(channelPinA) != digitalRead(channelPinB))
    {
      IsCW = true;
      if (ISRCounter + 1 <= maxSteps) ISRCounter++;
    }
    else
    {
      IsCW = false;
      if (ISRCounter - 1 > 0) ISRCounter--;
    }
    timeCounter = millis();
  }
}
```


