/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/conectar-arduino-a-una-pantalla-tft/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
/***************************************************
This is our GFX example for the Adafruit ILI9341 Breakout and Shield
----> http://www.adafruit.com/products/1651

Check out the links above for our tutorials and wiring diagrams
These displays use SPI to communicate, 4 or 5 pins are required to
interface (RST is optional)
Adafruit invests time and resources providing this open source code,
please support Adafruit and open-source hardware by purchasing
products from Adafruit!

Written by Limor Fried/Ladyada for Adafruit Industries.
MIT license, all text above must be included in any redistribution
****************************************************/


#include "SPI.h"
#include "Adafruit_GFX.h"
#include "Adafruit_ILI9341.h"

#define TFT_CS 10
#define TFT_DC 8

// Use hardware SPI (on Uno, #13, #12, #11) and the above for CS/DC
Adafruit_ILI9341 ILI9341 = Adafruit_ILI9341(TFT_CS, TFT_DC);

void setup() {
	Serial.begin(9600);
	Serial.println("ILI9341 Test!");

	ILI9341.begin();

	// read diagnostics (optional but can help debug problems)
	uint8_t x = ILI9341.readcommand8(ILI9341_RDMODE);
	Serial.print("Display Power Mode: 0x"); Serial.println(x, HEX);
	x = ILI9341.readcommand8(ILI9341_RDMADCTL);
	Serial.print("MADCTL Mode: 0x"); Serial.println(x, HEX);
	x = ILI9341.readcommand8(ILI9341_RDPIXFMT);
	Serial.print("Pixel Format: 0x"); Serial.println(x, HEX);
	x = ILI9341.readcommand8(ILI9341_RDIMGFMT);
	Serial.print("Image Format: 0x"); Serial.println(x, HEX);
	x = ILI9341.readcommand8(ILI9341_RDSELFDIAG);
	Serial.print("Self Diagnostic: 0x"); Serial.println(x, HEX);

	Serial.println(F("Benchmark                Time (microseconds)"));

	Serial.print(F("Screen fill              "));
	Serial.println(testFillScreen());
	delay(500);

	Serial.print(F("Text                     "));
	Serial.println(testText());
	delay(3000);

	Serial.print(F("Lines                    "));
	Serial.println(testLines(ILI9341_CYAN));
	delay(500);

	Serial.print(F("Horiz/Vert Lines         "));
	Serial.println(testFastLines(ILI9341_RED, ILI9341_BLUE));
	delay(500);

	Serial.print(F("Rectangles (outline)     "));
	Serial.println(testRects(ILI9341_GREEN));
	delay(500);

	Serial.print(F("Rectangles (filled)      "));
	Serial.println(testFilledRects(ILI9341_YELLOW, ILI9341_MAGENTA));
	delay(500);

	Serial.print(F("Circles (filled)         "));
	Serial.println(testFilledCircles(10, ILI9341_MAGENTA));

	Serial.print(F("Circles (outline)        "));
	Serial.println(testCircles(10, ILI9341_WHITE));
	delay(500);

	Serial.print(F("Triangles (outline)      "));
	Serial.println(testTriangles());
	delay(500);

	Serial.print(F("Triangles (filled)       "));
	Serial.println(testFilledTriangles());
	delay(500);

	Serial.print(F("Rounded rects (outline)  "));
	Serial.println(testRoundRects());
	delay(500);

	Serial.print(F("Rounded rects (filled)   "));
	Serial.println(testFilledRoundRects());
	delay(500);

	Serial.println(F("Done!"));

}


void loop(void) {
	for (uint8_t rotation = 0; rotation<4; rotation++) {
		ILI9341.setRotation(rotation);
		testText();
		delay(1000);
	}
}

unsigned long testFillScreen() {
	unsigned long start = micros();
	ILI9341.fillScreen(ILI9341_BLACK);
	ILI9341.fillScreen(ILI9341_RED);
	ILI9341.fillScreen(ILI9341_GREEN);
	ILI9341.fillScreen(ILI9341_BLUE);
	ILI9341.fillScreen(ILI9341_BLACK);
	return micros() - start;
}

unsigned long testText() {
	ILI9341.fillScreen(ILI9341_BLACK);
	unsigned long start = micros();
	ILI9341.setCursor(0, 0);
	ILI9341.setTextColor(ILI9341_WHITE);  ILI9341.setTextSize(1);
	ILI9341.println("Hello World!");
	ILI9341.setTextColor(ILI9341_YELLOW); ILI9341.setTextSize(2);
	ILI9341.println(1234.56);
	ILI9341.setTextColor(ILI9341_RED);    ILI9341.setTextSize(3);
	ILI9341.println(0xDEADBEEF, HEX);
	ILI9341.println();
	ILI9341.setTextColor(ILI9341_GREEN);
	ILI9341.setTextSize(5);
	ILI9341.println("Groop");
	ILI9341.setTextSize(2);
	ILI9341.println("I implore thee,");
	ILI9341.setTextSize(1);
	ILI9341.println("my foonting turlingdromes.");
	ILI9341.println("And hooptiously drangle me");
	ILI9341.println("with crinkly bindlewurdles,");
	ILI9341.println("Or I will rend thee");
	ILI9341.println("in the gobberwarts");
	ILI9341.println("with my blurglecruncheon,");
	ILI9341.println("see if I don't!");
	return micros() - start;
}

unsigned long testLines(uint16_t color) {
	unsigned long start, t;
	int           x1, y1, x2, y2,
		w = ILI9341.width(),
		h = ILI9341.height();

	ILI9341.fillScreen(ILI9341_BLACK);

	x1 = y1 = 0;
	y2 = h - 1;
	start = micros();
	for (x2 = 0; x2<w; x2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);
	x2 = w - 1;
	for (y2 = 0; y2<h; y2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);
	t = micros() - start; // fillScreen doesn't count against timing

	ILI9341.fillScreen(ILI9341_BLACK);

	x1 = w - 1;
	y1 = 0;
	y2 = h - 1;
	start = micros();
	for (x2 = 0; x2<w; x2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);
	x2 = 0;
	for (y2 = 0; y2<h; y2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);
	t += micros() - start;

	ILI9341.fillScreen(ILI9341_BLACK);

	x1 = 0;
	y1 = h - 1;
	y2 = 0;
	start = micros();
	for (x2 = 0; x2<w; x2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);
	x2 = w - 1;
	for (y2 = 0; y2<h; y2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);
	t += micros() - start;

	ILI9341.fillScreen(ILI9341_BLACK);

	x1 = w - 1;
	y1 = h - 1;
	y2 = 0;
	start = micros();
	for (x2 = 0; x2<w; x2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);
	x2 = 0;
	for (y2 = 0; y2<h; y2 += 6) ILI9341.drawLine(x1, y1, x2, y2, color);

	return micros() - start;
}

unsigned long testFastLines(uint16_t color1, uint16_t color2) {
	unsigned long start;
	int           x, y, w = ILI9341.width(), h = ILI9341.height();

	ILI9341.fillScreen(ILI9341_BLACK);
	start = micros();
	for (y = 0; y<h; y += 5) ILI9341.drawFastHLine(0, y, w, color1);
	for (x = 0; x<w; x += 5) ILI9341.drawFastVLine(x, 0, h, color2);

	return micros() - start;
}

unsigned long testRects(uint16_t color) {
	unsigned long start;
	int           n, i, i2,
		cx = ILI9341.width() / 2,
		cy = ILI9341.height() / 2;

	ILI9341.fillScreen(ILI9341_BLACK);
	n = min(ILI9341.width(), ILI9341.height());
	start = micros();
	for (i = 2; i<n; i += 6) {
		i2 = i / 2;
		ILI9341.drawRect(cx - i2, cy - i2, i, i, color);
	}

	return micros() - start;
}

unsigned long testFilledRects(uint16_t color1, uint16_t color2) {
	unsigned long start, t = 0;
	int           n, i, i2,
		cx = ILI9341.width() / 2 - 1,
		cy = ILI9341.height() / 2 - 1;

	ILI9341.fillScreen(ILI9341_BLACK);
	n = min(ILI9341.width(), ILI9341.height());
	for (i = n; i>0; i -= 6) {
		i2 = i / 2;
		start = micros();
		ILI9341.fillRect(cx - i2, cy - i2, i, i, color1);
		t += micros() - start;
		// Outlines are not included in timing results
		ILI9341.drawRect(cx - i2, cy - i2, i, i, color2);
	}

	return t;
}

unsigned long testFilledCircles(uint8_t radius, uint16_t color) {
	unsigned long start;
	int x, y, w = ILI9341.width(), h = ILI9341.height(), r2 = radius * 2;

	ILI9341.fillScreen(ILI9341_BLACK);
	start = micros();
	for (x = radius; x<w; x += r2) {
		for (y = radius; y<h; y += r2) {
			ILI9341.fillCircle(x, y, radius, color);
		}
	}

	return micros() - start;
}

unsigned long testCircles(uint8_t radius, uint16_t color) {
	unsigned long start;
	int           x, y, r2 = radius * 2,
		w = ILI9341.width() + radius,
		h = ILI9341.height() + radius;

	// Screen is not cleared for this one -- this is
	// intentional and does not affect the reported time.
	start = micros();
	for (x = 0; x<w; x += r2) {
		for (y = 0; y<h; y += r2) {
			ILI9341.drawCircle(x, y, radius, color);
		}
	}

	return micros() - start;
}

unsigned long testTriangles() {
	unsigned long start;
	int           n, i, cx = ILI9341.width() / 2 - 1,
		cy = ILI9341.height() / 2 - 1;

	ILI9341.fillScreen(ILI9341_BLACK);
	n = min(cx, cy);
	start = micros();
	for (i = 0; i<n; i += 5) {
		ILI9341.drawTriangle(
			cx, cy - i, // peak
			cx - i, cy + i, // bottom left
			cx + i, cy + i, // bottom right
			ILI9341.color565(0, 0, i));
	}

	return micros() - start;
}

unsigned long testFilledTriangles() {
	unsigned long start, t = 0;
	int           i, cx = ILI9341.width() / 2 - 1,
		cy = ILI9341.height() / 2 - 1;

	ILI9341.fillScreen(ILI9341_BLACK);
	start = micros();
	for (i = min(cx, cy); i>10; i -= 5) {
		start = micros();
		ILI9341.fillTriangle(cx, cy - i, cx - i, cy + i, cx + i, cy + i,
			ILI9341.color565(0, i, i));
		t += micros() - start;
		ILI9341.drawTriangle(cx, cy - i, cx - i, cy + i, cx + i, cy + i,
			ILI9341.color565(i, i, 0));
	}

	return t;
}

unsigned long testRoundRects() {
	unsigned long start;
	int           w, i, i2,
		cx = ILI9341.width() / 2 - 1,
		cy = ILI9341.height() / 2 - 1;

	ILI9341.fillScreen(ILI9341_BLACK);
	w = min(ILI9341.width(), ILI9341.height());
	start = micros();
	for (i = 0; i<w; i += 6) {
		i2 = i / 2;
		ILI9341.drawRoundRect(cx - i2, cy - i2, i, i, i / 8, ILI9341.color565(i, 0, 0));
	}

	return micros() - start;
}

unsigned long testFilledRoundRects() {
	unsigned long start;
	int           i, i2,
		cx = ILI9341.width() / 2 - 1,
		cy = ILI9341.height() / 2 - 1;

	ILI9341.fillScreen(ILI9341_BLACK);
	start = micros();
	for (i = min(ILI9341.width(), ILI9341.height()); i>20; i -= 6) {
		i2 = i / 2;
		ILI9341.fillRoundRect(cx - i2, cy - i2, i, i, i / 8, ILI9341.color565(0, i, 0));
	}

	return micros() - start;
}
```