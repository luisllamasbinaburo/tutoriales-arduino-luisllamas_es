/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/convertir-texto-a-numero-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
#define DEBUG(a) Serial.println(a);

String text = "-12345";

void setup() 
{
	Serial.begin(9600);

	long value;
	value = text.toInt();
	DEBUG(value);
}

void loop()
{
}
```

```cpp
#define DEBUG(a) Serial.println(a);

String text = "-123.45";

void setup() 
{
	Serial.begin(9600);

	float value;
	value = text.toFloat();
	DEBUG(value);
}

void loop()
{
}
```

```cpp
#define DEBUG(a) Serial.println(a);

char* text = "-12345";

void setup() 
{
	Serial.begin(9600);

	long value;
	value = atol(text);
	DEBUG(value);
}

void loop()
{
}
```

```cpp
#define DEBUG(a) Serial.println(a);

char* text= "-123.45";

void setup() 
{
	Serial.begin(9600);

	long value;
	value = atof(text);
	DEBUG(value);
}

void loop()
{
}
```

```cpp
#define DEBUG(a) Serial.println(a);

char *text = "-12345";

void setup() {
	Serial.begin(9600);

	long value;
	value = naiveToInt(text);
	DEBUG(value);
}

void loop()
{
}

long naiveToInt(const char *charArray) {
	long data = 0;
	bool isNegative = false;
	if (*charArray == '-') 
	{
		isNegative = true;
		++charArray;
	}

	while (*charArray >= '0' && *charArray <= '9')
	{
		data = (data * 10) + (*charArray - '0');
		++charArray;
	}

	return isNegative ? -data : data;
}
```

```cpp
#define DEBUG(a) Serial.println(a);

char *text = "-123.45";

void setup() {
	Serial.begin(9600);

	float value;
	value = naiveToFloat(text);
	DEBUG(value);
}

void loop()
{
}

float naiveToFloat(const char *charArray) 
{
	long dataReal = 0;
	long dataDecimal = 0;
	long dataPow = 1;
	bool isNegative = false;

	if (*charArray == '-')
	{
		isNegative = true;
		++charArray;
	}

	while ((*charArray >= '0' && *charArray <= '9'))
	{
		dataReal = (dataReal * 10) + (*charArray - '0');
		++charArray;
	}

	if (*charArray == '.' || *charArray == ',')
	{
		++charArray;
		while ((*charArray >= '0' && *charArray <= '9'))
		{
			dataDecimal = (dataDecimal * 10) + (*charArray - '0');
			dataPow *= 10;
			++charArray;
		}
	}

	float data = (float)dataReal + (float)dataDecimal / dataPow;
	return isNegative ? -data : data;
}
```