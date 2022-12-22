[![latest](https://img.shields.io/github/v/release/GyverLibs/GyverOLED.svg?color=brightgreen)](https://github.com/GyverLibs/GyverOLED/releases/latest/download/GyverOLED.zip)
[![Foo](https://img.shields.io/badge/Website-AlexGyver.ru-blue.svg?style=flat-square)](https://alexgyver.ru/)
[![Foo](https://img.shields.io/badge/%E2%82%BD$%E2%82%AC%20%D0%9D%D0%B0%20%D0%BF%D0%B8%D0%B2%D0%BE-%D1%81%20%D1%80%D1%8B%D0%B1%D0%BA%D0%BE%D0%B9-orange.svg?style=flat-square)](https://alexgyver.ru/support_alex/)
[![Foo](https://img.shields.io/badge/README-ENGLISH-blueviolet.svg?style=flat-square)](https://github-com.translate.goog/GyverLibs/GyverOLED?_x_tr_sl=ru&_x_tr_tl=en)  

[![Foo](https://img.shields.io/badge/ПОДПИСАТЬСЯ-НА%20ОБНОВЛЕНИЯ-brightgreen.svg?style=social&logo=telegram&color=blue)](https://t.me/GyverLibs)

# GyverOLED
Лёгкая и быстрая библиотека для OLED дисплея
- Поддержка OLED дисплеев на SSD1306/SSH1106 с разрешением 128х64 и 128х32 с подключением по I2C и SPI
- Выбор буфера
    - Вообще без буфера (и без особой потери возможностей)
    - Буфер на стороне МК (тратит кучу оперативки, но удобнее в работе)
    - Обновление буфера в выбранном месте (для быстрой отрисовки)
    - Динамический буфер выбранного размера (вся геометрия, текст, байты)
    - TODO: Буфер на стороне дисплея (только для SSH1106!!!)
- Вывод текста
    - Самый быстрый вывод текста среди OLED библиотек
    - Поддержка русского языка и буквы ё (!)
    - Более приятный шрифт (по сравнению с beta)
    - Координаты вне дисплея для возможности прокрутки
    - Вывод текста в любую точку (попиксельная адресация)
    - Полноэкранный вывод с удалением лишних пробелов
    - 4 размера букв (на базе одного шрифта, экономит кучу памяти!)
    - Возможность писать чёрным-по-белому и белым-по-чёрному
- Управление дисплеем
    - Установка яркости
    - Быстрая инверсия всего дисплея
    - Включение/выключение дисплея из скетча
    - Изменение ориентации дисплея (зеркально по вертикали и горизонтали)
- Графика (контур, заливка, очистка)
    - Точки
    - Линии
    - Прямоугольники
    - Прямоугольники со скруглёнными углами
    - Окружности
    - Кривые Безье
- Изображения (битмап)
    - Вывод битмапа в любую точку дисплея
    - Вывод "за дисплей"
    - Программа для конвертации изображений есть в библиотеке
- Поддержка библиотеки microWire для ATmega328 (очень лёгкий и быстрый вывод)

### Совместимость
Совместима со всеми Arduino платформами (используются Arduino-функции)

## Содержание
- [Установка](#install)
- [Инициализация](#init)
- [Использование](#usage)
- [Пример](#example)
- [Версии](#versions)
- [Баги и обратная связь](#feedback)

<a id="install"></a>
## Установка
- Библиотеку можно найти по названию **GyverOLED** и установить через менеджер библиотек в:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Скачать библиотеку](https://github.com/GyverLibs/GyverOLED/archive/refs/heads/main.zip) .zip архивом для ручной установки:
    - Распаковать и положить в *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Распаковать и положить в *C:\Program Files\Arduino\libraries* (Windows x32)
    - Распаковать и положить в *Документы/Arduino/libraries/*
    - (Arduino IDE) автоматическая установка из .zip: *Скетч/Подключить библиотеку/Добавить .ZIP библиотеку…* и указать скачанный архив
- Читай более подробную инструкцию по установке библиотек [здесь](https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Обновление
- Рекомендую всегда обновлять библиотеку: в новых версиях исправляются ошибки и баги, а также проводится оптимизация и добавляются новые фичи
- Через менеджер библиотек IDE: найти библиотеку как при установке и нажать "Обновить"
- Вручную: **удалить папку со старой версией**, а затем положить на её место новую. "Замену" делать нельзя: иногда в новых версиях удаляются файлы, которые останутся при замене и могут привести к ошибкам!


<a id="init"></a>
## Инициализация
```cpp
// I2C
GyverOLED<SSD1306_128x32, OLED_BUFFER> oled;        // с буфером
GyverOLED<SSD1306_128x32, OLED_NO_BUFFER> oled;     // без буфера
GyverOLED<SSD1306_128x64, OLED_BUFFER> oled;        // с буфером
GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled;     // без буфера
GyverOLED<SSH1106_128x64> oled;                     // только программный буфер
// конструктору можно передать адрес oled(0x3C)

// SPI
GyverOLED<SSD1306_128x64, OLED_BUFFER, OLED_SPI, CS, DS, RST> oled;
// где CS, DS, RST - цифровые пины
// дисплей подключается D0 к SCK, D1 к MOSI
```

<a id="usage"></a>
## Использование
```cpp
// ===== СЕРВИС =====
void init();                    // инициализация
void init(sda, scl);            // инициализация (можно указать пины i2c для esp8266/32)

void clear();                   // очистить дисплей
void clear(int x0, int y0, int x1, int y1); // очистить область
void setContrast(uint8_t value);    // яркость 0-255 (умолч. 127)
void setPower(bool mode);       // вкл/выкл
void flipH(bool mode);          // отразить по горизонтали
void invertDisplay(bool mode);  // инвертировать дисплей
void flipV(bool mode);          // отразить по вертикали

// ==== НАСТРОЙКИ ===
// дефайны ПЕРЕД подключением библиотеки
#define OLED_NO_PRINT   // отключить модуль вывода текста. Экономит ~2.5 кБ Flash
#define OLED_SPI_SPEED  // скорость SPI

// ===== ПЕЧАТЬ =====
// наследует класс Print, то есть print/println любой тип данных

void autoPrintln(bool mode);    // автоматически переносить текст
void home();                    // отправить курсор в 0,0
void setCursor(int x, int y);   // поставить курсор для символа столбец 0-127, строка 0-8(4)
void setCursorXY(int x, int y); // поставить курсор для символа столбец 0-127, пиксель 0-63(31)
void setScale(uint8_t scale);   // масштаб шрифта (1-4)
void invertText(bool inv);      // инвертировать текст (0-1)
bool isEnd();                   // возвращает true, если дисплей "кончился" - при побуквенном выводе

void textMode(byte mode);       // режим отрисовки текста
BUF_ADD - добавить
BUF_SUBTRACT - вычесть
BUF_REPLACE - заменить

// ===== ГРАФИКА =====
// далее fill:
OLED_CLEAR - очистить область под фигурой
OLED_FILL - залить фигуру
OLED_STROKE - обвести фигуру

void dot(int x, int y, byte fill);                      // точка (заливка 1/0)
void line(int x0, int y0, int x1, int y1, byte fill);   // линия (x0, y0, x1, y1)
void fastLineH(int y, int x0, int x1, byte fill);       // горизонтальная линия
void fastLineV(int x, int y0, int y1, byte fill);       // вертикальная линия
void rect(int x0, int y0, int x1, int y1, byte fill);   // прямоугольник (лев. верхн, прав. нижн)	
void roundRect(int x0, int y0, int x1, int y1, byte fill);  // прямоугольник скруглённый (лев. верхн, прав. нижн)
void circle(int x, int y, int radius, byte fill);       // окружность (центр х, центр у, радиус, заливка)
void bezier(int* arr, uint8_t size, uint8_t dense, uint8_t fill);   // кривая Безье

// вывести битмап
// invert - BITMAP_NORMAL/BITMAP_INVERT инвертировать
// mode BUF_ADD / BUF_SUBTRACT / BUF_REPLACE
void drawBitmap(int x, int y, const uint8_t *frame, int width, int height, uint8_t invert = 0, byte mode = 0);

void fill(uint8_t data);                        // залить весь дисплей указанным байтом
void drawByte(uint8_t data);                    // шлёт байт в "столбик" setCursor() и setCursorXY()
void drawBytes(uint8_t* data, byte size);       // вывести одномерный байтовый массив (линейный битмап высотой 8)
void update();                                  // полностью обновить дисплей из буфера
void update(int x0, int y0, int x1, int y1);    // выборочно обновить дисплей из буфера (x0, y0, x1, y1)
```

### Дисплей по SPI + SD карта
известная беда SD карт http://elm-chan.org/docs/mmc/mmc_e.html, 
пункт Cosideration on Multi-slave Configuration. Как решить: после завершения общения 
с картой памяти нужно отпустить CS карты (библа SD возможно сама это делает, либо отпустить вручную) 
и закинуть по SPI пару байт (пару нулей условно). Почему - карта держит линию даты.

<a id="example"></a>
## Пример
Остальные примеры смотри в **examples**!
```cpp
// дефайн перед подключением либы - использовать microWire (лёгкая либа для I2C)
//#define USE_MICRO_WIRE

// дефайн перед подключением либы - скорость SPI
//#define OLED_SPI_SPEED 4000000ul

#include <GyverOLED.h>

// инициализация:
// GyverOLED<модель, буфер, интерфейс, CS, DC, RST> oled;
// "по умолчанию" - можно не указывать

// модель дисплея:
// SSD1306_128x32
// SSD1306_128x64
// SSH1106_128x64 (ТОЛЬКО С БУФЕРОМ)

// буфер:
// OLED_NO_BUFFER (без буфера)
// OLED_BUFFER (с буфером на стороне МК) - по умолчанию

// интерфейс:
// OLED_I2C - по умолчанию
// OLED_SPI (указать пины CS, DC, RST/RES)

// примеры:
//GyverOLED<SSD1306_128x32, OLED_BUFFER> oled;
//GyverOLED<SSD1306_128x32, OLED_NO_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled;
//GyverOLED<SSD1306_128x64, OLED_BUFFER, OLED_SPI, 8, 7, 6> oled;
GyverOLED<SSH1106_128x64> oled;

// для I2C можно передать адрес: GyverOLED oled(0x3C);

// битмап создан в ImageProcessor https://github.com/AlexGyver/imageProcessor
// с параметрами вывода vertical byte (OLED)
const uint8_t bitmap_32x32[] PROGMEM = {
  0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0xC0, 0xC0, 0xE0, 0xF0, 0x70, 0x70, 0x30, 0x30, 0x30, 0x20, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xC0, 0xE0, 0xF0, 0xF0, 0x70, 0x30, 0x30, 0x20, 0x00, 0x00,
  0x00, 0x30, 0x78, 0xFC, 0x7F, 0x3F, 0x0F, 0x0F, 0x1F, 0x3C, 0x78, 0xF0, 0xE0, 0xC0, 0x80, 0x80, 0x80, 0x40, 0xE0, 0xF0, 0xF8, 0xFC, 0xFF, 0x7F, 0x33, 0x13, 0x1E, 0x1C, 0x1C, 0x0E, 0x07, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0xC0, 0xE0, 0xF0, 0xF9, 0xF7, 0xEF, 0x5F, 0x3F, 0x7F, 0xFE, 0xFD, 0xFB, 0xF1, 0xE0, 0xC0, 0x80, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x0C, 0x1E, 0x33, 0x33, 0x1F, 0x0F, 0x07, 0x03, 0x01, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x03, 0x07, 0x0F, 0x1F, 0x3F, 0x1F, 0x0E, 0x04, 0x00, 0x00, 0x00, 0x00,
};

void setup() {
  Serial.begin(9600);
  oled.init();  // инициализация

  // --------------------------
  // настройка скорости I2C
  //Wire.setClock(800000L);   // макс. 800'000

  // --------------------------
  oled.clear();   // очистить дисплей (или буфер)
  oled.update();  // обновить. Только для режима с буфером! OLED_BUFFER

  // --------------------------
  oled.home();            // курсор в 0,0
  oled.print("Hello!");   // печатай что угодно: числа, строки, float, как Serial!
  oled.update();
  delay(2000);

  // --------------------------
  oled.setCursor(5, 1);   // курсор в (пиксель X, строка Y)
  oled.setScale(2);
  oled.print("Hello!");
  oled.update();
  delay(2000);

  // --------------------------
  oled.setCursorXY(15, 30); // курсор в (пиксель X, пиксель Y)
  oled.setScale(3);
  oled.invertText(true);    // инвертируй текст!
  oled.print("Привет!");
  oled.update();
  delay(2000);

  // --------------------------
  oled.clear();
  oled.home();
  oled.setScale(1);
  oled.invertText(false);
  oled.autoPrintln(true);   // автоматически переносить текст
  oled.print(F("Lorem ipsum dolor sit amet, лорем ипсум долор сит амет привет народ ё, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam"));
  oled.update();
  delay(2000);

  // --------------------------
  oled.home();
  oled.textMode(BUF_ADD);
  // BUF_ADD - наложить текст
  // BUF_SUBTRACT - вычесть текст
  // BUF_REPLACE - заменить (весь прямоугольник буквы)
  oled.home();
  oled.setScale(3);
  oled.print("KEK!");
  oled.update();
  delay(2000);

  // --------------------------
  // СЕРВИС
  //oled.setContrast(10);   // яркость 0..15
  //oled.setPower(true);    // true/false - включить/выключить дисплей
  //oled.flipH(true);       // true/false - отзеркалить по горизонтали
  //oled.flipV(true);       // true/false - отзеркалить по вертикали
  //oled.isEnd();           // возвращает true, если дисплей "кончился" - при побуквенном выводе

  // --------------------------
  oled.clear();
  oled.dot(0, 0);     // точка на x,y
  oled.dot(0, 1, 1);  // третий аргумент: 0 выкл пиксель, 1 вкл пиксель (по умолч)
  oled.line(5, 5, 10, 10);        // линия x0,y0,x1,y1
  //oled.line(5, 5, 10, 10, 0);   // пятый аргумент: 0 стереть, 1 нарисовать (по умолч)
  oled.fastLineH(0, 5, 10);       // горизонтальная линия (y, x1, x2)
  //oled.fastLineH(0, 5, 10, 0);  // четвёртый аргумент: 0 стереть, 1 нарисовать (по умолч)
  oled.fastLineV(0, 5, 10);       // аналогично верт. линия (x, y1, y2)
  oled.rect(20, 20, 30, 25);      // прямоугольник (x0,y0,x1,y1)
  oled.rect(5, 35, 35, 60, OLED_STROKE);      // прямоугольник (x0,y0,x1,y1)
  // параметры фигуры:
  // OLED_CLEAR - очистить
  // OLED_FILL - залить
  // OLED_STROKE - нарисовать рамку
  oled.roundRect(50, 5, 80, 25, OLED_STROKE);  // аналогично скруглённый прямоугольник
  oled.circle(60, 45, 15, OLED_STROKE);        // окружность с центром в (x,y, с радиусом)
  oled.circle(60, 45, 5, OLED_FILL);           // четвёртый аргумент: параметр фигуры

  // битмап
  oled.drawBitmap(90, 16, bitmap_32x32, 32, 32, BITMAP_NORMAL, BUF_ADD);
  //oled.drawBitmap(90, 16, bitmap_32x32, 32, 32);  // по умолч. нормал и BUF_ADD
  // x, y, имя, ширина, высота, BITMAP_NORMAL(0)/BITMAP_INVERT(1), BUF_ADD/BUF_SUBTRACT/BUF_REPLACE
  
  oled.update();
}

void loop() {
}
```

<a id="versions"></a>
## Версии
- v0.1 (27.02.2021) - исправил непечатающуюся нижнюю строку
- v0.2 (16.03.2021) - исправлены символы [|]~$
- v0.3 (26.03.2021) - добавил кривую Безье
- v0.4 (10.04.2021) - совместимость с есп
- v0.5 (09.05.2021) - добавлена поддержка SPI и SSH1106 (только буфер)! gnd-vcc-sck-data-rst-dc-cs
    
- v1.0 - релиз
- v1.1 - улучшен перенос строк (не убирает первый символ просто так)
- v1.2 - переделан FastIO
- v1.3 - прямоугольники можно рисовать из любого угла
- v1.3.1 - пофиксил линии (сломались в 1.3.0)
- v1.3.2 - убран FastIO
- v1.4 - пофикшены SPI дисплеи
- v1.5 - пофикшен битый вывод после очистки без указания курсора
- v1.6 - добавлен выбор пинов I2C для espX, исправлен clear(..) для BUFFER, добавлена возможность отключить модуль текста
- v1.6.1 - повторный релиз для менеджера библиотек

<a id="feedback"></a>
## Баги и обратная связь
При нахождении багов создавайте **Issue**, а лучше сразу пишите на почту [alex@alexgyver.ru](mailto:alex@alexgyver.ru)  
Библиотека открыта для доработки и ваших **Pull Request**'ов!


При сообщении о багах или некорректной работе библиотеки нужно обязательно указывать:
- Версия библиотеки
- Какой используется МК
- Версия SDK (для ESP)
- Версия Arduino IDE
- Корректно ли работают ли встроенные примеры, в которых используются функции и конструкции, приводящие к багу в вашем коде
- Какой код загружался, какая работа от него ожидалась и как он работает в реальности
- В идеале приложить минимальный код, в котором наблюдается баг. Не полотно из тысячи строк, а минимальный код
