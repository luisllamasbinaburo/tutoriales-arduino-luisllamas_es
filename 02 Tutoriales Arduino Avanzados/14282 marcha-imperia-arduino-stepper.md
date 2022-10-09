/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/marcha-imperia-arduino-stepper/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```csharp
typedef void(*PlayNoteCallback)(int note, int duration);

class ImperialMarch
{
public:
	PlayNoteCallback playNoteCallback;

	void playSong()
	{
		firstSection();
		secondSection();
		firstVariant();
		secondSection();
		secondVariant();
	}

	void playNote(int note, int duration)
	{
		playNoteCallback(note, duration);
		delay(20);
	}

private:
	const int c = 261;
	const int d = 294;
	const int e = 329;
	const int f = 349;
	const int g = 391;
	const int gS = 415;
	const int a = 440;
	const int aS = 455;
	const int b = 466;
	const int cH = 523;
	const int cSH = 554;
	const int dH = 587;
	const int dSH = 622;
	const int eH = 659;
	const int fH = 698;
	const int fSH = 740;
	const int gH = 784;
	const int gSH = 830;
	const int aH = 880;

	void firstSection()
	{
		playNote(a, 500); 
		playNote(a, 500); 
		playNote(a, 500); 
		playNote(f, 350);
		playNote(cH, 150);
		playNote(a, 500);
		playNote(f, 350);
		playNote(cH, 150);
		playNote(a, 650);

		delay(500);

		playNote(eH, 500);
		playNote(eH, 500);
		playNote(eH, 500);
		playNote(fH, 350);
		playNote(cH, 150);
		playNote(gS, 500);
		playNote(f, 350);
		playNote(cH, 150);
		playNote(a, 650);

		delay(500);
	}

	void secondSection()
	{
		playNote(aH, 500);
		playNote(a, 300);
		playNote(a, 150);
		playNote(aH, 500);
		playNote(gSH, 325);
		playNote(gH, 175);
		playNote(fSH, 125);
		playNote(fH, 125);
		playNote(fSH, 250);

		delay(325);

		playNote(aS, 250);
		playNote(dSH, 500);
		playNote(dH, 325);
		playNote(cSH, 175);
		playNote(cH, 125);
		playNote(b, 125);
		playNote(cH, 250);

		delay(350);
	}

	void firstVariant()
	{
		playNote(f, 250);
		playNote(gS, 500);
		playNote(f, 350);
		playNote(a, 125);
		playNote(cH, 500);
		playNote(a, 375);
		playNote(cH, 125);
		playNote(eH, 650);

		delay(500);
	}

	void secondVariant()
	{
		playNote(f, 250);
		playNote(gS, 500);
		playNote(f, 375);
		playNote(cH, 125);
		playNote(a, 500);
		playNote(f, 375);
		playNote(cH, 125);
		playNote(a, 650);

		delay(650);
	}
};
```

```csharp
#include <./imperialMarch.hpp>

 ImperialMarch imperialMarch;

void setup() 
{
  imperialMarch.playNoteCallback = beep;
}

void loop() 
{
   imperialMarch.playSong();

   delay(500);
}
```

```csharp
#include <M5StickC.h>

#include <./imperialMarch.hpp>

 ImperialMarch imperialMarch;

void playNote(int note, int duration)
{
   ledcWriteTone(ledChannel, note);
   delay(duration);
   ledcWriteTone(ledChannel, 0);
}

void setup() 
{
  M5.begin();

  ledcSetup(ledChannel, freq, resolution);
  ledcAttachPin(servo_pin, ledChannel);
  ledcWrite(ledChannel, 256);

  imperialMarch.playNoteCallback = playNote;
}

void loop() 
{
   imperialMarch.playSong();

   delay(500);
}
```

```csharp
#include <./imperialMarch.hpp>

const int STEPPER1_STEP = 0;
const int STEPPER1_DIR = 0;

ImperialMarch imperialMarch;

void playNote(int note, int duration)
{
   auto start_time = millis();
   auto step_time = 1000000 / note;

   while (millis() - start_time < duration)
   {
      digitalWrite(STEPPER1_STEP, HIGH);
      delayMicroseconds(100);
      digitalWrite(STEPPER1_STEP, LOW);
      delayMicroseconds(step_time - 100);
   }
}

void setup()
{
   pinMode(STEPPER1_DIR, OUTPUT);
   pinMode(STEPPER1_STEP, OUTPUT);

   digitalWrite(STEPPER1_DIR, HIGH);
   digitalWrite(STEPPER1_STEP, LOW);
   imperialMarch.playNoteCallback = playNote;
}

void loop()
{
   imperialMarch.playSong();

   delay(500);
}
```