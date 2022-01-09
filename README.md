# esp-idf-ssd1283
SSD1283 Driver for esp-idf.   
My SSD1283 is 1.6" 130x130.   

![graph-1](https://user-images.githubusercontent.com/6020549/126050614-814f0c07-6e0a-42d3-b91f-4002d781618e.JPG)

# Software requirements
esp-idf v4.4 or later.   
This is because this version supports ESP32-C3.   

# Installation for ESP32

```
git clone https://github.com/nopnop2002/esp-idf-ssd1283
cd esp-idf-ssd1283
idf.py set-target esp32
idf.py menuconfig
idf.py flash
```

# Installation for ESP32-S2

```
git clone https://github.com/nopnop2002/esp-idf-ssd1283
cd esp-idf-ssd1283
idf.py set-target esp32s2
idf.py menuconfig
idf.py flash
```

__Note__   
tjpgd library does not exist in ESP32-S2 ROM.   
Therefore, the JPEG file cannot be displayed.   

# Installation for ESP32-C3

```Shell
git clone https://github.com/nopnop2002/esp-idf-ssd1283
cd esp-idf-ssd1283
idf.py set-target esp32c3
idf.py menuconfig
idf.py flash
```

__Note__   
For some reason, there are development boards that cannot use GPIO06, GPIO08, GPIO09, GPIO19 for SPI clock pins.   
According to the ESP32C3 specifications, these pins can also be used as SPI clocks.   
I used a raw ESP-C3-13 to verify that these pins could be used as SPI clocks.   

# Configuration   
You have to set this config value with menuconfig.   
- CONFIG_WIDTH   
- CONFIG_HEIGHT   
- CONFIG_OFFSETX   
- CONFIG_OFFSETY   
- CONFIG_MOSI_GPIO   
- CONFIG_SCLK_GPIO   
- CONFIG_CS_GPIO   
- CONFIG_DC_GPIO   
- CONFIG_RESET_GPIO   
- CONFIG_BL_GPIO   

![ssd1283-config](https://user-images.githubusercontent.com/6020549/126050521-5f825f31-4719-4bc8-99e9-d0ab8b08939e.jpg)

# Graphic support
![graph-2](https://user-images.githubusercontent.com/6020549/126050616-fa5d6bd2-6376-4685-be01-160cdf15da96.JPG)
![graph-3](https://user-images.githubusercontent.com/6020549/126050618-64318c90-8b24-4b03-a61b-8e7287f540de.JPG)
![graph-4](https://user-images.githubusercontent.com/6020549/126050619-776b7c05-42d0-4b1f-a690-bc653e3adde0.JPG)
![graph-5](https://user-images.githubusercontent.com/6020549/126050620-233cd1a3-77f3-48c4-a419-e975b2a06e33.JPG)
![graph-6](https://user-images.githubusercontent.com/6020549/126050621-af6288fe-de7a-451c-b89a-f59c98b7980d.JPG)

# Fonts support
![font-1](https://user-images.githubusercontent.com/6020549/126050629-ab66cdb6-85dd-40f6-aaa2-613ef7ca6809.JPG)

It's possible to text rotation and invert.   
![font-2](https://user-images.githubusercontent.com/6020549/126050631-5a3001c8-b624-4efa-bb4d-9bfcffae601d.JPG)
![font-3](https://user-images.githubusercontent.com/6020549/126050632-e680d006-3b8c-4ce4-8388-4531cbf7ed28.JPG)
![font-4](https://user-images.githubusercontent.com/6020549/126050633-b45c6da6-bc25-4f7c-8224-7cba32398fa2.JPG)

It's possible to indicate more than one font at the same time.   
![font-5](https://user-images.githubusercontent.com/6020549/126050628-da49513d-170c-4e9f-80c8-6b60e425748f.JPG)

# Image support
BMP file   
![image-bmp](https://user-images.githubusercontent.com/6020549/126050669-55fbd893-dd51-46aa-9cb1-a6a6bd80fbaa.JPG)
The image file is quoted from [here](https://hombre-nuevo.com/microcomputer/esp320001/).

JPEG file(ESP32 only)   
![image-jpeg](https://user-images.githubusercontent.com/6020549/126050672-e3cf5f51-6a5f-43f4-80e9-2f6652eddab8.JPG)

PNG file    
![image-png](https://user-images.githubusercontent.com/6020549/126050677-40bcb450-34b2-49a7-a597-c3a145e7ba5a.JPG)

# JPEG Decoder   
The ESP-IDF component includes Tiny JPEG Decompressor.   
The document of Tiny JPEG Decompressor is [here](http://elm-chan.org/fsw/tjpgd/00index.html).   
This can reduce the image to 1/2 1/4 1/8.   

# PNG Decoder   
The ESP-IDF component includes part of the miniz library, such as mz_crc32.   
But it doesn't support all of the miniz.   
The document of miniz library is [here](https://github.com/richgel999/miniz).   

And I ported the pngle library from [here](https://github.com/kikuchan/pngle).   
This can reduce the image to any size.   

# Font File   
You can add your original fonts.   
The format of the font file is the FONTX format.   
Your font file is put in font directory.   
Your font file is uploaded to SPIFFS partition using meke flash.   

Please refer [this](http://elm-chan.org/docs/dosv/fontx_e.html) page about FONTX format.   

# Font File Editor(FONTX Editor)   
[There](http://elm-chan.org/fsw/fontxedit.zip) is a font file editor.   
This can be done on Windows 10.   
Developer page is [here](http://elm-chan.org/fsw_e.html).   

![FontxEditor](https://user-images.githubusercontent.com/6020549/78731275-3b889800-797a-11ea-81ba-096dbf07c4b8.png)

This library uses the following as default fonts:   
- font/ILGH16XB.FNT // 8x16Dot Gothic
- font/ILGH24XB.FNT // 12x24Dot Gothic
- font/ILGH32XB.FNT // 16x32Dot Gothic
- font/ILMH16XB.FNT // 8x16Dot Mincyo
- font/ILMH24XB.FNT // 12x24Dot Mincyo
- font/ILMH32XB.FNT // 16x32Dot Mincyo

From 0x00 to 0x7f, the characters image of Alphanumeric are stored.   
From 0x80 to 0xff, the characters image of Japanese are stored.   
Changing this file will change the font.

# How to build your own font file   
step1)   
download fontxedit.exe.   

step2)   
download BDF font file from Internet.   
I downloaded from [here](https://github.com/fcambus/spleen).   
fontxedit.exe can __ONLY__ import Monospaced bitmap fonts file.   
Monospaced bitmap fonts can also be downloaded [here](https://github.com/Tecate/bitmap-fonts).

step3)   
import the BDF font file into your fontxedit.exe.   
this tool can convert from BDF to FONTX.   
![FONTX-EDITTOR-1](https://user-images.githubusercontent.com/6020549/112736427-d7e5e900-8f95-11eb-80d5-11dd9df42903.jpg)

step4)   
adjust font size.   
![FONTX-EDITTOR-2](https://user-images.githubusercontent.com/6020549/112736434-e6cc9b80-8f95-11eb-8b8e-b523746c1c96.jpg)

step5)   
check font pattern.   
![FONTX-EDITTOR-13](https://user-images.githubusercontent.com/6020549/112746492-11e0da80-8fea-11eb-94f1-8d299b2dc756.jpg)

step6)   
save as .fnt file from your fontedit.exe.   
![FONTX-EDITTOR-14](https://user-images.githubusercontent.com/6020549/112746501-2329e700-8fea-11eb-9a3a-4481c1a14ddc.jpg)

step7)   
upload your font file to $HOME/esp-idf-st7789/fonts directory.   

step8)   
add font to use   
```
FontxFile fx32L[2];
InitFontx(fx32L,"/spiffs/LATIN32B.FNT",""); // 16x32Dot LATIN
```

Font file that From 0x80 to 0xff, the characters image of Japanese are stored.   
![font-6](https://user-images.githubusercontent.com/6020549/126050681-40eca790-5d2c-4fc1-a244-8af326ddf858.JPG)

Font file that From 0x80 to 0xff, the characters image of Latin are stored.   
![font-7](https://user-images.githubusercontent.com/6020549/126050684-09978a06-d33e-45ed-be01-0ce49712a2dc.JPG)
