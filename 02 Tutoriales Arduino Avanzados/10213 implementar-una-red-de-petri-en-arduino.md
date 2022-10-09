> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/implementar-una-red-de-petri-en-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
enum Input
{
	ForwardA = 0,
	ForwardB = 1,
	Unknown = 2
};

Input input;

const int numStates = 8;

uint8_t _markup[numStates];
uint8_t _prevMarkup[numStates];

// Definicion de las transiciones
void Transition0()
{
	if (_prevMarkup[0] && _prevMarkup[4] && (input == Input::ForwardA || input == Input::ForwardB))
	{
		RemoveMark(0); RemoveMark(4);
		AddMark(1); AddMark(5);
		Serial.println("Fired T0"); 
		printMarkup();
	}
}

void Transition1()
{
	if (_prevMarkup[1] && input == Input::ForwardA)
	{
		RemoveMark(1);
		AddMark(2);
		Serial.println("Fired T1");
		printMarkup();
	}
}

void Transition2()
{
	if (_prevMarkup[2] && _prevMarkup[6] && (input == Input::ForwardA || input == Input::ForwardB))
	{
		RemoveMark(2); RemoveMark(6);
		AddMark(3); AddMark(7);
		Serial.println("Fired T2");
		printMarkup();
	}
}

void Transition3()
{
	if (_prevMarkup[3] && input == Input::ForwardA)
	{
		RemoveMark(3);
		AddMark(0);
		Serial.println("Fired T3");
		printMarkup();
	}
}

void Transition4()
{
	if (_prevMarkup[5] && input == Input::ForwardB)
	{
		RemoveMark(5);
		AddMark(6);
		activateTimer();
		Serial.println("Fired T4");
		printMarkup();
	}
}

void Transition5()
{
	if (_prevMarkup[7] && input == Input::ForwardB)
	{
		RemoveMark(7);
		AddMark(4);
		Serial.println("Fired T5");
		printMarkup();
	}
}

void Transition6()
{
	if (_prevMarkup[6] && timerExpired())
	{
		RemoveMark(6);
		AddMark(5);
		Serial.println("Fired T6");
		printMarkup();
	}
}


void setup()
{
	Serial.begin(9600);

	// Estado inicial
	for (uint8_t index = 0; index < numStates; index++)
		_markup[index] = 0;

	_markup[0] = 1;
	_markup[4] = 1;

	printMarkup();  // Mostrar el estado inicial
}


void loop()
{
	input = static_cast<Input>(readInput());

	Update();
}

// Actualizar la red
void Update()
{
	// Copiar markup a prevaMarkup
	memcpy(_prevMarkup, _markup, numStates * sizeof(uint8_t));

	Transition0();
	Transition1();
	Transition2();
	Transition3();
	Transition4();
	Transition5();
	Transition6();
}

void AddMark(uint8_t index)
{
	_markup[index]++;
}

void RemoveMark(uint8_t index)
{
	_prevMarkup[index]--;
	_markup[index]--;
}

// Realiza la lectura de un caracter para el ejemplo
int readInput()
{
	Input currentInput = Input::Unknown;
	if (Serial.available())
	{
		char incomingChar = Serial.read();

		switch (incomingChar)
		{
		case 'A': currentInput = Input::ForwardA; break;
		case 'B': currentInput = Input::ForwardB; break;
		default: break;
		}
	}

	return currentInput;
}

// Para debug del ejemplo
void printMarkup()
{
	Serial.print(_markup[0]);
	Serial.print(_markup[1]);
	Serial.print(_markup[2]);
	Serial.println(_markup[3]);

	Serial.print(_markup[4]);
	Serial.print(_markup[5]);
	Serial.print(_markup[6]);
	Serial.println(_markup[7]);
}

// Timer para la transicion 6
unsigned long previousMillis;
bool isTimerON = false;

void activateTimer()
{
	isTimerON = true;
	previousMillis = millis();
}

bool timerExpired()
{
	if (isTimerON == false) return false;

	if ((unsigned long)(millis() - previousMillis) >= 5000)
		return true;
	return false;
}
```

```cpp
#include "PetriNetLib.h"

enum Input
{
	ForwardA = 0,
	ForwardB = 1,
	Unknown = 2
};

Input input;

PetriNet petriNet(8, 7);

void setup()
{
	Serial.begin(9600);
	
	// Definicion de la red de petri del ejemplo
	// Entradas y salidas de los estados
	static uint8_t inputs0[] = { 0, 4 };
	static uint8_t outputs0[] = { 1, 5 };

	static uint8_t inputs1[] = { 1 };
	static uint8_t outputs1[] = { 2 };

	static uint8_t inputs2[] = { 2, 6 };
	static uint8_t outputs2[] = { 3, 7 };

	static uint8_t inputs3[] = { 3 };
	static uint8_t outputs3[] = { 0 };

	static uint8_t inputs4[] = { 5 };
	static uint8_t outputs4[] = { 6 };

	static uint8_t inputs5[] = { 7 };
	static uint8_t outputs5[] = { 4 };
	
	// Transiciones
	petriNet.SetTransition(0, inputs0, 2, outputs0, 2,
		[]() -> bool {return input == Input::ForwardA || input == Input::ForwardB; },
		[]() {Serial.println("Fired T0"); printMarkup(); });

	petriNet.SetTransition(1, inputs1, 1, outputs1, 1,
		[]() -> bool {return input == Input::ForwardA; },
		[]() {Serial.println("Fired T1"); printMarkup(); });

	petriNet.SetTransition(2, inputs2, 2, outputs2, 2,
		[]() -> bool {return input == Input::ForwardA || input == Input::ForwardB; },
		[]() {Serial.println("Fired T2"); printMarkup(); });

	petriNet.SetTransition(3, inputs3, 1, outputs3, 1,
		[]() -> bool {return input == Input::ForwardA; },
		[]() {Serial.println("Fired T3"); printMarkup(); });

	petriNet.SetTransition(4, inputs4, 1, outputs4, 1,
		[]() -> bool {return input == Input::ForwardB; },
		[]() {Serial.println("Fired T4"); printMarkup(); activateTimer(); });

	petriNet.SetTransition(5, inputs5, 1, outputs5, 1,
		[]() -> bool {return input == Input::ForwardB; },
		[]() {Serial.println("Fired T5");  printMarkup(); });

	petriNet.SetTransition(6, outputs4, 1, inputs4, 1,
		timerExpired,
		[]() {Serial.println("Reseting T6"); printMarkup(); });

	// Marcado inicial
	petriNet.SetMarkup(0, 1);
	petriNet.SetMarkup(4, 1);
	
	printMarkup();  // Mostrar el estado inicial
}

void loop()
{
	input = static_cast<Input>(readInput());

	petriNet.Update();
}

// Realiza la lectura de un caracter para el ejemplo
int readInput()
{
	Input currentInput = Input::Unknown;
	if (Serial.available())
	{
		char incomingChar = Serial.read();

		switch (incomingChar)
		{
		case 'A': currentInput = Input::ForwardA; break;
		case 'B': currentInput = Input::ForwardB; break;
		default: break;
		}
	}

	return currentInput;
}

// Para debug del ejemplo
void printMarkup()
{
	Serial.print(petriNet.GetMarkup(0));
	Serial.print(petriNet.GetMarkup(1));
	Serial.print(petriNet.GetMarkup(2));
	Serial.println(petriNet.GetMarkup(3));

	Serial.print(petriNet.GetMarkup(4));
	Serial.print(petriNet.GetMarkup(5));
	Serial.print(petriNet.GetMarkup(6));
	Serial.println(petriNet.GetMarkup(7));
}

// Timer para la transicion 6
unsigned long previousMillis;
bool isTimerON = false;

void activateTimer()
{
	isTimerON = true;
	previousMillis = millis();
}

bool timerExpired()
{
	if (isTimerON == false) return false;

	if ((unsigned long)(millis() - previousMillis) >= 5000)
		return true;
	return false;
}
```