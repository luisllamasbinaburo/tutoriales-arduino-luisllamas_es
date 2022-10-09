/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/reproducir-sonidos-arduino-buzzer-pasivo-altavoz/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```csharp
tone(pin, frecuencia); //activa un tono de frecuencia determinada en un pin dado
noTone(pin);           //detiene el tono en el pin
```

```csharp
tone(pin, frequencia, duracion);  //activa un tono de frecuencia y duracion determinados en un pin dado
```

```csharp
const int pinBuzzer = 9;

void setup() 
{
}

void loop() 
{
  //generar tono de 440Hz durante 1000 ms
  tone(pinBuzzer, 440);
  delay(1000);

  //detener tono durante 500ms  
  noTone(pinBuzzer);
  delay(500);

  //generar tono de 523Hz durante 500ms, y detenerlo durante 500ms.
  tone(pinBuzzer, 523, 300);
  delay(500);
}
```

```csharp
const int pinBuzzer = 9;

const int tonos[] = {261, 277, 294, 311, 330, 349, 370, 392, 415, 440, 466, 494};
const int countTonos = 10;
   
void setup()
{ 
}

void loop()
{
  for (int iTono = 0; iTono < countTonos; iTono++)
  {
   tone(pinBuzzer, tonos[iTono]);
   delay(1000);
  }
  noTone(pinBuzzer);
}
```