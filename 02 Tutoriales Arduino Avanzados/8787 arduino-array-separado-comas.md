/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-array-separado-comas/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
#define DEBUG_ARRAY(a) {for (int index = 0; index < sizeof(a) / sizeof(a[0]); index++) 	{Serial.print(a[index]); Serial.print('\t');} Serial.println();};

String str = "";
const char separator = ',';
const int dataLength = 3;
int data[dataLength];

void setup()
{
	Serial.begin(9600);
}

void loop()
{
	if (Serial.available())
	{
		str = Serial.readStringUntil('\n');
		for (int i = 0; i < dataLength ; i++)
		{
			int index = str.indexOf(separator);
			data[i] = str.substring(0, index).toInt();
			str = str.substring(index + 1);
		}
		DEBUG_ARRAY(data);
	}
}
```

```cpp
#define DEBUG_ARRAY(a) {for (int index = 0; index < sizeof(a) / sizeof(a[0]); index++) 	{Serial.print(a[index]); Serial.print('\t');} Serial.println();};

String str = "";
const char separator = ',';
const int dataLength = 3;
int dataArray[dataLength];
int data = 0;
int dataIndex = 0;

void setup()
{
	Serial.begin(9600);
}

void loop()
{
	while (Serial.available())
	{
		char incomingChar = Serial.read();
		if (incomingChar >= '0' && incomingChar <= '9')
		{
			data = (data * 10) + (incomingChar - '0');
		}
		else if (incomingChar == separator)
		{
			dataArray[dataIndex] = data;
			if (++dataIndex >= dataLength) dataIndex = 0;
			data = 0;
		}
		else if (incomingChar == '\n')
		{
			dataArray[dataIndex] = data;
			data = 0;
			dataIndex = 0;
			DEBUG_ARRAY(dataArray);
		}
	}
}
```