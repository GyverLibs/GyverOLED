This is an automatic translation, may be incorrect in some places. See sources and examples!

# GyverOLED
Lightweight and fast library for OLED display
- Support for OLED displays on SSD1306 / SSH1106 with a resolution of 128x64 and 128x32 with I2C and SPI connection
- Buffer selection
    - No buffer at all (and no loss of features)
    - Buffer on the MK side (spends a lot of RAM, but it's more convenient to use)
    - Update the buffer in the selected location (for fast rendering)
    - Dynamic buffer of selected size (all geometry, text, bytes)
    - TODO: Buffer on display side (only for SSH1106!!!)
- Text output
    - Fastest text output among OLED libraries
    - Support for the Russian language and the letter ё (!)
    - Nicer font (compared to beta)
    - Off-display coordinates for scrolling
    - Text output to any point (per pixel addressing)
    - Fullscreen output with extra spaces removed
    - 4 letter sizes (based on one font, saves a lot of memory!)
    - Ability to write in black-on-white and white-on-black
- Display control
    - Brightness setting
    - Quick inversion of the entire display
    - Turn on / off the display from the sketch
    - Changing display orientation (mirrored vertically and horizontally)
- Graphics (contour, fill, clear)
    - Points
    - Lines
    - Rectangles
    - Rectangles with rounded corners
    - Circles
    - Bezier curves
- Images (bitmap)
    - Bitmap output to any point on the display
    - Conclusion "forDisplay"
    - The program for converting images is in the library
- Support for microWire library for ATmega328 (very easy and fast output)

### Compatibility
Compatible with all Arduino platforms (using Arduino functions)

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found by the name **GyverOLED** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/GyverOLED/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unzip and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE% D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
```cpp
// I2C
GyverOLED<SSD1306_128x32, OLED_BUFFER> oled; // with buffer
GyverOLED<SSD1306_128x32, OLED_NO_BUFFER> oled; // no buffer
GyverOLED<SSD1306_128x64, OLED_BUFFER> oled; // with buffer
GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled; // no buffer
GyverOLED<SSH1106_128x64> oled; // software buffer only
// constructor can be passed address oled(0x3C)

// SPI
GyverOLED<SSD1306_128x64, OLED_BUFFER, OLED_SPI, CS, DS, RST> oled;
// where CS, DS, RST are digital pins
// display connects D0 to SCK, D1 to MOSI
```

<a id="usage"></a>
## Usage
```cpp
// ===== SERVICE =====
void init(); // andcranberry initialization
void clear(); // clear display
void clear(int x0, int y0, int x1, int y1); // clear area
void setContrast(uint8_t value); // brightness 0-255 (default 127)
void setPower(bool mode); // on off
void flipH(bool mode); // flip horizontally
void invertDisplay(bool mode); // invert display
void flipV(bool mode); // flip vertically

// ===== PRINT =====
// inherits the Print class, i.e. print/println any data type

void autoPrintln(bool mode); // automatically wrap text
voidhome(); // send cursor to 0,0
void setCursor(int x, int y); // put cursor on character column 0-127, row 0-8(4)
void setCursorXY(int x, int y); // put cursor on character column 0-127, pixel 0-63(31)
void setScale(uint8_tscale); // font scale (1-4)
void invertText(bool inv); // invert text (0-1)
bool isEnd(); // returns true if the display is "ended" - for letter-by-letter output

void textMode(byte mode); // text rendering mode
BUF_ADD - add
BUF_SUBTRACT - subtract
BUF_REPLACE - replace

// ===== GRAPHICS =====
// further fill:
OLED_CLEAR - clear the area under the figure
OLED_FILL - fill the shape
OLED_STROKE - stroke shape

void dot(int x, int y, byte fill); // dot (fill 1/0)
void line(int x0, int y0, int x1, int y1, byte fill); // line (x0, y0, x1, y1)
void fastLineH(int y, int x0, int x1, byte fill); // horizontal line
void fastLineV(int x, int y0, int y1, byte fill); // vertical line
void rect(int x0, int y0, int x1, int y1, byte fill); // rectangle (top left, bottom right)
void roundRect(int x0, int y0, int x1, int y1, byte fill); // rounded rectangle (top left, bottom right)
void circle(int x, int y, int radius, byte fill); // circle (center x, center y, radius, fill)
void bezier(int* arr, uint8_t size, uint8_t dense, uint8_t fill); // bezier curve

// output bitmap
// invert - BITMAP_NORMAL/BITMAP_INVERT invert
// mode BUF_ADD / BUF_SUBTRACT / BUF_REPLACE
void drawBitmap(int x, int y, const uint8_t *frame, int width, int height, uint8_t invert = 0, byte mode = 0);

void fill(uint8_t data); // fill the entire display with the specified byte
void drawByte(uint8_tdata); // sends bytes to "column" setCursor() and setCursorXY()
void drawBytes(uint8_t* data, byte size); // output a one-dimensional byte array (linear bitmap height 8)
void update(); // completely update the display from the buffer
void update(int x0, int y0, int x1, int y1); // selectively update display from buffer (x0, y0, x1, y1)
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
// define before connecting lib - use microWire (easy lib for I2C)
//#define USE_MICRO_WIRE

// define before connecting lib - SPI speed
//#define OLED_SPI_SPEED 4000000ul

#include <GyverOLED.h>

// initialization:
// GyverOLED<model, buffer, interface, CS, DC, RST> oled;
// "default" - can be omitted

// display model:
// SSD1306_128x32
// SSD1306_128x64
// SSH1106_128x64 (BUFFER ONLY)

// buffer:
// OLED_NO_BUFFER (no buffer)
// OLED_BUFFER (with a buffer on the MK side) - by default

// interface:
// OLED_I2C - default
// OLED_SPI (specify pins CS, DC, RST/RES)

// examples:
//GyverOLED<SSD1306_128x32, OLED_BUFFER> oled;
//GyverOLED<SSD1306_128x32, OLED_NO_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_BUFFER, OLED_SPI, 8, 7, 6> oled;
GyverOLED<SSH1106_128x64> oled;

// for I2C, you can pass the address: GyverOLED oled(0x3C);

// bitmap created in ImageProcessor https://github.com/AlexGyver/imageProcessor
// with output parameters vertical byte (OLED)
const uint8_t bitmap_32x32[] PROGMEM = {
  0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0xC0Cranberry, 0xc0, 0xe0, 0xf0, 0x70, 0x70, 0x30, 0x30, 0x30, 0x20, 0x00, 0x00, 0x00, 0x00, 0x00, 0xc0, 0xe0, 0xf0, 0xf0, 0x70, 0x30, 0x30, 0x20, 0x00, 0x00, 0x00,
  0x00 0x30 0x78 0xFC 0x7F 0x3F 0x0F 0x0F 0x1F 0x3C 0x78 0xF0 0xE0 0xC0 0x80 0x80 0x80 0x40 0xE0 0x13, 0x1E, 0x1C, 0x1C, 0x0E, 0x07, 0x00,
  0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x80 0xC0 0xE0 0xF0 0xF9 0xF7 0xEF 0x5F 0x3F 0x7F 0xFE 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x0C, 0x1E, 0x33, 0x33, 0x1F, 0x0F, 0x07, 0x03, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x03, 0x07, 0x0F, 0x1F, 0x3F, 0x1F, 0x0E, 0x04, 0x00, 0x00, 0x00, 0x00,
};

void setup() {
  Serial.begin(9600);
  oled.init(); // initialization

  // --------------------
  // set I2C speed
  //Wire.setClock(800000L); // max. 800'000

  // --------------------
  oled.clear(); // clear display (or buffer)
  oled update(); // update. Only for buffered mode! OLED_BUFFER

  // --------------------
  oled.home(); // cursor at 0,0
  oled.print("Hello!"); // print anything: numbers, strings, floats, like Serial!
  oled update();
  delay(2000);

  // --------------------
  oled.setCursor(5, 1); // cursor at (pixel X, row Y)
  oled.setScale(2);
  oled.print("Hello!");
  oled update();
  delay(2000);

  // --------------------
  oled.setCursorXY(15, 30); // cursor at (pixel X, pixel Y)
  oled.setScale(3);
  oled.invertText(true); // invert text!
  oled.print("Hello!");
  oled update();
  delay(2000);

  // --------------------
  oled.clear();
  oled.home();
  oled.setScale(1);
  oled.invertText(false);
  oled.autoprintln(true); // automatically wrap text
  oled.print(F("Lorem ipsum dolor sit amet, lorem ipsum dolor sit amet hello yo people, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna acranberry liqua. Ut enim ad minim veniam"));
  oled update();
  delay(2000);

  // --------------------
  oled.home();
  oled.textMode(BUF_ADD);
  // BUF_ADD - overlay text
  // BUF_SUBTRACT - subtract text
  // BUF_REPLACE - replace (whole letter rectangle)
  oled.home();
  oled.setScale(3);
  oled.print("KEK!");
  oled update();
  delay(2000);

  // --------------------
  // SERVICE
  //oled.setContrast(10); // brightness 0..15
  //oled.setPower(true); // true/false - enable/disable the display
  //oled.flipH(true); // true/false - flip horizontally
  //oled.flipV(true); // true/false - flip vertically
  //oled.isEnd(); // returns true if the display is "ended" - for letter-by-letter output

  // --------------------
  oled.clear();
  dot(0, 0); // point on x,y
  oled.dot(0, 1, 1); // third argument: 0 pixel off, 1 pixel on (default)
  oled.line(5, 5, 10, 10); // line x0,y0,x1,y1
  //oled.line(5, 5, 10, 10, 0); // fifth argument: 0 erase, 1 draw (default)
  oled.fastLineH(0, 5, 10); // horizontal line (y, x1, x2)
  //oled.fastLineH(0, 5, 10, 0); // fourth argument: 0 erase, 1 draw (default)
  oled.fastLineV(0, 5, 10); // similarly vert. line(x, y1, y2)
  oled.rect(20, 20, 30, 25); // rectangle (x0,y0,x1,y1)
  oled.rect(5, 35, 35, 60, OLED_STROKE); // rectangle (x0,y0,x1,y1)
  // shape parameters:
  // OLED_CLEAR - clear
  // OLED_FILL - fill
  // OLED_STROKE - draw a frame
  oled.roundRect(50, 5, 80, 25, OLED_STROKE); // similarly rounded rectangle
  oled.circle(60, 45, 15, OLED_STROKE); // circle centered at (x,y, with radius)
  oled.circle(60, 45, 5, OLED_FILL); // fourth argument: shape parameter

  // bitmap
  oled.drawBitmap(90, 16, bitmap_32x32, 32, 32, BITMAP_NORMAL, BUF_ADD);
  //oled.drawBitmap(90, 16, bitmap_32x32, 32, 32); // by default normal and BUF_ADD
  // x, y, name, width, height, BITMAP_NORMAL(0)/BITMAP_INVERT(1), BUF_ADD/BUF_SUBTRACT/BUF_REPLACE
  
  oled update();
}

void loop() {
}
```

<a id="versions"></a>
## Versions
- v0.1 (27.02.2021) - fixed non-printing bottom line
- v0.2 (16.03.2021) - fixed symbols [|]~$
- v0.3 (03/26/2021) - added bezier curve
- v0.4 (10.04.2021) - compatible with esp
- v0.5 (05/09/2021) - added support for SPI and SSH1106 (buffer only)! gnd- vcc-sck-data-rst-dc-cs
    
- v1.0 - release
- v1.1 - improved line wrapping (does not remove the first character just like that)
- v1.2 - redesigned FastIO
- v1.3 - rectangles can be drawn from any corner
- v1.3.1 - fixed lines (broke in 1.3.0)
- v1.3.2 - removed FastIO
- v1.4 - fixed SPI displays

<a id="feedback"></a>
## Bugs and feedback
When you find bugs, create an **Issue**, or better, immediately write to the mail [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'s!