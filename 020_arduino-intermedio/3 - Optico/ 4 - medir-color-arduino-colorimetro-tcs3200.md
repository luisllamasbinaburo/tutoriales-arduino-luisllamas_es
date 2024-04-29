> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-color-arduino-colorimetro-tcs3200/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```cpp
//VCC——5V  
//GND——GND
//S0——D3  
//S1——D4
//S2——D5  
//S3——D6
//OUT——D2

const int s0 = 8;
const int s1 = 9;
const int s2 = 12;
const int s3 = 11;
const int out = 10;

byte countRed = 0;
byte countGreen = 0;
byte countBlue = 0;

void setup() {
  Serial.begin(9600);
  pinMode(s0, OUTPUT);
  pinMode(s1, OUTPUT);
  pinMode(s2, OUTPUT);
  pinMode(s3, OUTPUT);
  pinMode(out, INPUT);
  digitalWrite(s0, HIGH);
  digitalWrite(s1, HIGH);
}

void loop() {
  getColor();
  Serial.print("Red: ");
  Serial.print(countRed, DEC);
  Serial.print("Green: ");
  Serial.print(countGreen, DEC);
  Serial.print("Blue: ");
  Serial.print(countBlue, DEC);

  if (countRed < countBlue && countGreen > 100 && countRed < 80)
  {
    Serial.println(" - Red");
  }
  else if (countBlue < countRed && countBlue < countGreen)
  {
    Serial.println(" - Blue");
  }
  else if (countGreen < countRed && countGreen < countBlue)
  {
    Serial.println(" - Green");
  }
  else {
    Serial.println("-");
  }
  delay(300);
}

void getColor()
{
  digitalWrite(s2, LOW);
  digitalWrite(s3, LOW);
  countRed = pulseIn(out, digitalRead(out) == HIGH ? LOW : HIGH);
  digitalWrite(s3, HIGH);
  countBlue = pulseIn(out, digitalRead(out) == HIGH ? LOW : HIGH);
  digitalWrite(s2, HIGH);
  countGreen = pulseIn(out, digitalRead(out) == HIGH ? LOW : HIGH);
}
```


