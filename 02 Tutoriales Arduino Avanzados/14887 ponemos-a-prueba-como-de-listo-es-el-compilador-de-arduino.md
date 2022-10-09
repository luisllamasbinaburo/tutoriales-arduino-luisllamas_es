/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/ponemos-a-prueba-como-de-listo-es-el-compilador-de-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

void printResults(unsigned long long counter, unsigned long long rst, unsigned long long microseconds)
{
	Serial.print("benchmark ");
	Serial.print(counter);
	Serial.print(": ");
	Serial.print(rst);
	Serial.print(" ");
	Serial.print(microseconds);
	Serial.println("us");
}

void benchmark()
{
	auto start = micros();

	unsigned long counter = 1000000;
	unsigned long sum = 0;
	for (size_t i = 0; i < counter; i++)
	{
		sum ++;
	}
	
	auto finish = micros();
	auto microseconds = finish - start;
	printResults();
}

void setup()
{
	Serial.begin(115200);
}

void loop()
{
	delay(5000);
	benchmark();
}


benchmark 1000000: 1000000 1us


unsigned long counter = 1000000;
unsigned long sum = 0;
for (size_t i = 0; i < counter; i++)
{
	sum += 3;
}


benchmark 1000000: 3000000 1us
benchmark 1000000000: 3000000 1us


const int value = 5;
unsigned long counter = 1000000;
unsigned long sum = 0;
for (size_t i = 0; i < counter; i++)
{
	sum += value;
}


benchmark 1000000: 50000000 1us


int value = 10;
unsigned long counter = 1000000;
unsigned long sum = 0;
for (size_t i = 0; i < counter; i++)
{
	sum += value;
}


benchmark 1000000: 50000000 1us


int givemeNumber()
{
	return 3;
}

void benchmark()
{
	auto start = micros();
	
	unsigned long counter = 1000000;
	unsigned long sum = 0;
	for (size_t i = 0; i < counter; i++)
	{
		sum += givemeNumber();
	}
	
	auto finish = micros();
	auto microseconds = finish - start;
	printResults(counter, 0, microseconds);
}


benchmark 1000000: 3000000 1us


void benchmark()
{
	unsigned long counter = 1000000;
	unsigned long sum = 0;
	for (size_t i = 0; i < counter; i++)
	{		
		sum += i;
	}
}


6	1783293664	8393


unsigned long counter = 1000000;
unsigned long sum = 0;
for (size_t i = 0; i < counter; i++)
{
	sum += random(10);
}


void benchmark()
{
	auto start = micros();
	
	unsigned long counter = 1000000;
	unsigned long sum = 0;
	for (size_t i = 0; i < counter; i++)
	{
		sum += i;
	}
	
	auto finish = micros();
	auto microseconds = finish - start;
	printResults(counter, 0, microseconds);  // no estamos usando sum
}


benchmark 1000000: 0 0us


void benchmark()
{
	auto start = micros();
	
	unsigned long counter = 1000000;
	unsigned long sum = 0;
	for (size_t i = 0; i < counter; i++)
	{
		sum += random(10);
	}
	
	auto finish = micros();
	auto microseconds = finish - start;
	printResults(counter, 0, microseconds);
}


benchmark 1000000: 0 681200us


void benchmark()
{
	auto counter = 1000000;
	auto sum = 0;
	for (size_t i = counter - 1; i != 0; i--)
	{
		sum += i;
	}
}


benchmark 1000000: 1783293664 8393us