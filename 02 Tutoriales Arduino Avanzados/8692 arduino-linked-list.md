/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-linked-list/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

struct node {
  int value;
  node *next;
};

node *head;
size_t listSize;

// Crea una linkedlist con un único elemento
void createLinkedList(int value)
{
  listSize = 1;
  head = new node();
  head->next = nullptr;
  head->value = value;
}

// Añade un elemento al principio de la linkedlist
void insertHead(int value)
{
  node* newHead = new node();
  newHead->next = head;
  newHead->value = value;
  head = newHead;
  listSize++;
}

// Añade un elemento al final de la linkedlist
void insertTail(int value)
{
  node* newTail = new node();
  newTail->next = nullptr;
  newTail->value = value;
  
  node *enumerator =  head;
  for (size_t index = 0; index < listSize - 1; index++)
     enumerator = enumerator->next;
   
  enumerator->next = newTail;
  listSize++;
 }

 void removeHead()
 {
   if(listSize <= 1) return;

    node* newHead = head->next;
    delete(head);
    head = newHead;
    listSize--; 
  
 }

 void removeTail()
 {
     if(listSize <= 1) return;

    node *enumerator =  head;
      for (size_t index = 0; index < listSize - 2; index++)
        enumerator = enumerator->next;

    delete enumerator->next;
    enumerator->next = nullptr;
    listSize--;
 }

// Muestra la linkedlist por Serial
void printLinkedList()
{
  Serial.print("Size:");
  Serial.print(listSize);
  
  Serial.print("  Items:");
  node *enumerator;
  enumerator = head;

  for (size_t index = 0; index < listSize; index++)
  {
    Serial.print(enumerator->value);
    Serial.print(' ');
    enumerator = enumerator->next;
  }
  Serial.println();
}


void setup()
{
  Serial.begin(9600);

  Serial.println("Lista con elemento 3");
  createLinkedList(3);
  printLinkedList();

  Serial.println();
  Serial.println("Añadir 4 y 5 al final");
  insertTail(4);
  insertTail(5);
  printLinkedList();

  Serial.println();
  Serial.println("Añadir 1 y 2 al principio");
  insertHead(2);
  insertHead(1);
  printLinkedList();

  Serial.println();
  Serial.println("Eliminar 1 y 5");
  removeHead();
  removeTail();
  printLinkedList();
}

void loop()
{
}