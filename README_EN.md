This is an automatic translation, may be incorrect in some places. See sources and examples!

# Gyveroled
Light and fast library for OLED display
- Support for OLED display on SSD1306/SSH1106 with a resolution of 128x64 and 128x32 with connection by i2C and SPI
- Choosing a buffer
    - at all without a buffer (and without a special loss of possibilities)
    - buffer on the side of MK (spends a bunch of RAM, but more convenient in work)
    - Boofer update in the selected place (for quick rendering)
    - dynamic buffer of the selected size (all geometry, text, bytes)
    - Todo: Boofer on the display (only for SSH1106 !!!)
- Conclusion of the text
    - The fastest output of the text among OLED Library
    - Support for the Russian language and letters e (!)
    - A more pleasant font (compared to Beta)
    - coordinates outside the display for the possibility of scrolling
    - The output of the text to any point (Popixel addressing)
    - Full -screen conclusion with the removal of extra gaps
    - 4 sizes of letters (based on one font, saves a bunch of memory!)
    -the ability to write black and white and white-to-black
- display management
    - Installation of brightness
    - Fast inversion of the whole display
    - turning on/off the display from the sketch
    - change in the orientation of the display (mirror vertically and horizontal)
- graphics (contour, filling, cleaning)
    - Points
    - Lines
    - rectangles
    - Rectangles with rounded corners
    - Circles
    - Crooked Bezier
- Images (Bitmap)
    - Bitmap output to any display point
    - Conclusion "For display"
    - There is a program for converting images in the library
- Support for the Microwire library for Atmega328 (very light and quick conclusion)

## compatibility
Compatible with all arduino platforms (used arduino functions)

## Content
- [installation] (# Install)
- [initialization] (#init)
- [use] (#usage)
- [Example] (# Example)
- [versions] (#varsions)
- [bugs and feedback] (#fedback)

<a id="install"> </a>
## Installation
- The library can be found by the name ** gyveroled ** and installed through the library manager in:
    - Arduino ide
    - Arduino ide v2
    - Platformio
- [download the library] (https://github.com/gyverlibs/gyveroled/archive/refs/heads/main.zip) .Zip archive for manual installation:
    - unpack and put in * C: \ Program Files (X86) \ Arduino \ Libraries * (Windows X64)
    - unpack and put in * C: \ Program Files \ Arduino \ Libraries * (Windows X32)
    - unpack and put in *documents/arduino/libraries/ *
    - (Arduino id) Automatic installation from. Zip: * sketch/connect the library/add .Zip library ... * and specify downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%BD%D0%BE%BE%BE%BED0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Update
- I recommend always updating the library: errors and bugs are corrected in the new versions, as well as optimization and new features are added
- through the IDE library manager: find the library how to install and click "update"
- Manually: ** remove the folder with the old version **, and then put a new one in its place.“Replacement” cannot be done: sometimes in new versions, files that remain when replacing are deleted and can lead to errors!


<a id="init"> </a>
## initialCranberries
`` `CPP
// I2C
GyverOLED <SSD1306_128X32, OLED_BUFFER> OLED;// with a buffer
Gyveroled <SSD1306_128X32, OLED_NO_BUFFER> OLED;// without buffer
GyverOLED <SSD1306_128X64, OLED_BUFFER> OLED;// with a buffer
GyverOLED <SSD1306_128X64, OLED_NO_BUFFER> OLED;// without buffer
GyverOLED <SSH1106_128X64> OLED;// only software buffer
// to the designer you can send the address OLED (0x3c)

// SPI
Gyveroled <SSD1306_128X64, OLED_BUFFER, OLED_SPI, CS, DS, RST> OLED;
// where cs, ds, rst - digital pins
// display connects D0 to SCK, D1 to MOSI

// Note: You can disperse the i2C tire to increase the speed of updating the display.Call
// wire.setclock (800000l);
// after init () initialization of the display
// Note: Not all other i2C modules and sensors will work at such a frequency!
`` `

<a id="usage"> </a>
## Usage
`` `CPP
// ===== Service ========
VOID Init ();// Initialization
VOID Init (SDA, SCL);// initialization (you can specify pins i2c for ESP8266/32)

Void Clear ();// Clean the display
Void Clear (Int X0, Int Y0, Int X1, Int Y1);// Clean the region
VOID setContrast (Uint8_t Value);// brightness 0-255 (silence 127)
VOID setpower (Bool Mode);// on off
VOID FLIPH (Bool Mode);// Reflect horizontally
VOID Invertdisplay (Bool Mode);// Invert the display
Void Flipv (Bool Mode);// Reflect vertically

// ==== Settings ====
// Defines before connecting the library
#define oled_no_print // Disable the text output module.Saves ~ 2.5 kb flash
#define oled_spi_Speed // SPI speed

// ===== Print =======
// Inherits the Print class, that is, Print/Println any data type

Void Autoprintln (Bool Mode);// automatically tolerate text
VOID Home ();// Send the cursor at 0.0
VOID setcursor (Int X, Int y);// Put a cursor for the symbol column 0-127, line 0-8 (4)
VOID setcursorxy (int x, int y);// Put a cursor for the symbol column 0-127, pixel 0-63 (31)
VOID Setscale (Uint8_T Scale);// font scale (1-4)
VOID Invertext (Bool Inv);// Invert text (0-1)
Bool Isend ();// Returns True if the display "ends" - with a multiple conclusion

VOID Textmode (byte mode);// Text drawing mode
BUF_ADD - Add
BUF_SUBTRACT - subtract
Buf_replas - replace

// ===== graphics ========
// Next Fill:
Oled_Clear - Clean the area under the figure
Oled_Fill - Pour the figure
Oled_stroke - circle the figure

VOID DOT (Int X, Int Y, byte Fill);// Point (filling 1/0)
VOID Line (Int X0, Int Y0, Int X1, Int Y1, Byte Fill);// Line (X0, Y0, X1, Y1)
VOID Fastlineh (Int y, Int X0, Int X1, Byte Fill);// horizontal line
VOID FASTLINEV (Int X, Int Y0, Int Y1, Byte Fill);// Vertical line
VOID RECT (Int X0, Int Y0, Int X1, Int Y1, Byte Fill);// rectangle (lion. Verkhn, right. Nizhn)
VOID Roundrect (Int X0, Int Y0, Int X1, Int Y1, Byte Fill);// Rectangle rounded (lion. Verkhn, right. Nizhn)
VOID Circle (Int X, Int y, int radius, byte Fill);// circumference (center x, center U, radius, filling)
VOID Bezier (int* arr, uint8_t size, uint8_t dense, uint8_t fill);// Curve Bezier

// Bitmap
// invert - bitmap_normal/bitmap_invert invert
// Mode buf_add / buf_subtract / buf_replay
VOID DRAWBITMAP (Int X, Int Y, COST UINT8_T *FRAME, Int width, int Height, Uint8_t Invert = 0, Byte Mode = 0);

Void Fill (Uint8_t Data);// Pour the entire display specified byte
VOID DRAWBYTE (UINT8_T DATA);// Bait's helmet in the "column" setcursor () and setcursorxy ()
VOID DRAWBYTES (UINT8_T* DATA, Byte SIZE);// Bring one -dimensional byte array (linear bitmap 8)
VOID update ();// completely update the display from the buffer
VOID update (int X0, Int Y0, Int X1, Int Y1);// selectively update the display from the buffer (x0, y0, x1, y1)
`` `

### display on SPI + SD card
Famous trouble SD cards http: // ELM-Chan.org/docs/mmc/mmc_e.html,
COSIDERATION ON MULTI-SLAVE Configuration.How to solve: after the completion of communication
With a memory card, you need to release CS cards (the Bibla Bibla is perhaps does it yourself, or let go manually)
And throw a couple of bytes on SPI (a couple of zeros conditionally).Why - the map holds the line of the date.

<a id="EXAMPLE"> </a>
## Example
The rest of the examples look at ** Examples **!
`` `CPP
// Define before connecting the LIBA - use microWire (Light Liba for i2c)
//# Define use_micro_wire

// Define before connecting the libe - SPI speed
//# Define OLED_SPI_Speed 4000000UL

#include <gyveroled.h>

// Initialization:
// gyveroled <model, buffer, interface, CS, DC, RST> OLED;
// "by default" - you can not indicate

// Model display:
// SSD1306_128X32
// SSD1306_128X64
// ssh1106_128x64 (only with buffer)

// buffer:
// OLED_NO_BUFFER (without buffer)
// OLED_BUFFER (with a buffer on the MK side) - by default

// Interface:
// oled_i2c - by default
// oled_spi (specify pins CS, DC, RST/Res)

// Examples:
// GyverOLED <SSD1306_128X32, OLED_BUFFER> OLED;
// gyveroled <SSD1306_128X32, OLED_NO_BUFFER> OLED;
// GyverOLED <SSD1306_128X64, OLED_BUFFER> OLED;
// gyveroled <SSD1306_128X64, OLED_NO_BUFFER> OLED;
// GyverOLED <SSD1306_128X64, OLED_BUFFER, OLED_SPI, 8, 7, 6> OLED;
GyverOLED <SSH1106_128X64> OLED;

// for i2c you can transmit the address: gyveroled OLED (0x3C);

// Bitmap created in ImageProcessor https://github.com/alexgyver/imageprocessor
// with the output parameters Vertical byte (OLED)
const uint8_t bitmap_32x32 [] progmem = {{
  0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0xc0, 0xc0, 0xe0, 0xf0, 0x70, 0x70, 0x30, 0x30, 0x30, 0x20, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xc0, 0xe0, 0xf0, 0xf0,0xf0, 0x70, 0x30, 0x30, 0x20, 0x00, 0x00,
  0x00, 0x30, 0x78, 0xfc, 0x7f, 0x3f, 0x0f, 0x0f, 0x1f, 0x3c, 0x78, 0xf0, 0xe0, 0xc0, 0x80, 0x80, 0x80, 0x40, 0xe0, 0xf0, 0xf8, 0xfc, 0xFF, 0x7f, 0x33, 0x33, 0x33, 0x33, 0x33, 0x33, 0x33, 0x33, 0x33, 0x33, 0x0x13, 0x1e, 0x1c, 0x1c, 0x0e, 0x07, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0xc0, 0xe0, 0xf0, 0xf9, 0xf7, 0XEF, 0x5F, 0x3F, 0x7F, 0XFE, 0xfd, 0xfb, 0xf1, 0xe0, 0xc0, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80, 0x80,0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x0c, 0x1e, 0x33, 0x33, 0x1f, 0x0f, 0x07, 0x03, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x03, 0x07, 0x0f, 0x1f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f, 0x3f0x1f, 0x0e, 0x04, 0x00, 0x00, 0x00, 0x00,
};

VOID setup () {
  Serial.Begin (9600);
  oled.init ();// Initialization

  // -----------------------
  // Speed settings i2c
  //Wire.setclock(800000l);// Max.800'000

  // -----------------------
  oled.clear ();// Clean the display (or buffer)
  oled.update ();// update.Only for the regime with the buffer!OLED_BUFFER

  // -----------------------
  oled.home ();// Cursor at 0.0
  oled.print ("Hello!");// Print anything: numbers, lines, float, like serial!
  oled.update ();
  DELAY (2000);

  // -----------------------
  oled.Setcursor (5, 1);// cursor in (pixel x, line y)
  oled.Setscale (2);
  oled.print ("Hello!");
  oled.update ();
  DELAY (2000);

  // -----------------------
  oled.setcursorxy (15, 30);// cursor in (pixel x, pixel y)
  oled.Setscale (3);
  oled.invertext (True);// Invert the text!
  oled.print ("Hello!");
  oled.update ();
  DELAY (2000);

  // -----------------------
  oled.clear ();
  oled.home ();
  oled.setscale (1);
  oled.invertext (false);
  oled.autoprintln (True);// automatically tolerate text
  oled.print (f ("lorem ipsum dolor site amet, flamier ipsum sit amet Hello people ё, Consectur Adipiscing Elit, Sed Do -eiusmod Tempor IncidIDIDIDUNT UT DOLORE MAGNA ALIIKA. D minim Veniam "));
  oled.update ();
  DELAY (2000);

  // -----------------------
  oled.home ();
  oled.textmode (buf_add);
  // buf_add - impose a text
  // buf_subtract - subtract text
  // buf_replas - replace (the entire rectangle of the letter)
  oled.home ();
  oled.Setscale (3);
  oled.print ("Kek!");
  oled.update ();
  DELAY (2000);

  // -----------------------
  // SERVICE
  //OLED.SETCONTRAST (10);// brightness 0..15
  //OLED.SetPower(True);// True/false - turn on/off the display
  //OLED.FLIPH(True);// True/false - wink horizontally
  //OLED.FLIPV(True);// True/false - wiring vertically
  //OLED.Send ();// Returns True if the display "ends" - with a multiple conclusion

  // -----------------------
  oled.clear ();
  oled.dot (0, 0);// Point on X, Y
  oled.dot (0, 1, 1);// Third Argument: 0 Off Pixel, 1 ON PIXEL (SITION)
  oled.line (5, 5, 10, 10);// Line X0, Y0, X1, Y1
  //OLED.LINE (5, 5, 10, 10, 0);// Fifth argument: 0 erase, 1 draw (by the silence)
  oled.fastlineh (0, 5, 10);// horizontal line (y, x1, x2)
  //OLED.FASTLINEH, 5, 10, 0);// Fourth argument: 0 erase, 1 draw (by the silence)
  oled.fastlinev (0, 5, 10);// Similarly, vert.Line (X, Y1, Y2)
  oled.rect (20, 20, 30, 25);// rectangle (x0, y0, x1, y1)
  oled.rect (5, 35, 35, 60, oled_stroke);// rectangle (x0, y0, x1, y1)
  // Figure Parameters:
  // OLED_Clear - Clear
  // oled_fill - pour
  // oled_stroke - draw a frame
  oled.roundrect (50, 5, 80, 25, Oled_stroke);// similarly a rounded rectangle
  oled.circle (60, 45, 15, oled_stroke);// circumference with center B (x, y, with radius)
  oled.circle (60, 45, 5, oled_fill);// Fourth argument: figure parameter

  // Bitmap
  oled.drawbitmap (90, 16, Bitmap_32x32, 32, 32, Bitmap_normal, Buf_add);
  //OLELD.DRAWBITMAP (90, 16, bitmap_32x32, 32, 32);// by the silence.Normal and buf_add
  // x, y, name, width, height, bitmap_normal (0)/bitmap_invert (1), buf_add/buf_subtract/buf_replay
  
  oled.update ();
}

VOID loop () {
}
`` `

<a id="versions"> </a>
## versions
- v0.1 (02.27.2021) - corrected the unprintable lower line
- v0.2 (03/16/2021) - Fixed symbols [|] ~ $
- v0.3 (03/26/2021) - added Curve Bezier
- v0.4 (10.04.2021) - compatibility with ESP
- V0.5 (05.05.2021) - Support SPI and SSH1106 (only buffer) added!GND-VCC-SCK-DATA-RST-DC-CS
    
- v1.0 - release
- V1.1 - improved the transfer of lines (does not remove the first symbol just like that)
- V1.2 - Redeled Fastio
- v1.3 - rectangles can be drawn from any corner
- v1.3.1 - fixed the lines (broke in 1.3.0)
- v1.3.2 - removed Fastio
- V1.4 - Spi displays Faced
- v1.5 - a broken conclusion after cleaning without specifying the cursor
- V1.6 - The choice of I2C Pin for ESPX has been added, Clear (..) for buffer, added the ability to disable the text module
- V1.6.1 - re -release for the library manager

<a id="feedback"> </a>
## bugs and feedback
Create ** Issue ** when you find the bugs, and better immediately write to the mail [alex@alexgyver.ru] (mailto: alex@alexgyver.ru)
The library is open for refinement and your ** pull Request ** 'ow!


When reporting about bugs or incorrect work of the library, it is necessary to indicate:
- The version of the library
- What is MK used
- SDK version (for ESP)
- version of Arduino ide
- whether the built -in examples work correctly, in which the functions and designs are used, leading to a bug in your code
- what code has been loaded, what work was expected from it and how it works in reality
- Ideally, attach the minimum code in which the bug is observed.Not a canvas of a thousand lines, but a minimum code