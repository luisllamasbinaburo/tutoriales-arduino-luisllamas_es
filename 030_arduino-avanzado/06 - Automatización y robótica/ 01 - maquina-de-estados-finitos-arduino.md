> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/maquina-de-estados-finitos-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Máquina de estados en Arduino
```cpp
enum State
{
  PosicionA,
  PosicionB,
  PosicionC,
  PosicionD
};

enum Input
{
  Unknown,
  Reset,
  Forward,
  Backward
};

// Variables globales
State currentState;
Input currentInput;

// Acciones de los estados y condiciones de transiciones
void stateA()
{
  if (currentInput == Input::Forward)
    changeState(State::PosicionB);
}

void stateB()
{
  if (currentInput == Input::Backward)
    changeState(State::PosicionA);
  if (currentInput == Input::Forward)
    changeState(State::PosicionC);
  if (currentInput == Input::Reset)
    changeState(State::PosicionA);
}

void stateC()
{
  if (currentInput == Input::Backward)
    changeState(State::PosicionB);
  if (currentInput == Input::Forward)
    changeState(State::PosicionD);
  if (currentInput == Input::Reset)
    changeState(State::PosicionA);
}

void stateD()
{
  if (currentInput == Input::Backward)
    changeState(State::PosicionC);
  if (currentInput == Input::Reset)
    changeState(State::PosicionA);
}

// Salidas asociadas a las transiciones
void outputA()
{
  Serial.println("A   B   C   D");
  Serial.println("X            ");
  Serial.println();
}

void outputB()
{
  Serial.println("A   B   C   D");
  Serial.println("    X        ");
  Serial.println();
}

void outputC()
{
  Serial.println("A   B   C   D");
  Serial.println("        X    ");
  Serial.println();
}

void outputD()
{
  Serial.println("A   B   C   D");
  Serial.println("            X");
  Serial.println();
}

void setup()
{
  Serial.begin(9600);
  currentState = PosicionA;
  outputA();
}

void loop() 
{
  readInput();
  updateStateMachine();
}

// Actualiza el estado de la maquina
void updateStateMachine()
{
  switch (currentState)
  {
    case PosicionA: stateA(); break;
    case PosicionB: stateB(); break;
    case PosicionC: stateC(); break;
    case PosicionD: stateD(); break;
  }
}

// Lee la entrada por puerto serie
void readInput()
{
  currentInput = Input::Unknown;
  if (Serial.available())
  {
    char incomingChar = Serial.read();

    switch (incomingChar)
    {
    case 'R': currentInput = Input::Reset;  break;
    case 'A': currentInput = Input::Backward; break;
    case 'D': currentInput = Input::Forward; break;
    default: break;
    }
  }
}

// Funcion que cambia el estado y dispara las transiciones
void changeState(int newState)
{
  currentState = newState;

  switch (currentState)
  {
    case State::PosicionA: outputA();   break;
    case State::PosicionB: outputB();   break;
    case State::PosicionC: outputC();   break;
    case State::PosicionD: outputD();   break;
    default: break;
  }
}
```



## Máquina de estados tabular en Arduino
```cpp
enum State
{
  PosicionA,
  PosicionB,
  PosicionC,
  PosicionD
};

enum Input
{
  Unknown,
  Reset,
  Forward,
  Backward
};

const void outputA()
{
  Serial.println("A   B   C   D");
  Serial.println("X            ");
  Serial.println();
}

const void outputB()
{
  Serial.println("A   B   C   D");
  Serial.println("    X        ");
  Serial.println();
}

const void outputC()
{
  Serial.println("A   B   C   D");
  Serial.println("        X    ");
  Serial.println();
}

const void outputD()
{
  Serial.println("A   B   C   D");
  Serial.println("            X");
  Serial.println();
}

typedef void (*const StateFunc)();
State currentState;
Input currentInput;

const StateFunc transitionOutput[] PROGMEM = { outputA, outputB, outputC, outputD};

const byte stateChange[4][4] PROGMEM =
{
  { PosicionA, PosicionA, PosicionB, PosicionA },
  { PosicionB, PosicionA, PosicionC, PosicionA },
  { PosicionC, PosicionA, PosicionD, PosicionB },
  { PosicionD, PosicionA, PosicionD, PosicionC },
};

void setup()
{
  Serial.begin(9600);
  currentState = PosicionA;
  outputA();
}

void loop() 
{
  readInput();
  updateStateMachine();
}
    
void updateStateMachine()
{
  State newState = (State)pgm_read_byte(&stateChange[currentState][currentInput]);
  if (currentState == newState) return;

  currentState = newState;

  StateFunc pStateFunc = (StateFunc)pgm_read_ptr(&transitionOutput[currentState]); 
  pStateFunc();
}

void readInput()
{
  currentInput = Input::Unknown;
  if (Serial.available())
  {
    char incomingChar = Serial.read();

    switch (incomingChar)
    {
      case 'R': currentInput = Input::Reset;  break;
      case 'A': currentInput = Input::Backward; break;
      case 'D': currentInput = Input::Forward; break;
      default: break;
    }
  }
}
```



## Máquina de estados en una librería
```cpp
#include "StateMachineLib.h"

enum State
{
  PosicionA = 0,
  PosicionB = 1,
  PosicionC = 2,
  PosicionD = 3
};

enum Input
{
  Reset = 0,
  Forward = 1,
  Backward = 2,
  Unknown = 3,
};

StateMachine stateMachine(4, 9);
Input input;

void setupStateMachine()
{
  stateMachine.AddTransition(PosicionA, PosicionB, []() { return input == Forward; });

  stateMachine.AddTransition(PosicionB, PosicionA, []() { return input == Backward; });
  stateMachine.AddTransition(PosicionB, PosicionC, []() { return input == Forward; });
  stateMachine.AddTransition(PosicionB, PosicionA, []() { return input == Reset; });

  stateMachine.AddTransition(PosicionC, PosicionB, []() { return input == Backward; });
  stateMachine.AddTransition(PosicionC, PosicionD, []() { return input == Forward; });
  stateMachine.AddTransition(PosicionC, PosicionA, []() { return input == Reset; });

  stateMachine.AddTransition(PosicionD, PosicionC, []() { return input == Backward; });
  stateMachine.AddTransition(PosicionD, PosicionA, []() { return input == Reset; });

  stateMachine.SetOnEntering(PosicionA, outputA);
  stateMachine.SetOnEntering(PosicionB, outputB);
  stateMachine.SetOnEntering(PosicionC, outputC);
  stateMachine.SetOnEntering(PosicionD, outputD);

  stateMachine.SetOnLeaving(PosicionA, []() {Serial.println("Leaving A"); });
  stateMachine.SetOnLeaving(PosicionB, []() {Serial.println("Leaving B"); });
  stateMachine.SetOnLeaving(PosicionC, []() {Serial.println("Leaving C"); });
  stateMachine.SetOnLeaving(PosicionD, []() {Serial.println("Leaving D"); });
}

void setup() 
{
  Serial.begin(9600);

  Serial.println("Starting State Machine...");
  setupStateMachine();  
  Serial.println("Start Machine Started");

  stateMachine.SetState(PosicionA, false, true);
}

void loop() 
{
  input = static_cast<Input>(readInput());

  stateMachine.Update();
}

int readInput()
{
  Input currentInput = Input::Unknown;
  if (Serial.available())
  {
    char incomingChar = Serial.read();

    switch (incomingChar)
    {
      case 'R': currentInput = Input::Reset;   break;
      case 'A': currentInput = Input::Backward; break;
      case 'D': currentInput = Input::Forward; break;
      default: break;
    }
  }

  return currentInput;
}

void outputA()
{
  Serial.println("A   B   C   D");
  Serial.println("X            ");
  Serial.println();
}

void outputB()
{
  Serial.println("A   B   C   D");
  Serial.println("    X        ");
  Serial.println();
}

void outputC()
{
  Serial.println("A   B   C   D");
  Serial.println("        X    ");
  Serial.println();
}

void outputD()
{
  Serial.println("A   B   C   D");
  Serial.println("            X");
  Serial.println();
}
```


