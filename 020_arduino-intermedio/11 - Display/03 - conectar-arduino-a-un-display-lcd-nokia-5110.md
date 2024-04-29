> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/conectar-arduino-a-un-display-lcd-nokia-5110/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Sin librería
```cpp
const int PIN_RESET = 3;  // LCD1 Reset
const int PIN_SCE = 4;    // LCD2 Chip Select
const int PIN_DC = 5;     // LCD3 Dat/Command
const int PIN_SDIN = 6;   // LCD4 Data in
const int PIN_SCLK = 7;   // LCD5 Clk
        // LCD6 Vcc
        // LCD7 Vled
        // LCD8 Gnd

// Invertir para conseguir blanco sobre negro o negro sobre blanco
const int LCD_C = LOW;
const int LCD_D = HIGH;

const int LCD_X = 84;
const int LCD_Y = 48;
const int LCD_CMD = 0;

// Tabla para visualizar caracteres Ascii
static const byte ASCII[][5] =
{
  { 0x00, 0x00, 0x00, 0x00, 0x00 } // 20  
  ,{ 0x00, 0x00, 0x5f, 0x00, 0x00 } // 21 !
  ,{ 0x00, 0x07, 0x00, 0x07, 0x00 } // 22 "
  ,{ 0x14, 0x7f, 0x14, 0x7f, 0x14 } // 23 #
  ,{ 0x24, 0x2a, 0x7f, 0x2a, 0x12 } // 24 $
  ,{ 0x23, 0x13, 0x08, 0x64, 0x62 } // 25 %
  ,{ 0x36, 0x49, 0x55, 0x22, 0x50 } // 26 &
  ,{ 0x00, 0x05, 0x03, 0x00, 0x00 } // 27 '
  ,{ 0x00, 0x1c, 0x22, 0x41, 0x00 } // 28 (
  ,{ 0x00, 0x41, 0x22, 0x1c, 0x00 } // 29 )
  ,{ 0x14, 0x08, 0x3e, 0x08, 0x14 } // 2a *
  ,{ 0x08, 0x08, 0x3e, 0x08, 0x08 } // 2b +
  ,{ 0x00, 0x50, 0x30, 0x00, 0x00 } // 2c ,
  ,{ 0x08, 0x08, 0x08, 0x08, 0x08 } // 2d -
  ,{ 0x00, 0x60, 0x60, 0x00, 0x00 } // 2e .
  ,{ 0x20, 0x10, 0x08, 0x04, 0x02 } // 2f /
  ,{ 0x3e, 0x51, 0x49, 0x45, 0x3e } // 30 0
  ,{ 0x00, 0x42, 0x7f, 0x40, 0x00 } // 31 1
  ,{ 0x42, 0x61, 0x51, 0x49, 0x46 } // 32 2
  ,{ 0x21, 0x41, 0x45, 0x4b, 0x31 } // 33 3
  ,{ 0x18, 0x14, 0x12, 0x7f, 0x10 } // 34 4
  ,{ 0x27, 0x45, 0x45, 0x45, 0x39 } // 35 5
  ,{ 0x3c, 0x4a, 0x49, 0x49, 0x30 } // 36 6
  ,{ 0x01, 0x71, 0x09, 0x05, 0x03 } // 37 7
  ,{ 0x36, 0x49, 0x49, 0x49, 0x36 } // 38 8
  ,{ 0x06, 0x49, 0x49, 0x29, 0x1e } // 39 9
  ,{ 0x00, 0x36, 0x36, 0x00, 0x00 } // 3a :
  ,{ 0x00, 0x56, 0x36, 0x00, 0x00 } // 3b ;
  ,{ 0x08, 0x14, 0x22, 0x41, 0x00 } // 3c <
  ,{ 0x14, 0x14, 0x14, 0x14, 0x14 } // 3d =
  ,{ 0x00, 0x41, 0x22, 0x14, 0x08 } // 3e >
  ,{ 0x02, 0x01, 0x51, 0x09, 0x06 } // 3f ?
  ,{ 0x32, 0x49, 0x79, 0x41, 0x3e } // 40 @
  ,{ 0x7e, 0x11, 0x11, 0x11, 0x7e } // 41 A
  ,{ 0x7f, 0x49, 0x49, 0x49, 0x36 } // 42 B
  ,{ 0x3e, 0x41, 0x41, 0x41, 0x22 } // 43 C
  ,{ 0x7f, 0x41, 0x41, 0x22, 0x1c } // 44 D
  ,{ 0x7f, 0x49, 0x49, 0x49, 0x41 } // 45 E
  ,{ 0x7f, 0x09, 0x09, 0x09, 0x01 } // 46 F
  ,{ 0x3e, 0x41, 0x49, 0x49, 0x7a } // 47 G
  ,{ 0x7f, 0x08, 0x08, 0x08, 0x7f } // 48 H
  ,{ 0x00, 0x41, 0x7f, 0x41, 0x00 } // 49 I
  ,{ 0x20, 0x40, 0x41, 0x3f, 0x01 } // 4a J
  ,{ 0x7f, 0x08, 0x14, 0x22, 0x41 } // 4b K
  ,{ 0x7f, 0x40, 0x40, 0x40, 0x40 } // 4c L
  ,{ 0x7f, 0x02, 0x0c, 0x02, 0x7f } // 4d M
  ,{ 0x7f, 0x04, 0x08, 0x10, 0x7f } // 4e N
  ,{ 0x3e, 0x41, 0x41, 0x41, 0x3e } // 4f O
  ,{ 0x7f, 0x09, 0x09, 0x09, 0x06 } // 50 P
  ,{ 0x3e, 0x41, 0x51, 0x21, 0x5e } // 51 Q
  ,{ 0x7f, 0x09, 0x19, 0x29, 0x46 } // 52 R
  ,{ 0x46, 0x49, 0x49, 0x49, 0x31 } // 53 S
  ,{ 0x01, 0x01, 0x7f, 0x01, 0x01 } // 54 T
  ,{ 0x3f, 0x40, 0x40, 0x40, 0x3f } // 55 U
  ,{ 0x1f, 0x20, 0x40, 0x20, 0x1f } // 56 V
  ,{ 0x3f, 0x40, 0x38, 0x40, 0x3f } // 57 W
  ,{ 0x63, 0x14, 0x08, 0x14, 0x63 } // 58 X
  ,{ 0x07, 0x08, 0x70, 0x08, 0x07 } // 59 Y
  ,{ 0x61, 0x51, 0x49, 0x45, 0x43 } // 5a Z
  ,{ 0x00, 0x7f, 0x41, 0x41, 0x00 } // 5b [
  ,{ 0x02, 0x04, 0x08, 0x10, 0x20 } // 5c ¥
  ,{ 0x00, 0x41, 0x41, 0x7f, 0x00 } // 5d ]
  ,{ 0x04, 0x02, 0x01, 0x02, 0x04 } // 5e ^
  ,{ 0x40, 0x40, 0x40, 0x40, 0x40 } // 5f _
  ,{ 0x00, 0x01, 0x02, 0x04, 0x00 } // 60 `
  ,{ 0x20, 0x54, 0x54, 0x54, 0x78 } // 61 a
  ,{ 0x7f, 0x48, 0x44, 0x44, 0x38 } // 62 b
  ,{ 0x38, 0x44, 0x44, 0x44, 0x20 } // 63 c
  ,{ 0x38, 0x44, 0x44, 0x48, 0x7f } // 64 d
  ,{ 0x38, 0x54, 0x54, 0x54, 0x18 } // 65 e
  ,{ 0x08, 0x7e, 0x09, 0x01, 0x02 } // 66 f
  ,{ 0x0c, 0x52, 0x52, 0x52, 0x3e } // 67 g
  ,{ 0x7f, 0x08, 0x04, 0x04, 0x78 } // 68 h
  ,{ 0x00, 0x44, 0x7d, 0x40, 0x00 } // 69 i
  ,{ 0x20, 0x40, 0x44, 0x3d, 0x00 } // 6a j 
  ,{ 0x7f, 0x10, 0x28, 0x44, 0x00 } // 6b k
  ,{ 0x00, 0x41, 0x7f, 0x40, 0x00 } // 6c l
  ,{ 0x7c, 0x04, 0x18, 0x04, 0x78 } // 6d m
  ,{ 0x7c, 0x08, 0x04, 0x04, 0x78 } // 6e n
  ,{ 0x38, 0x44, 0x44, 0x44, 0x38 } // 6f o
  ,{ 0x7c, 0x14, 0x14, 0x14, 0x08 } // 70 p
  ,{ 0x08, 0x14, 0x14, 0x18, 0x7c } // 71 q
  ,{ 0x7c, 0x08, 0x04, 0x04, 0x08 } // 72 r
  ,{ 0x48, 0x54, 0x54, 0x54, 0x20 } // 73 s
  ,{ 0x04, 0x3f, 0x44, 0x40, 0x20 } // 74 t
  ,{ 0x3c, 0x40, 0x40, 0x20, 0x7c } // 75 u
  ,{ 0x1c, 0x20, 0x40, 0x20, 0x1c } // 76 v
  ,{ 0x3c, 0x40, 0x30, 0x40, 0x3c } // 77 w
  ,{ 0x44, 0x28, 0x10, 0x28, 0x44 } // 78 x
  ,{ 0x0c, 0x50, 0x50, 0x50, 0x3c } // 79 y
  ,{ 0x44, 0x64, 0x54, 0x4c, 0x44 } // 7a z
  ,{ 0x00, 0x08, 0x36, 0x41, 0x00 } // 7b {
  ,{ 0x00, 0x00, 0x7f, 0x00, 0x00 } // 7c |
  ,{ 0x00, 0x41, 0x36, 0x08, 0x00 } // 7d }
  ,{ 0x10, 0x08, 0x08, 0x10, 0x08 } // 7e ?
  ,{ 0x00, 0x06, 0x09, 0x09, 0x06 } // 7f ?
};

// Inicializar el LCD
void LcdInitialise(void)
{
  pinMode(PIN_SCE, OUTPUT);
  pinMode(PIN_RESET, OUTPUT);
  pinMode(PIN_DC, OUTPUT);
  pinMode(PIN_SDIN, OUTPUT);
  pinMode(PIN_SCLK, OUTPUT);

  digitalWrite(PIN_RESET, LOW);
  digitalWrite(PIN_RESET, HIGH);

  LcdWrite(LCD_CMD, 0x21);  // LCD Extended Commands.
  LcdWrite(LCD_CMD, 0xBf);  // Set LCD Vop (Contrast). //B1
  LcdWrite(LCD_CMD, 0x04);  // Set Temp coefficent. //0x04
  LcdWrite(LCD_CMD, 0x14);  // LCD bias mode 1:48. //0x13
  LcdWrite(LCD_CMD, 0x0C);  // LCD in normal mode. 0x0d for inverse
  LcdWrite(LCD_C, 0x20);
  LcdWrite(LCD_C, 0x0C);
}

// Enviar byte a la pantalla
void LcdWrite(byte dc, byte data)
{
  digitalWrite(PIN_DC, dc);
  digitalWrite(PIN_SCE, LOW);
  shiftOut(PIN_SDIN, PIN_SCLK, MSBFIRST, data);
  digitalWrite(PIN_SCE, HIGH);
}

// Posicionar cursor en x,y
void gotoXY(int x, int y)
{
  LcdWrite(0, 0x80 | x);  // Column.
  LcdWrite(0, 0x40 | y);  // Row.  
}

// Borrar pantalla
void LcdClear(void)
{
  for (int index = 0; index < LCD_X * LCD_Y / 8; index++)
  {
    LcdWrite(LCD_D, 0x00);
  }
}

// Mostrar caracter por pantalla
void LcdCharacter(char character)
{
  LcdWrite(LCD_D, 0x00);
  for (int index = 0; index < 5; index++)
  {
    LcdWrite(LCD_D, ASCII[character - 0x20][index]);
  }
  LcdWrite(LCD_D, 0x00);
}

// Mostrar string por pantalla
void LcdString(char *characters)
{
  while (*characters)
  {
    LcdCharacter(*characters++);
  }
}

// Dibujar caja (dos lineas horizontales y dos verticales)
void DrawBox()
{
  unsigned char j;
  for (j = 0; j<84; j++) // top
  {
    gotoXY(j, 0);
    LcdWrite(1, 0x01);
  }
  for (j = 0; j<84; j++) //Bottom
  {
    gotoXY(j, 5);
    LcdWrite(1, 0x80);
  }

  for (j = 0; j<6; j++) // Right
  {
    gotoXY(83, j);
    LcdWrite(1, 0xff);
  }
  for (j = 0; j<6; j++) // Left
  {
    gotoXY(0, j);
    LcdWrite(1, 0xff);
  }
}

void setup()
{
  LcdInitialise();
  LcdClear();
}

void loop()
{
  int a, b;
  char Str[15];
  // Draw a Box
  for (b = 1000; b > 0; b--) {
    DrawBox();
    for (a = 0; a <= 5; a++) {
      gotoXY(4, 1);
      // Put text in Box
      LcdString("TestDisplay");
      gotoXY(24, 3);
      LcdCharacter('H');
      LcdCharacter('E');
      LcdCharacter('L');
      LcdCharacter('L');
      LcdCharacter('O');
      LcdCharacter(' ');
      LcdCharacter('=');

      // Draw + at this position
      gotoXY(10, 3);
      LcdCharacter('=');
      delay(500);

      gotoXY(24, 3);
      LcdCharacter('h');
      LcdCharacter('e');
      LcdCharacter('l');
      LcdCharacter('l');
      LcdCharacter('o');
      LcdCharacter(' ');
      LcdCharacter('-');

      // Draw - at this position
      gotoXY(10, 3);
      LcdCharacter('-');

      delay(500);
    }
  }
}
```


## Librería Adafruit Nokia 5110 LCD
```cpp
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_PCD8544.h>

const int PIN_RESET = 3;  // LCD1 Reset
const int PIN_SCE = 4;    // LCD2 Chip Select
const int PIN_DC = 5;     // LCD3 Dat/Command
const int PIN_SDIN = 6;   // LCD4 Data in
const int PIN_SCLK = 7;   // LCD5 Clk
              // LCD6 Vcc
              // LCD7 Vled
              // LCD8 Gnd

Adafruit_PCD8544 display = Adafruit_PCD8544(PIN_SCLK, PIN_SDIN, PIN_DC, PIN_SCE, PIN_RESET);


#define NUMFLAKES 10

#define XPOS 0

#define YPOS 1

#define DELTAY 2


#define LOGO16_GLCD_HEIGHT 16

#define LOGO16_GLCD_WIDTH  16

static const unsigned char PROGMEM logo16_glcd_bmp[] =
{ B00000000, B11000000,
B00000001, B11000000,
B00000001, B11000000,
B00000011, B11100000,
B11110011, B11100000,
B11111110, B11111000,
B01111110, B11111111,
B00110011, B10011111,
B00011111, B11111100,
B00001101, B01110000,
B00011011, B10100000,
B00111111, B11100000,
B00111111, B11110000,
B01111100, B11110000,
B01110000, B01110000,
B00000000, B00110000 };

void setup() {
  Serial.begin(9600);

  display.begin();
  // init done

  // you can change the contrast around to adapt the display
  // for the best viewing!
  display.setContrast(50);

  display.display(); // show splashscreen
  delay(2000);
  display.clearDisplay();   // clears the screen and buffer

                // draw a single pixel
  display.drawPixel(10, 10, BLACK);
  display.display();
  delay(2000);
  display.clearDisplay();

  // draw many lines
  testdrawline();
  display.display();
  delay(2000);
  display.clearDisplay();

  // draw rectangles
  testdrawrect();
  display.display();
  delay(2000);
  display.clearDisplay();

  // draw multiple rectangles
  testfillrect();
  display.display();
  delay(2000);
  display.clearDisplay();

  // draw mulitple circles
  testdrawcircle();
  display.display();
  delay(2000);
  display.clearDisplay();

  // draw a circle, 10 pixel radius
  display.fillCircle(display.width() / 2, display.height() / 2, 10, BLACK);
  display.display();
  delay(2000);
  display.clearDisplay();

  testdrawroundrect();
  delay(2000);
  display.clearDisplay();

  testfillroundrect();
  delay(2000);
  display.clearDisplay();

  testdrawtriangle();
  delay(2000);
  display.clearDisplay();

  testfilltriangle();
  delay(2000);
  display.clearDisplay();

  // draw the first ~12 characters in the font
  testdrawchar();
  display.display();
  delay(2000);
  display.clearDisplay();

  // text display tests
  display.setTextSize(1);
  display.setTextColor(BLACK);
  display.setCursor(0, 0);
  display.println("Hello, world!");
  display.setTextColor(WHITE, BLACK); // 'inverted' text
  display.println(3.141592);
  display.setTextSize(2);
  display.setTextColor(BLACK);
  display.print("0x"); display.println(0xDEADBEEF, HEX);
  display.display();
  delay(2000);

  // rotation example
  display.clearDisplay();
  display.setRotation(1);  // rotate 90 degrees counter clockwise, can also use values of 2 and 3 to go further.
  display.setTextSize(1);
  display.setTextColor(BLACK);
  display.setCursor(0, 0);
  display.println("Rotation");
  display.setTextSize(2);
  display.println("Example!");
  display.display();
  delay(2000);

  // revert back to no rotation
  display.setRotation(0);

  // miniature bitmap display
  display.clearDisplay();
  display.drawBitmap(30, 16, logo16_glcd_bmp, 16, 16, 1);
  display.display();

  // invert the display
  display.invertDisplay(true);
  delay(1000);
  display.invertDisplay(false);
  delay(1000);

  // draw a bitmap icon and 'animate' movement
  testdrawbitmap(logo16_glcd_bmp, LOGO16_GLCD_WIDTH, LOGO16_GLCD_HEIGHT);
}

void loop() {

}

void testdrawbitmap(const uint8_t *bitmap, uint8_t w, uint8_t h) {
  uint8_t icons[NUMFLAKES][3];
  randomSeed(666);     // whatever seed

             // initialize
  for (uint8_t f = 0; f< NUMFLAKES; f++) {
    icons[f][XPOS] = random(display.width());
    icons[f][YPOS] = 0;
    icons[f][DELTAY] = random(5) + 1;

    Serial.print("x: ");
    Serial.print(icons[f][XPOS], DEC);
    Serial.print(" y: ");
    Serial.print(icons[f][YPOS], DEC);
    Serial.print(" dy: ");
    Serial.println(icons[f][DELTAY], DEC);
  }

  while (1) {
    // draw each icon
    for (uint8_t f = 0; f< NUMFLAKES; f++) {
      display.drawBitmap(icons[f][XPOS], icons[f][YPOS], logo16_glcd_bmp, w, h, BLACK);
    }
    display.display();
    delay(200);

    // then erase it + move it
    for (uint8_t f = 0; f< NUMFLAKES; f++) {
      display.drawBitmap(icons[f][XPOS], icons[f][YPOS], logo16_glcd_bmp, w, h, WHITE);
      // move it
      icons[f][YPOS] += icons[f][DELTAY];
      // if its gone, reinit
      if (icons[f][YPOS] > display.height()) {
        icons[f][XPOS] = random(display.width());
        icons[f][YPOS] = 0;
        icons[f][DELTAY] = random(5) + 1;
      }
    }
  }
}

void testdrawchar(void) {
  display.setTextSize(1);
  display.setTextColor(BLACK);
  display.setCursor(0, 0);

  for (uint8_t i = 0; i < 168; i++) {
    if (i == '\n') continue;
    display.write(i);
    //if ((i > 0) && (i % 14 == 0))
    //display.println();
  }
  display.display();
}

void testdrawcircle(void) {
  for (int16_t i = 0; i<display.height(); i += 2) {
    display.drawCircle(display.width() / 2, display.height() / 2, i, BLACK);
    display.display();
  }
}

void testfillrect(void) {
  uint8_t color = 1;
  for (int16_t i = 0; i<display.height() / 2; i += 3) {
    // alternate colors
    display.fillRect(i, i, display.width() - i * 2, display.height() - i * 2, color % 2);
    display.display();
    color++;
  }
}

void testdrawtriangle(void) {
  for (int16_t i = 0; i<min(display.width(), display.height()) / 2; i += 5) {
    display.drawTriangle(display.width() / 2, display.height() / 2 - i,
      display.width() / 2 - i, display.height() / 2 + i,
      display.width() / 2 + i, display.height() / 2 + i, BLACK);
    display.display();
  }
}

void testfilltriangle(void) {
  uint8_t color = BLACK;
  for (int16_t i = min(display.width(), display.height()) / 2; i>0; i -= 5) {
    display.fillTriangle(display.width() / 2, display.height() / 2 - i,
      display.width() / 2 - i, display.height() / 2 + i,
      display.width() / 2 + i, display.height() / 2 + i, color);
    if (color == WHITE) color = BLACK;
    else color = WHITE;
    display.display();
  }
}

void testdrawroundrect(void) {
  for (int16_t i = 0; i<display.height() / 2 - 2; i += 2) {
    display.drawRoundRect(i, i, display.width() - 2 * i, display.height() - 2 * i, display.height() / 4, BLACK);
    display.display();
  }
}

void testfillroundrect(void) {
  uint8_t color = BLACK;
  for (int16_t i = 0; i<display.height() / 2 - 2; i += 2) {
    display.fillRoundRect(i, i, display.width() - 2 * i, display.height() - 2 * i, display.height() / 4, color);
    if (color == WHITE) color = BLACK;
    else color = WHITE;
    display.display();
  }
}

void testdrawrect(void) {
  for (int16_t i = 0; i<display.height() / 2; i += 2) {
    display.drawRect(i, i, display.width() - 2 * i, display.height() - 2 * i, BLACK);
    display.display();
  }
}

void testdrawline() {
  for (int16_t i = 0; i<display.width(); i += 4) {
    display.drawLine(0, 0, i, display.height() - 1, BLACK);
    display.display();
  }
  for (int16_t i = 0; i<display.height(); i += 4) {
    display.drawLine(0, 0, display.width() - 1, i, BLACK);
    display.display();
  }
  delay(250);

  display.clearDisplay();
  for (int16_t i = 0; i<display.width(); i += 4) {
    display.drawLine(0, display.height() - 1, i, 0, BLACK);
    display.display();
  }
  for (int8_t i = display.height() - 1; i >= 0; i -= 4) {
    display.drawLine(0, display.height() - 1, display.width() - 1, i, BLACK);
    display.display();
  }
  delay(250);

  display.clearDisplay();
  for (int16_t i = display.width() - 1; i >= 0; i -= 4) {
    display.drawLine(display.width() - 1, display.height() - 1, i, 0, BLACK);
    display.display();
  }
  for (int16_t i = display.height() - 1; i >= 0; i -= 4) {
    display.drawLine(display.width() - 1, display.height() - 1, 0, i, BLACK);
    display.display();
  }
  delay(250);

  display.clearDisplay();
  for (int16_t i = 0; i<display.height(); i += 4) {
    display.drawLine(display.width() - 1, 0, 0, i, BLACK);
    display.display();
  }
  for (int16_t i = 0; i<display.width(); i += 4) {
    display.drawLine(display.width() - 1, 0, i, display.height() - 1, BLACK);
    display.display();
  }
  delay(250);
}
```


