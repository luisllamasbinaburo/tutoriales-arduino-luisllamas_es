/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-actuador-lineal/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
const int pinRelayA = 8;
const int pinRelayB = 9;

void setup()
{
	// initialize relay pins
	pinMode(pinRelayA, OUTPUT);
	pinMode(pinRelayB, OUTPUT);

	// preset relays to LOW
	digitalWrite(pinRelayA, LOW);
	digitalWrite(pinRelayB, LOW);
}

void loop()
{
	extendActuator();
	delay(5000);
	retractActuator();
	delay(5000);
	stopActuator();
	delay(5000);
}

//Set one relay one and the other off
//this will move extend the actuator
void extendActuator()
{
	digitalWrite(pinRelayB, LOW);
	delay(250);
	digitalWrite(pinRelayA, HIGH);
}

//Set one relay off and the other on 
//this will move retract the actuator 
void retractActuator()
{
	digitalWrite(pinRelayA, LOW);
	delay(250);
	digitalWrite(pinRelayB, HIGH);
}

//Set both relays off
//this will stop the actuator in a braking
void stopActuator()
{
	digitalWrite(pinRelayA, LOW);
	digitalWrite(pinRelayB, LOW);
}
```

```cpp
const int pinRelayA = 8;
const int pinRelayB = 9;
const int sensorPin = A0;

int GoalPosition = 350;
int CurrentPosition = 0;
bool IsExtending = false;
bool IsRetracting = false;

void setup()
{
	// initialize relay pins
	pinMode(pinRelayA, OUTPUT);
	pinMode(pinRelayB, OUTPUT);

	// preset relays to LOW
	digitalWrite(pinRelayA, LOW);
	digitalWrite(pinRelayB, LOW);
}

void loop()
{
	int newGoalPosition = GetGoalPosition();
	if (newGoalPosition != GetGoalPosition())
	{
		GoalPosition = newGoalPosition;
		ActivateActuator();
	}

	if (IsExtending || IsRetracting)
	{
		CurrentPosition = analogRead(sensorPin);

		if ((IsExtending = true && CurrentPosition > GoalPosition) || (IsRetracting = true && CurrentPosition < GoalPosition))
		{
			IsExtending = false;
			IsExtending = false;
			stopActuator();
		}
	}
}


int GetGoalPosition()
{
	return 300;
}


int ActivateActuator()
{
	if (CurrentPosition < GoalPosition)
	{
		IsExtending = true;
		IsRetracting = false;
		extendActuator();
	}
	if (CurrentPosition > GoalPosition)
	{
		IsExtending = false;
		IsRetracting = true;
		retractActuator();
	}
}

void extendActuator()
{
	digitalWrite(pinRelayB, LOW);
	delay(250);
	digitalWrite(pinRelayA, HIGH);
}

void retractActuator()
{
	digitalWrite(pinRelayA, LOW);
	delay(250);
	digitalWrite(pinRelayB, HIGH);
}

void stopActuator()
{
	digitalWrite(pinRelayA, LOW);
	digitalWrite(pinRelayB, LOW);
}

```