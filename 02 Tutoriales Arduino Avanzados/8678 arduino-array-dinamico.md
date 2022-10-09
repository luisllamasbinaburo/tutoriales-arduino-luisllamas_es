> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-array-dinamico/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
int* list;
size_t count;
size_t capacity;

// Crear una nueva lista
void CreateList(size_t _capacity)
{
  list = new int[_capacity];
  capacity = _capacity;
  count = 0;
}

// Añadir elemento al final de la lista
void AddItem(int item)
{
  ++count;
    
  if (count > capacity)
  {
    size_t newSize = capacity * 2;
    resize(newSize);
  } 

  list[count - 1] = item;
}

// Eliminar ultimo elemento de la lista
void RemoveTail()
{
  --count;
}

// Reducir capacidad de la lista al numero de elementos
void Trim()
{
  resize(count);
}

// Reescalar lista
void resize(size_t newCapacity)
{
  int* newList = new int[newCapacity];
  memmove(newList, list, count  * sizeof(int));
  delete[] list;
  capacity = newCapacity;
  list = newList;
}

// Muestra la lista por Serial para debug
void printList()
{
  Serial.print("Capacity:");
  Serial.print(capacity);

  Serial.print("  Count:");
  Serial.print(count);

  Serial.print("  Items:");
  for (size_t index = 0; index < count; index++)
  {
    Serial.print(list[index]);
    Serial.print(' ');
  }
  Serial.println();
}


void setup()
{
  Serial.begin(9600);

  Serial.println("Lista con 2 elementos");
  CreateList(2);

  Serial.println();
  Serial.println("Introducir 1 y 2 elementos");
  AddItem(1);
  AddItem(2);
  printList();

  Serial.println();
  Serial.println("Resize e introducir 3 y 4 elementos");
  AddItem(3);
  AddItem(4);
  printList();

  Serial.println();
  Serial.println("Resize e introducir 5 y 6 elementos");
  AddItem(5);
  AddItem(6);
  printList();

  Serial.println();
  Serial.println("Eliminar dos ultimos");
  RemoveTail();
  RemoveTail();
  printList();

  Serial.println();
  Serial.println("Ajustar tamaño");
  Trim();
  printList();
}

void loop()
{
}
```