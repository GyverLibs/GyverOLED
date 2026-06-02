This is an automatic translation and may be incorrect in some places. See the source README and examples for authoritative information.

[![latest](https://img.shields.io/github/v/release/GyverLibs/GyverOLED.svg?color=brightgreen)](https://github.com/GyverLibs/GyverOLED/releases/latest/download/GyverOLED.zip)
[![PIO](https://badges.registry.platformio.org/packages/gyverlibs/library/GyverOLED.svg)](https://registry.platformio.org/libraries/gyverlibs/GyverOLED)
[![Foo](https://img.shields.io/badge/Website-AlexGyver.ru-blue.svg?style=flat-square)](https://alexgyver.ru/)
[![Foo](https://img.shields.io/badge/%E2%82%BD%24%E2%82%AC%20%D0%9F%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%8C-%D0%B0%D0%B2%D1%82%D0%BE%D1%80%D0%B0-orange.svg?style=flat-square)](https://alexgyver.ru/support_alex/)
[![Foo](https://img.shields.io/badge/README-ENGLISH-blueviolet.svg?style=flat-square)](https://github-com.translate.goog/GyverLibs/GyverOLED?_x_tr_sl=ru&_x_tr_tl=en)  

[![Foo](https://img.shields.io/badge/ПОДПИСАТЬСЯ-НА%20ОБНОВЛЕНИЯ-brightgreen.svg?style=social&logo=telegram&color=blue)](https://t.me/GyverLibs)

# GyverOLED
Lightweight and fast library for OLED display
- Support for OLED displays on SSD1306/SSH1106 with a resolution of 128x64 and 128x32 with I2C and SPI connection
- Choosing a buffer
    - No buffer at all (and no loss of opportunities)
    - Buffer on the MK side (spending a lot of RAM, but more convenient to work)
    - Update the buffer at the selected location (for quick rendering)
    - Dynamic buffer of the selected size (all geometry, text, bytes)
    - TODO: Display side buffer (SSH1106 only!!!)
- Conclusion
    - The fastest text output among OLED libraries
    - Support for the Russian language and the letter E (!)
    - More pleasant font (compared to beta)
    - Outside display coordinates for scrollability
    - Output of text to any point (pixel addressing)
    - Full-screen output with the removal of unnecessary gaps
    - 4 letter sizes (on the basis of one font, saves a lot of memory!)
    - Ability to write black in white and white in black
- Display control
    - Brightness setting
    - Quick inversion of the entire display
    - Turning on/off the display from the sketch
    - Change the orientation of the display (mirror vertically and horizontally)
- Graphics (contour, pouring, cleaning)
    - Points
    - Lines
    - Rectangles
    - Rectangles with rounded corners
    - Circles
    - Bezier curves
- Images (bitmap)
    - Output of bitmap to any point of the display
    - Conclusion behind the display
    - Image conversion software is available in the library
- MicroWire library support for ATmega328 (very easy and fast output)

### Compatibility
Compatible with all Arduino platforms (Arduino features are used)

## Contents
- [Installation](#install)
- [Initialization](#init)
- [Use of use](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found under the name **GyverOLED** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download the library](https://github.com/GyverLibs/GyverOLED/archive/refs/heads/main.zip).zip archive for manual installation:
    - Unpack and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unpack and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/ *
    - (Arduino IDE) Automatic installation from .zip: *Sketch/Connect library/Add .ZIP library...* and specify downloaded archive
- Read more detailed instructions for installing libraries[here](https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Update
- I recommend always updating the library: new versions fix errors and bugs, as well as optimize and add new features.
- Through the library manager IDE: find the library as when installing and click "Update"
- Manually: **Delete the folder with the old version** and then put the new one in its place. “Replacement” can not be done: sometimes new versions delete files that will remain when replaced and can lead to errors!

<a id="init"></a>
## Initialization
```cpp
// I2C
GyverOLED<SSD1306_128x32, OLED_BUFFER> oled;        // buffered
GyverOLED<SSD1306_128x32, OLED_NO_BUFFER> oled;     // bufferless
GyverOLED<SSD1306_128x64, OLED_BUFFER> oled;        // buffered
GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled;     // bufferless
GyverOLED<SSH1106_128x64> oled;                     // software buffer
// You can send the address oled(0x3C) to the designer.

// SPI
GyverOLED<SSD1306_128x64, OLED_BUFFER, OLED_SPI, CS, DS, RST> oled;
// where CS, DS, RST are digital pins
// Display connects D0 to SCK, D1 to MOSI

// Note: you can overclock the i2c bus to increase the refresh rate of the display. Call out.
// Wire.setClock(800000L);
// after init() display initialization
// Note: Not all other i2c modules and sensors will work at this frequency.
```

<a id="usage"></a>
## Use of use
```cpp
// ======SERVICE ======
void init();                    // initialization
void init(sda, scl);            // initialization (you can specify i2c pins for esp8266/32)

void clear();                   // clear out
void clear(int x0, int y0, int x1, int y1); // clear out
void setContrast(uint8_t value);    // brightness 0-255 (silent 127)
void setPower(bool mode);       // off
void flipH(bool mode);          // horizontally
void invertDisplay(bool mode);  // reverse
void flipV(bool mode);          // vertically

// === = = = = = = = =
// Definition before connecting the library
#define OLED_NO_PRINT   // disable the text output module. Savings ~2.5 kB of Flash
#define OLED_SPI_SPEED  // speed

// ==========
// Inherit the Print class, i.e. print/println any type of data

void autoPrintln(bool mode);    // text-transfer
void home();                    // cursor
void setCursor(int x, int y);   // cursor for the symbol column 0-127, line 0-8(4)
void setCursorXY(int x, int y); // cursor for the symbol column 0-127, pixel 0-63(31)
void setScale(uint8_t scale);   // font scale (1-4)
void invertText(bool inv);      // reverse
bool isEnd();                   // returns true if the display is "out" - with the letter output

void textMode(byte mode);       // rendering
BUF_ADD - добавить
BUF_SUBTRACT - вычесть
BUF_REPLACE - заменить

// ======Graphics =====
// further fill:
OLED_CLEAR - очистить область под фигурой
OLED_FILL - залить фигуру
OLED_STROKE - обвести фигуру

void dot(int x, int y, byte fill);                      // point (fill 1/0)
void line(int x0, int y0, int x1, int y1, byte fill);   // line (x0, y0, x1, y1)
void fastLineH(int y, int x0, int x1, byte fill);       // horizontal
void fastLineV(int x, int y0, int y1, byte fill);       // vertical
void rect(int x0, int y0, int x1, int y1, byte fill);   // rectangle (top left, bottom right)
void roundRect(int x0, int y0, int x1, int y1, byte fill);  // The upper right, the upper right, the lower right.
void circle(int x, int y, int radius, byte fill);       // circumference (center x, center y, radius, fill)
void bezier(int* arr, uint8_t size, uint8_t dense, uint8_t fill);   // bezier

// bitmap
// invert - BITMAP NORMAL/BITMAP INVERT invert
// mode BUF_ADD / BUF_SUBTRACT / BUF_REPLACE
void drawBitmap(int x, int y, const uint8_t *frame, int width, int height, uint8_t invert = 0, byte mode = 0);

void fill(uint8_t data);                        // fill the entire display with the specified byte
void drawByte(uint8_t data);                    // sends bytes to setCursor() and setCursorXY()
void drawBytes(uint8_t* data, byte size);       // output one-dimensional byte array (linear bitmap height 8)
void update();                                  // completely update the display from the buffer
void update(int x0, int y0, int x1, int y1);    // selectively update the display from the buffer (x0, y0, x1, y1)
```

### Display by SPI + SD card
SD card troublehttp://elm-chan.org/docs/mmc/mmc_e.html, 
Cosideration on Multi-Slave Configuration. How to solve: after the end of communication
With a memory card, you need to release the CS card (the SD beebla may do it herself, or release it manually)
And throw a pair of bytes on SPI (a couple of zeros conditionally). Why - the card holds the date line.

<a id="example"></a>
## Example
For more examples see **examples**!
```cpp
// Define before connecting a liba - use microWire (light liba for I2C)
//#define USE_MICRO_WIRE

// Define before connecting liba - SPI speed
//#define OLED_SPI_SPEED 4000000ul

#include <GyverOLED.h>

// initialization:
// GyverOLED <model, buffer, interface, CS, DC, RST> oled;
// "default" - you may not specify

// display model:
// SSD1306_128x32
// SSD1306_128x64
// SSH1106 128x64 (Buffer only)

// buffer:
// OLED NO BUFFER (no buffer)
// OLED BUFFER (with buffer on the MK side) - by default

// interface
// OLED I2C - by default
// OLED SPI (specify CS, DC, RST/RES pins)

// examples:
//GyverOLED<SSD1306_128x32, OLED_BUFFER> oled;
//GyverOLED<SSD1306_128x32, OLED_NO_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_BUFFER, OLED_SPI, 8, 7, 6> oled;
GyverOLED<SSH1106_128x64> oled;

// For I2C, you can send the address: GyverOLED oled(0x3C);

// bitmap created in ImageProcessorhttps://github.com/AlexGyver/imageProcessor
// with vertical byte output parameters (OLED)
const uint8_t bitmap_32x32[] PROGMEM = {
  0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0xC0, 0xC0, 0xE0, 0xF0, 0x70, 0x70, 0x30, 0x30, 0x30, 0x20, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xC0, 0xE0, 0xF0, 0xF0, 0x70, 0x30, 0x30, 0x20, 0x00, 0x00,
  0x00, 0x30, 0x78, 0xFC, 0x7F, 0x3F, 0x0F, 0x0F, 0x1F, 0x3C, 0x78, 0xF0, 0xE0, 0xC0, 0x80, 0x80, 0x80, 0x40, 0xE0, 0xF0, 0xF8, 0xFC, 0xFF, 0x7F, 0x33, 0x13, 0x1E, 0x1C, 0x1C, 0x0E, 0x07, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0xC0, 0xE0, 0xF0, 0xF9, 0xF7, 0xEF, 0x5F, 0x3F, 0x7F, 0xFE, 0xFD, 0xFB, 0xF1, 0xE0, 0xC0, 0x80, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x0C, 0x1E, 0x33, 0x33, 0x1F, 0x0F, 0x07, 0x03, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x03, 0x07, 0x0F, 0x1F, 0x3F, 0x1F, 0x0E, 0x04, 0x00, 0x00, 0x00, 0x00,
};

void setup() {
  Serial.begin(9600);
  oled.init();  // initialization

  // --------------------------
  // I2C speed setting
  //Wire.setClock(800000L); // Max. 800'000

  // --------------------------
  oled.clear();   // clear the display (or buffer)
  oled.update();  // Update. Only for the buffer regime! OLED BUFFER

  // --------------------------
  oled.home();            // 0.0
  oled.print("Hello!");   // Type anything: numbers, strings, float like Serial!
  oled.update();
  delay(2000);

  // --------------------------
  oled.setCursor(5, 1);   // cursor in (pixel X, line Y)
  oled.setScale(2);
  oled.print("Hello!");
  oled.update();
  delay(2000);

  // --------------------------
  oled.setCursorXY(15, 30); // cursor in (pixel X, pixel Y)
  oled.setScale(3);
  oled.invertText(true);    // Invert the text!
  oled.print("Привет!");
  oled.update();
  delay(2000);

  // --------------------------
  oled.clear();
  oled.home();
  oled.setScale(1);
  oled.invertText(false);
  oled.autoPrintln(true);   // text-transfer
  oled.print(F("Lorem ipsum dolor sit amet, лорем ипсум долор сит амет привет народ ё, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam"));
  oled.update();
  delay(2000);

  // --------------------------
  oled.home();
  oled.textMode(BUF_ADD);
  // BUF ADD - Overlay text
  // BUF SUBTRACT - subtract the text
  // BUF REPLACE - Replace (the entire rectangle of the letter)
  oled.home();
  oled.setScale(3);
  oled.print("KEK!");
  oled.update();
  delay(2000);

  // --------------------------
  // Service.
  //oled.setContrast(10); // brightness 0.. 15.
  //oled.setPower(true); // true/false - turn on/off display
  //oled.flipH(true); // true/false - mirror horizontally
  //oled.flipV(true); // true/false - mirror vertically
  //oled.isEnd(); // returns true if the display is "out" - with the letter output

  // --------------------------
  oled.clear();
  oled.dot(0, 0);     // point
  oled.dot(0, 1, 1);  // third argument: 0 off pixel, 1 on pixel (by default)
  oled.line(5, 5, 10, 10);        // line x0,y0,x1 . y1
  //oled.line(5, 5, 10, 10, 0); Fifth argument: 0 erase, 1 draw (in silence)
  oled.fastLineH(0, 5, 10);       // horizontal line (y, x1, x2)
  //oled.fastLineH(0, 5, 10, 0) Fourth argument: 0 erase, 1 draw (in silence)
  oled.fastLineV(0, 5, 10);       // similar to the vertical line (x, y1, y2)
  oled.rect(20, 20, 30, 25);      // rectangle (x0,y0,x1,y1)
  oled.rect(5, 35, 35, 60, OLED_STROKE);      // rectangle (x0,y0,x1,y1)
  // Figure parameters:
  // OLED CLEAR - clean up
  // OLED FILL - pour
  // OLED STROKE - Draw a frame
  oled.roundRect(50, 5, 80, 25, OLED_STROKE);  // rounded
  oled.circle(60, 45, 15, OLED_STROKE);        // a circle centered in (x,y, radius)
  oled.circle(60, 45, 5, OLED_FILL);           // Fourth argument: parameter of the figure

  // bitmap
  oled.drawBitmap(90, 16, bitmap_32x32, 32, 32, BITMAP_NORMAL, BUF_ADD);
  //oled.drawBitmap(90, 16, bitmap 32x32, 32, 32) Shut up. normal and BUF ADD
  // x, y, name, width, height, BITMAP NORMAL(0)/BITMAP INVERT(1), BUF ADD/BUF SUBTRACT/BUF REPLACE
  
  oled.update();
}

void loop() {
}
```

<a id="versions"></a>
## Versions
- v0.1 (27.02.2021) - corrected the non-printable bottom line
- v0.2 (16.03.2021) - corrected symbols [|]~$
- v0.3 (26.03.2021) - added the Bezier curve
- v0.4 (10.04.2021) - compatibility with ESP
- v0.5 (09.05.2021) - SPI and SSH1106 support added (buffer only)! gnd-vcc-sck-data-rst-dc-cs
    
- v1.0 - release
- v1.1 - improved line transfer (does not remove the first character just like that)
- v1.2 - redesigned FastIO
- v1.3 - rectangles can be drawn from any angle
- v1.3.1 - fixed lines (broken in 1.3.0)
- v1.3.2 - FastIO removed
- v1.4 - Fixed SPI displays
- v1.5 - Fixed broken output after cleaning without cursor indication
- v1.6 - added selection of I2C pins for espX, fixed clear(..) for BUFFER, added the ability to disable the text module
- v1.6.1 - Re-release for the library manager

<a id="feedback"></a>
## Bugs and feedback
If you find bugs, create **Issue**, or better write to the mail immediately.[alex@alexgyver.ru](mailto:alex@alexgyver.ru)  
The library is open for revision and your **Pull Requests*!

When reporting bugs or incorrect work of the library, it is necessary to specify:
- Library version
- What is used by the IC
- SDK version (for ESP)
- Arduino IDE version
- Are embedded examples that use features and designs that cause bugs in your code working correctly?
- What code was downloaded, what work was expected from it and how it works in reality
- Ideally, attach the minimum code in which the bug is observed. Not a canvas of a thousand lines, but a minimum code.
