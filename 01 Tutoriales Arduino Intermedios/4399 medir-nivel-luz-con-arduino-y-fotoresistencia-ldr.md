> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-nivel-luz-con-arduino-y-fotoresistencia-ldr/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int LEDPin = 13;
const int LDRPin = 2;

void setup()
{
pinMode(LEDPin, OUTPUT); 
pinMode(LDRPin, INPUT);
} 

void loop()
{
	int value = digitalRead(LDRPin);
	if (value == HIGH)
	{
		digitalWrite(LEDPin, HIGH);
		delay(50);
		digitalWrite(LEDPin, LOW);
		delay(50);
	}
}
```

```cpp

const int LEDPin = 13;
const int LDRPin = A0;
const int threshold = 100;

void setup() {
	pinMode(LEDPin, OUTPUT);
	pinMode(LDRPin, INPUT);
}

void loop() {
	int input = analogRead(LDRPin);
	if (input > threshold) {
		digitalWrite(LEDPin, HIGH);
	}
	else {
		digitalWrite(LEDPin, LOW);
	}
}
```

```cpp
const long A = 1000;     //Resistencia en oscuridad en KΩ
const int B = 15;        //Resistencia a la luz (10 Lux) en KΩ
const int Rc = 10;       //Resistencia calibracion en KΩ
const int LDRPin = A0;   //Pin del LDR

int V;
int ilum;

void setup() 
{
   Serial.begin(115200);
}

void loop()
{
   V = analogRead(LDRPin);         

   //ilum = ((long)(1024-V)*A*10)/((long)B*Rc*V);  //usar si LDR entre GND y A0 
   ilum = ((long)V*A*10)/((long)B*Rc*(1024-V));    //usar si LDR entre A0 y Vcc (como en el esquema anterior)
  
   Serial.println(ilum);	
   delay(1000);
}
```