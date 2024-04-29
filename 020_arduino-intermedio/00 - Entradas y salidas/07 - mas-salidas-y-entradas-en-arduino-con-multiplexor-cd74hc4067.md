> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/mas-salidas-y-entradas-en-arduino-con-multiplexor-cd74hc4067/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## CD74HC4067 como salida
```cpp
const int muxSIG = A0;
const int muxS0 = 8;
const int muxS1 = 9;
const int muxS2 = 10;
const int muxS3 = 11;

int SetMuxChannel(byte channel)
{
  digitalWrite(muxS0, bitRead(channel, 0));
  digitalWrite(muxS1, bitRead(channel, 1));
  digitalWrite(muxS2, bitRead(channel, 2));
  digitalWrite(muxS3, bitRead(channel, 3));
}

void setup()
{
  pinMode(muxSIG, OUTPUT);
  pinMode(muxS0, OUTPUT);
  pinMode(muxS1, OUTPUT);
  pinMode(muxS2, OUTPUT);
  pinMode(muxS3, OUTPUT);
}

void loop()
{
  for (byte i = 0; i < 16; i++)
  {
    SetMuxChannel(i);
    digitalWrite(muxSIG, HIGH);
    delay(200);
    digitalWrite(muxSIG, LOW);
    delay(200);
  }
}
```


## CD74HC4067 como entrada
```cpp
const int muxSIG = A0;
const int muxS0 = 8;
const int muxS1 = 9;
const int muxS2 = 10;
const int muxS3 = 11;

int SetMuxChannel(byte channel)
{
  digitalWrite(muxS0, bitRead(channel, 0));
  digitalWrite(muxS1, bitRead(channel, 1));
  digitalWrite(muxS2, bitRead(channel, 2));
  digitalWrite(muxS3, bitRead(channel, 3));
}

void setup()
{
  Serial.begin(9600);
  pinMode(muxS0, OUTPUT);
  pinMode(muxS1, OUTPUT);
  pinMode(muxS2, OUTPUT);
  pinMode(muxS3, OUTPUT);
}

void loop()
{
  for (byte i = 0; i < 16; i++)
  {
    SetMuxChannel(i);
    byte muxValue = analogRead(muxSIG);

    Serial.print(muxValue);
    Serial.print("\t");
  }
  Serial.println();
  delay(1000);
}
```


