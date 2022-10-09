/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-rs485-max485/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

void setup() 
{ 
  Serial.begin(9600);
} 
 
void loop() 
{ 
  byte data = millis() / 1000;
  Serial.write(data);
  delay(250);                           
} 


void setup() 
{ 
  Serial.begin(9600);  
} 
 
void loop() 
{  
  if (Serial.available()) 
  {
    byte data = Serial.read(); 
  }
} 


const int ledPin =  13;  // Led integrado
const int ReDePin =  2;  // HIGH = Driver / LOW = Receptor
const char HEADER = 'H';

void setup() 
{ 
	Serial.begin(9600);
	Serial.setTimeout(100);
  
	pinMode(ledPin, OUTPUT);
	pinMode(ReDePin, OUTPUT);
} 
 
void loop() 
{ 
	//RS485 transmisor 
	digitalWrite(ReDePin, HIGH);
	digitalWrite(ledPin, HIGH); 
	Serial.print(HEADER);
	Serial.flush();  
	delay(50); 
   
	//RS485 como receptor
	digitalWrite(ReDePin, LOW); 
	digitalWrite(ledPin, LOW); 
	if(Serial.find(HEADER))
	{
		int data = Serial.parseInt(); 
		 
		// Aqui hariamos lo que queramos con data
	}
} 


const int ledPin =  13;  // Led integrado
const int ReDePin =  2;  // HIGH = Driver / LOW = Receptor
const char HEADER = 'H';

void setup() 
{ 
	Serial.begin(9600);  

	pinMode(ReDePin, OUTPUT);
	pinMode(ledPin, OUTPUT);
	pinMode(ReDePin, OUTPUT);
} 
 
void loop() 
{ 
	//RS485 como receptor
	digitalWrite(ReDePin, LOW); 
	digitalWrite(ledPin, LOW); 
	
	if(Serial.available())
	{
		if(Serial.read()==HEADER)
		{
			//RS485 transmisor 
			digitalWrite(ReDePin, HIGH);
			digitalWrite(ledPin, HIGH); 
			
			int data = millis() / 1000;
			Serial.print(HEADER);
			Serial.print(data);
			Serial.flush();
		}
	}
	delay(10);
}