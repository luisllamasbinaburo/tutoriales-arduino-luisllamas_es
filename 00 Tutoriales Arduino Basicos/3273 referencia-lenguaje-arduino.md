/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/referencia-lenguaje-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

//x igual a y
x == y

//x distinto de y
x != y

//x menor que y  
x < y 

//x mayor que y
x > y

//x menor o igual que y
x <= y

//x mayor o igual que y
x >= y 


//operador de asignación
a = b

//adición
a + b

//substracción
a - b

//multiplicación
a * b

//división
a / b

//modulo
a % b


//and binario
a & b  

//or binario
a | b  

//xor binario
a ^ b  

//not binario
a ~ b  

//desplazamiento a izquierda
a << b 

//desplazamiento a derecha
a >> b 


//incremento
a++

//decremento
a--

//adición compuesta
a += b

//substracción compuesta
a -= b

//multiplicación compuesta
a *= b

//división compuesta
a /= b

//and compuesto
a &= b

//or compuesto
a |= b


//not
!a

//and
a && b

//or
a || b


//operacion indirección
*variable

//operacion dirección
&variable


//tipo vacio (solo para funciones)
void


//booleano, false o true
boolean = false;


//entero, 16 bits, de -32,768 a 32,767
int var = 100;

//entero, 16 bits, de 0 a 65535 (excepto en Due, donde son 32 bits)
unsigned int var = 100;

//entero, 16 bits, de 0 a 65535
short var = 100;

//entero, 32 bits, de -2,147,483,648 a 2,147,483,647
long var = 100000L;

//entero, 32bits, de 0 a 4,294,967,295
unsigned long var = 100000L;


//coma floante, 32 bits, de -3.4028235E+38 a 3.4028235E+38. Precision 6 digitos
float var = 1.117;

//idéntico a float, excepto en Arduino Due donde es flotante de 64 bits
double var = 1.117;


//8 bits, de 0 a 255
byte var = B10010;

//16bits, sin signo, de 0 a 65535
word var = 10000;


//8 bits, de -128 a 127
char var = 'A';

//8 bits, de 0 a 255
unsigned char var = 240;


//convierte a char
char(variable);

//convierte a byte
byte(variable);

//convierte a int
int(variable);

//convierte a word
word(variable);

//convierte a long
long(variable);

//convierte a float
float(variable);


//STATIC
//Variables visibles únicamente en el interior de una función,
//y cuyo valor se mantiene entre llamadas a la misma.
static int  variable;

//CONST
//Variables cuyo valor no puede ser redefinido tras la inicialización
const float pi = 3.14;

//VOLATILE
//Variables en las que se indica al compilador que no guarde en los registros 
//del microprocesador, sino que fuerza la actualización en memoria. Esto se 
//hace cuando existe la posibilidad de que el valor de la variable sea 
//modificado por otro proceso que se ejecuta concurrentemente con el actual 
//(por ejemplo cuando se emplean hilos o interrupciones)
volatile int variable = LOW;


//declarar vector
int miArray[5];

//iniciar vector
int miArray[] = {2, 4, 8, 3, 6};

//declarar e iniciar vector
int miArray[5] = {2, 4, -8, 3, 2};


//asignar valor a elemento del vector
miArray[0] = 10;

//obtener valor de elemento del vector
x = miArray[4];


char cadena1[15];
char cadena2[8]  = {'a', 'r', 'd', 'u', 'i', 'n', 'o'};
char cadena3[8]  = {'a', 'r', 'd', 'u', 'i', 'n', 'o', '\0'};
char cadena4[ ]  = "texto";
char cadena5[8]  = "texto";
char cadena6[15] = "texto";

//vector de cadenas
char* cadenaArray[]={"Cadena 1", "Cadena 2", "Cadena 3",
"Cadena 4", "Cadena 5", "Cadena 6"};


// literal de cadena de texto
String txtMsg = "Hola";

// convirtiendo un char a String
String txtMsg =  String('a');

// convirtiendo un literal a String
String txtMsg =  String("Texto");

// concatenando dos literales a String
String txtMsg =  String("texto1" + "texto2");


condition ? true : false;


if (variable < 10)
{
	// accion A
}


if (variable < 10)
{
	// accion A
}
else
{
	// accion B
}


if (variable < 10)
{
	// accion A
}
else if (variable >= 100)
{
	// accion B
}
else
{
	// accion C
}


switch (variable) {
	case 1:
	  // accion A
	  break;
	case 2:
	  //  accion B
	  break;
	default: 
	  // caso por defecto (opcional)
}


for (int i=0; i <= 100; i++){
	// accion
} 


variable = 0;

while(variable < 100){
	// accion
	variable++;
}


do
{
	//accion
	variable++;
} while (variable < 100);


//devuelve mínimo entra a y b
min(a,b);

//devuelve máximo entra a y b
max(a,b);

//devuelve valor absoluto de a
abs(a);

//devuelve x restringido a (a,b)
constrain(x, a, b);

//interpola linealmente y entre x1,y1 x2,y2
map(x, x1, x2, y1, y2);


//devuelve a^b (ambos tipo float)
pow(a,b);

//devuelve la raiz cuadrada de a
sqrt(a);


//inicializa la semilla del generador de numeros pseudo aleatorios
randomSeed(semilla);

//devuelve un numero aleatorio entre a y b (ambos tipo long)
random(a, b);


//devuelve el seno de a (a tipo float y en radianes)
sin(a);

//devuelve el coseno de a (a tipo float y en radianes)
cos(a);

//devuelve la tangente de a (a tipo float y en radianes)
tan(a);


//devuelve el byte menos signiticativo de una palabra o variable.
lowByte(variable);

//devuelve el byte más significativo de una palabra
//(o el segundo byte menos significativo en variables mayores)
highByte(variable);

//devuelve el bit n de una variable x 
//(siendo el bit 0 el menos significativo)
bitRead(x, n);

//escribe el bit n de la variable x con el valor b
//(siendo el bit 0 el menos significativo)
bitWrite(x, n,b );

//pone a 1 el bit n de la variable x
bitSet(x, n);

//pone a 0 el bit n de la variable x
bitClear(x, n);

//obtiene el valor del bit n (idéntico a 2^n)
bit(n);


//delvuelve el caracter en la posición 3 (idéntico a txtMsg[3];)
txtMsg.charAt(3);

//sustituye el caracter en la posición 3 por "A" (idéntico a txtMsg[3]="A";)
txtMsg.setCharAt("A", 3);

//concatena texto 1 y texto 2 (idéntico a texto1=texto1+texto2;)
texto1.concat("texto2");

//devuelve la longitud de la cadena
txtMsg.length();

//devuelve la cadena convertida en minúsculas
txtMsg.toLowerCase();

//devuelve la cadena convertida en mayúsculas
txtMsg.toUpperCase();

//elimina espacios y carácteres incorrectos
txtMsg.trim();

//devuelve la cadena de texto como entero
txtMsg.toInt();


//compara dos cadenas. Devuelve 1 si texto1 es mayor que texto2,
//0 si son iguales, y -1 en caso contrario 
texto1.compareTo(texto2);

//compara si dos cadenas son iguales (idéntico a texto1==texto2)
texto1.equals(texto2);

//compara si dos cadenas son iguales, ignorando mayúsculas y minúsculas
texto1.equalsIgnoreCase(texto2);


//devuelve una subcadena de la posicion 3 a la 10
txtMsg.substring(3, 10);

//comprueba si la cadena empieza por "texto", con offset 3
txtMsg.startsWith("texto", 3);

//comprueba si la cadena empieza por "texto", con offset 3
txtMsg.endsWith("texto");


//devuelve el índice de la primera ocurrencia de 'A',
//a partir de la posición offset
txtMsg.indexOf('A', offset);

//devuelve el índice de la última ocurrencia de 'A'
//previa a la posición offset
txtMsg.lastIndexOf('A', offset);

//sustituye las ocurrencias de "texto1" por "texto2"
txtMsg.replace("texto1", "texto2");


int option=1;

int cambiar(){
  option=4;
}

void setup(){
  Serial.begin(9600);
}

void loop(){
  cambiar();
  Serial.print(option);  //muestra 4
  delay(10000);
}


int cambiar(var){
  var=4;
}

void setup(){
  Serial.begin(9600);
}

void loop(){
  int option=1;
  cambiar(option);
  Serial.print(option);  //muestra 1
  delay(10000);
}


int cambiar(int &var){
  var=4;
}

void setup(){
  Serial.begin(9600);
}

void loop(){
  int option=1;
  cambiar(option);
  Serial.print(option);    //muestra 4
  delay(10000);
}


int cambiar(int* var){
  *var=4;
}

void setup(){
  Serial.begin(9600);
}

void loop(){
  int option=1;
  cambiar(&option);
  Serial.print(option);    //muestra 4
  delay(10000);
}


int cambiar(){
  int var=4;
  return var;
}

void setup(){
  Serial.begin(9600);
}

void loop(){
  int option=1;
  option=cambiar();
  Serial.print(option);    //muestra 4
  delay(10000);
}


//declaracion
enum miEnumeracion {
  opcion1,
  opcion2,
  opcion3
};

//ejemplo de uso
miEnumeracion variable = opcion2;

if (variable==opcion2){
  //accion
}


//declaracion
struct miEstructura
{
   int  campo1;
   int  campo2;
   char campo3;
};

//ejemplo de uso
struct miEstructura variable;

variable.campo1=10;


//declaraciones
typedef int nuevotipo;
typedef enum miEnumeracion nuevotipo;
typedef struct miEstructura nuevotipo;

//ejemplo de uso
nuevotipo variable;


class MiRobot;

//definicion de clase ejemplo
class MiRobot
{
public:
  void saludar();	 //muestra "Hola"
  void incCont();	 //incrementa contador
  int  getCont();	 //devuelve contador
  void sayCont();	 //muestra valor contador
  void setCont(int); //inicializa contador a un valor
private:
  int cont=0;		 //variable contador privada
};

//muestra "Hola"
void MiRobot::saludar(){
  Serial.println("Hola");
}

void MiRobot::incCont(){
  this->cont++;
}

//devuelve contador
int MiRobot::getCont(){
  return this->cont;
}

//muestra valor contador
void MiRobot::sayCont(){
  Serial.println(this->cont);
}

//inicializa contador a un valor
void MiRobot::setCont(int _cont){
  this->cont=_cont;
}

MiRobot robot;
void setup(){
  Serial.println("Iniciando");
  Serial.begin(9600); 

  robot.saludar(); 	//se muestra hola
}

void loop(){
  robot.incCont();	//se incrementa el contador
  robot.sayCont();	//muestra el valor
  delay(1000);
}