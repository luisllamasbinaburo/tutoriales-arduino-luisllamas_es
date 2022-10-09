> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/buffer-circular-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int circularBufferLength = 10;
int circularBuffer[circularBufferLength];
int circularBufferIndex = 0;

void appendToBuffer(int value)
{
	circularBuffer[circularBufferIndex] = value;
	circularBufferIndex++;
	if (circularBufferIndex >= circularBufferLength) circularBufferIndex = 0;
}


void getFromBuffer(int* out, int outLength)
{
	int readIndex = (circularBufferIndex - outLength + circularBufferLength) % circularBufferLength;
	for (int iCount = 0; iCount < outLength; iCount++)
	{
		if (readIndex >= circularBufferLength) readIndex = 0;
		out[iCount] = circularBuffer[readIndex];
		readIndex++;
	}
}


void getFromBufferInvert(int* out, int outLength)
{
	int readIndex = circularBufferIndex - 1;
	for (int iCount = 0; iCount < outLength; iCount++)
	{
		if (readIndex < 0) readIndex = circularBufferLength - 1;
		out[iCount] = circularBuffer[readIndex];
		readIndex--;
	}
}

void printArray(int* x, int length)
{
	for (int iCount = 0; iCount < length; iCount++)
	{
		Serial.print(x[iCount]);
		Serial.print(',');
	}
	Serial.println();
}

void setup()
{
	Serial.begin(115200);

	Serial.println("Store 0 - 12");
	for (int iCount = 0; iCount <= 12; iCount++)
	{
		appendToBuffer(iCount);
		printArray(circularBuffer, circularBufferLength);
	}

	Serial.println("");
	Serial.println("Get last 5");
	int data[5];
	int M = sizeof(data) / sizeof(int);
	getFromBuffer(data, M);
	printArray(data, M);

	Serial.println("");
	Serial.println("Get last 5 invert");
	getFromBufferInvert(data, M);
	printArray(data, M);
}

void loop()
{
}
```

```cpp
const int circularBufferLength = 10;
int circularBuffer[circularBufferLength];
int* circularBufferAccessor = circularBuffer;

void appendToBuffer(int value)
{
	circularBufferAccessor++;
	if (circularBufferAccessor >= circularBuffer + circularBufferLength) circularBufferAccessor = circularBuffer;
	*circularBufferAccessor = value;
}

int getFromBuffer()
{
	int rst = *circularBufferAccessor;
	circularBufferAccessor--;
	if (circularBufferAccessor < circularBuffer) circularBufferAccessor = circularBuffer + circularBufferLength - 1;
	return rst;
}

void getFromBuffer(int* out, int outLength)
{
	int* readAccessor = circularBufferAccessor - outLength + 1;
	if (circularBufferAccessor < circularBuffer) circularBufferAccessor = circularBuffer + circularBufferLength - 1;

	for (int iCount = 0; iCount < outLength; iCount++)
	{
		if (readAccessor >= circularBuffer + circularBufferLength) readAccessor = circularBuffer;
		out[iCount] = *readAccessor;
		readAccessor++;
	}
}


void getFromBufferInvert(int* out, int outLength)
{
	int* readAccessor = circularBufferAccessor;
	for (int iCount = 0; iCount < outLength; iCount++)
	{
		if (readAccessor < circularBuffer) readAccessor = circularBuffer + circularBufferLength - 1;
		out[iCount] = *readAccessor;
		readAccessor--;
	}
}

void printArray(int* x, int length)
{
	for (int iCount = 0; iCount < length; iCount++)
	{
		Serial.print(x[iCount]);
		Serial.print(',');
	}
	Serial.println();
}

void setup()
{
	Serial.begin(115200);

	Serial.println("Store 0 - 12");
	for (int iCount = 0; iCount <= 12; iCount++)
	{
		appendToBuffer(iCount);
		printArray(circularBuffer, circularBufferLength);
	}

	Serial.println("");
	Serial.println("Get last 5");
	int data[5];
	int M = sizeof(data) / sizeof(int);
	getFromBuffer(data, M);
	printArray(data, M);

	Serial.println("");
	Serial.println("Get last 5 invert");
	getFromBufferInvert(data, M);
	printArray(data, M);
}

void loop()
{
}
```