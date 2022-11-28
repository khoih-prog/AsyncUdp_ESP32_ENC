# AsyncUdp_ESP32_ENC


[![arduino-library-badge](https://www.ardu-badge.com/badge/AsyncUdp_ESP32_ENC.svg?)](https://www.ardu-badge.com/AsyncUdp_ESP32_ENC)
[![GitHub release](https://img.shields.io/github/release/khoih-prog/AsyncUdp_ESP32_ENC.svg)](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC/releases)
[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](#Contributing)
[![GitHub issues](https://img.shields.io/github/issues/khoih-prog/AsyncUdp_ESP32_ENC.svg)](http://github.com/khoih-prog/AsyncUdp_ESP32_ENC/issues)


<a href="https://www.buymeacoffee.com/khoihprog6" title="Donate to my libraries using BuyMeACoffee"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Donate to my libraries using BuyMeACoffee" style="height: 50px !important;width: 181px !important;" ></a>
<a href="https://www.buymeacoffee.com/khoihprog6" title="Donate to my libraries using BuyMeACoffee"><img src="https://img.shields.io/badge/buy%20me%20a%20coffee-donate-orange.svg?logo=buy-me-a-coffee&logoColor=FFDD00" style="height: 20px !important;width: 200px !important;" ></a>
<a href="https://profile-counter.glitch.me/khoih-prog/count.svg" title="Total khoih-prog Visitor count"><img src="https://profile-counter.glitch.me/khoih-prog/count.svg" style="height: 30px;width: 200px;"></a>
<a href="https://profile-counter.glitch.me/khoih-prog-AsyncUdp_ESP32_ENC/count.svg" title="Visitor count"><img src="https://profile-counter.glitch.me/khoih-prog-AsyncUdp_ESP32_ENC/count.svg" style="height: 30px;width: 200px;"></a>

---
---

## Table of Contents

* [Why do we need this AsyncUdp_ESP32_ENC library](#why-do-we-need-this-AsyncUdp_ESP32_ENC-library)
  * [Features](#features)
  * [Why Async is better](#why-async-is-better)
  * [Currently supported Boards](#currently-supported-boards)
* [Changelog](changelog.md)
* [Prerequisites](#prerequisites)
* [Installation](#installation)
  * [Use Arduino Library Manager](#use-arduino-library-manager)
  * [Manual Install](#manual-install)
  * [VS Code & PlatformIO](#vs-code--platformio)
* [Libraries' Patches](#libraries-patches)
  * [1. For fixing ESP32 compile error](#1-for-fixing-esp32-compile-error)
* [HOWTO Fix `Multiple Definitions` Linker Error](#howto-fix-multiple-definitions-linker-error) 
* [HOWTO Use analogRead() with ESP32 running WiFi and/or BlueTooth (BT/BLE)](#howto-use-analogread-with-esp32-running-wifi-andor-bluetooth-btble)
  * [1. ESP32 has 2 ADCs, named ADC1 and ADC2](#1--esp32-has-2-adcs-named-adc1-and-adc2)
  * [2. ESP32 ADCs functions](#2-esp32-adcs-functions)
  * [3. ESP32 WiFi uses ADC2 for WiFi functions](#3-esp32-wifi-uses-adc2-for-wifi-functions)
* [HOWTO Setting up the Async UDP Client](#howto-setting-up-the-async-udp-client)
* [Examples](#examples)
  * [ 1. AsyncUDPClient](examples/AsyncUDPClient)
  * [ 2. AsyncUdpNTPClient](examples/AsyncUdpNTPClient)
  * [ 3. AsyncUdpSendReceive](examples/AsyncUdpSendReceive)
  * [ 4. AsyncUDPServer](examples/AsyncUDPServer)
  * [ 5. AsyncUDPMulticastServer](examples/AsyncUDPMulticastServer)
  * [ 6. **multiFileProject**](examples/multiFileProject) **New**
* [Example AsyncUdpNTPClient](#example-asyncudpntpclient)
  * [File AsyncUdpNTPClient.ino](#file-asyncudpntpclientino)
* [Debug Terminal Output Samples](#debug-terminal-output-samples)
  * [1. AsyncUdpNTPClient on ESP32_DEV with ESP32_ENC28J60](#1-asyncudpntpclient-on-esp32_dev-with-ESP32_ENC28J60)
  * [2. AsyncUDPSendReceive on ESP32_DEV with ESP32_ENC28J60](#2-asyncudpsendreceive-on-esp32_dev-with-ESP32_ENC28J60)
* [Debug](#debug)
* [Troubleshooting](#troubleshooting)
* [Issues](#issues)
* [TO DO](#to-do)
* [DONE](#done)
* [Contributions and Thanks](#contributions-and-thanks)
* [Contributing](#contributing)
* [License](#license)
* [Copyright](#copyright)


---
---

### Why do we need this [AsyncUdp_ESP32_ENC library](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC)

#### Features

This [AsyncUdp_ESP32_ENC library](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC) is a fully asynchronous UDP library, designed for a trouble-free, multi-connection network environment, for ESP32 boards using `LwIP` ENC28J60 Ethernet. The library is easy to use and includes support for Unicast, Broadcast and Multicast environments.

This library is based on, modified from:

1. [Hristo Gochkov's AsyncUDP](https://github.com/espressif/arduino-esp32/tree/master/libraries/AsyncUDP)

to apply the better and faster **asynchronous** feature of the **powerful** [AsyncUDP](https://github.com/espressif/arduino-esp32/tree/master/libraries/AsyncUDP) into ESP32 boards using `LwIP` ENC28J60 Ethernet.


#### Why Async is better

- Using asynchronous network means that you can handle **more than one connection at the same time**
- You are called once the request is ready and parsed
- When you send the response, you are **immediately ready** to handle other connections while the server is taking care of sending the response in the background
- **Speed is OMG**
- After connecting to a UDP server as an Async Client, you are **immediately ready** to handle other connections while the Client is taking care of receiving the UDP responding packets in the background.
- You are not required to check in a tight loop() the arrival of the UDP responding packets to process them.


#### Currently supported Boards

1. **ESP32_ENC boards** using ESP32 boards with `LwIP` ENC28J60 Ethernet

---
---


## Prerequisites

 1. [`Arduino IDE 1.8.19+` for Arduino](https://www.arduino.cc/en/Main/Software)
 2. [`ESP32 Core 2.0.5+`](https://github.com/espressif/arduino-esp32) for ESP32-based boards. [![Latest release](https://img.shields.io/github/release/espressif/arduino-esp32.svg)](https://github.com/espressif/arduino-esp32/releases/latest/)
 
 3. [`WebServer_ESP32_ENC library 1.5.1+`](https://github.com/khoih-prog/WebServer_ESP32_ENC). To install, check [![arduino-library-badge](https://www.ardu-badge.com/badge/WebServer_ESP32_ENC.svg?)](https://www.ardu-badge.com/WebServer_ESP32_ENC)
 
---
---

### Installation

The suggested way to install is to:

#### Use Arduino Library Manager

The best way is to use `Arduino Library Manager`. Search for `AsyncUdp_ESP32_ENC`, then select / install the latest version. You can also use this link [![arduino-library-badge](https://www.ardu-badge.com/badge/AsyncUdp_ESP32_ENC.svg?)](https://www.ardu-badge.com/AsyncUdp_ESP32_ENC) for more detailed instructions.

### Manual Install

1. Navigate to [AsyncUdp_ESP32_ENC](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC) page.
2. Download the latest release `AsyncUdp_ESP32_ENC-main.zip`.
3. Extract the zip file to `AsyncUdp_ESP32_ENC-main` directory 
4. Copy the whole `AsyncUdp_ESP32_ENC-main` folder to Arduino libraries' directory such as `~/Arduino/libraries/`.

### VS Code & PlatformIO:

1. Install [VS Code](https://code.visualstudio.com/)
2. Install [PlatformIO](https://platformio.org/platformio-ide)
3. Install [**AsyncUdp_ESP32_ENC** library](https://registry.platformio.org/libraries/AsyncUdp_ESP32_ENC) by using [Library Manager](https://registry.platformio.org/libraries/AsyncUdp_ESP32_ENC/installation). Search for AsyncUdp_ESP32_ENC in [Platform.io Author's Libraries](https://platformio.org/lib/search?query=author:%22Khoi%20Hoang%22)
4. Use included [platformio.ini](platformio/platformio.ini) file from examples to ensure that all dependent libraries will installed automatically. Please visit documentation for the other options and examples at [Project Configuration File](https://docs.platformio.org/page/projectconf.html)

---
---


### Libraries' Patches

#### 1. For fixing ESP32 compile error

To fix [`ESP32 compile error`](https://github.com/espressif/arduino-esp32), just copy the following file into the [`ESP32`](https://github.com/espressif/arduino-esp32) cores/esp32 directory (e.g. ./arduino-1.8.19/hardware/espressif/cores/esp32) to overwrite the old file:
- [Server.h](LibraryPatches/esp32/cores/esp32/Server.h)



---
---


### HOWTO Fix `Multiple Definitions` Linker Error

The current library implementation, using `xyz-Impl.h` instead of standard `xyz.cpp`, possibly creates certain `Multiple Definitions` Linker error in certain use cases.

You can include this `.hpp` file

```cpp
// Can be included as many times as necessary, without `Multiple Definitions` Linker Error
#include "AsyncUdp_ESP32_ENC.hpp"     //https://github.com/khoih-prog/AsyncUdp_ESP32_ENC
```

in many files. But be sure to use the following `.h` file **in just 1 `.h`, `.cpp` or `.ino` file**, which must **not be included in any other file**, to avoid `Multiple Definitions` Linker Error

```cpp
// To be included only in main(), .ino with setup() to avoid `Multiple Definitions` Linker Error
#include "AsyncUdp_ESP32_ENC.h"       //https://github.com/khoih-prog/AsyncUdp_ESP32_ENC
```

Check the new [**multiFileProject** example](examples/multiFileProject) for a `HOWTO` demo.



---
---

### HOWTO Use analogRead() with ESP32 running WiFi and/or BlueTooth (BT/BLE)

Please have a look at [**ESP_WiFiManager Issue 39: Not able to read analog port when using the autoconnect example**](https://github.com/khoih-prog/ESP_WiFiManager/issues/39) to have more detailed description and solution of the issue.

#### 1.  ESP32 has 2 ADCs, named ADC1 and ADC2

#### 2. ESP32 ADCs functions

- ADC1 controls ADC function for pins **GPIO32-GPIO39**
- ADC2 controls ADC function for pins **GPIO0, 2, 4, 12-15, 25-27**

#### 3.. ESP32 WiFi uses ADC2 for WiFi functions

Look in file [**adc_common.c**](https://github.com/espressif/esp-idf/blob/master/components/driver/adc_common.c#L61)

> In ADC2, there're two locks used for different cases:
> 1. lock shared with app and Wi-Fi:
>    ESP32:
>         When Wi-Fi using the ADC2, we assume it will never stop, so app checks the lock and returns immediately if failed.
>    ESP32S2:
>         The controller's control over the ADC is determined by the arbiter. There is no need to control by lock.
> 
> 2. lock shared between tasks:
>    when several tasks sharing the ADC2, we want to guarantee
>    all the requests will be handled.
>    Since conversions are short (about 31us), app returns the lock very soon,
>    we use a spinlock to stand there waiting to do conversions one by one.
> 
> adc2_spinlock should be acquired first, then adc2_wifi_lock or rtc_spinlock.


- In order to use ADC2 for other functions, we have to **acquire complicated firmware locks and very difficult to do**
- So, it's not advisable to use ADC2 with WiFi/BlueTooth (BT/BLE).
- Use ADC1, and pins GPIO32-GPIO39
- If somehow it's a must to use those pins serviced by ADC2 (**GPIO0, 2, 4, 12, 13, 14, 15, 25, 26 and 27**), use the **fix mentioned at the end** of [**ESP_WiFiManager Issue 39: Not able to read analog port when using the autoconnect example**](https://github.com/khoih-prog/ESP_WiFiManager/issues/39) to work with ESP32 WiFi/BlueTooth (BT/BLE).


---
---

## HOWTO Setting up the Async UDP Client

```cpp
#if !( defined(ESP32) )
  #error This code is designed for (ESP32 + ENC28J60) to run on ESP32 platform! Please check your Tools->Board setting.
#endif

#include <Arduino.h>

#define ASYNC_UDP_ESP32_ENC_DEBUG_PORT      Serial

// Use from 0 to 4. Higher number, more debugging messages and memory usage.
#define _ASYNC_UDP_ESP32_ENC_LOGLEVEL_      1

//////////////////////////////////////////////////////////

// Optional values to override default settings
//#define SPI_HOST            1
//#define SPI_CLOCK_MHZ       8

// Must connect INT to GPIOxx or not working
//#define INT_GPIO            4

//#define MISO_GPIO           19
//#define MOSI_GPIO           23
//#define SCK_GPIO            18
//#define CS_GPIO             5

//////////////////////////////////////////////////////////

#include <AsyncUDP_ESP32_ENC.h>

/////////////////////////////////////////////

// Enter a MAC address and IP address for your controller below.
#define NUMBER_OF_MAC      20

byte mac[][NUMBER_OF_MAC] =
{
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x01 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x02 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x03 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x04 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x05 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x06 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x07 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x08 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x09 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x0A },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x0B },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x0C },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x0D },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x0E },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x0F },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x10 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x11 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x12 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0x13 },
  { 0xDE, 0xAD, 0xBE, 0xEF, 0xBE, 0x14 },
};

// Select the IP address according to your local network
IPAddress myIP(192, 168, 2, 232);
IPAddress myGW(192, 168, 2, 1);
IPAddress mySN(255, 255, 255, 0);

// Google DNS Server IP
IPAddress myDNS(8, 8, 8, 8);

/////////////////////////////////////////////

#include <time.h>

// 0.ca.pool.ntp.org
IPAddress timeServerIP = IPAddress(208, 81, 1, 244);
// time.nist.gov
//IPAddress timeServerIP = IPAddress(132, 163, 96, 1);

#define NTP_REQUEST_PORT      123

char ReplyBuffer[] = "ACK";      // a string to send back

char timeServer[]         = "time.nist.gov";  // NTP server

const int NTP_PACKET_SIZE = 48;       // NTP timestamp is in the first 48 bytes of the message

byte packetBuffer[NTP_PACKET_SIZE];   // buffer to hold incoming and outgoing packets

// A UDP instance to let us send and receive packets over UDP
AsyncUDP Udp;

void sendACKPacket(void)
{
  Serial.println("============= sendACKPacket =============");
  
  // Send unicast ACK to the same remoteIP and remotePort we received the packet
  // The AsyncUDP_STM32 library will take care of the correct IP and port based on pcb
  Udp.write((uint8_t *) ReplyBuffer, sizeof(ReplyBuffer));
}

// send an NTP request to the time server at the given address
void createNTPpacket(void)
{
  Serial.println("============= createNTPpacket =============");

  // set all bytes in the buffer to 0
  memset(packetBuffer, 0, NTP_PACKET_SIZE);
  // Initialize values needed to form NTP request
  // (see URL above for details on the packets)

  packetBuffer[0]   = 0b11100011;   // LI, Version, Mode
  packetBuffer[1]   = 0;     // Stratum, or type of clock
  packetBuffer[2]   = 6;     // Polling Interval
  packetBuffer[3]   = 0xEC;  // Peer Clock Precision
  
  // 8 bytes of zero for Root Delay & Root Dispersion
  packetBuffer[12]  = 49;
  packetBuffer[13]  = 0x4E;
  packetBuffer[14]  = 49;
  packetBuffer[15]  = 52;
}

void sendNTPPacket(void)
{
  createNTPpacket();
  //Send unicast
  Udp.write(packetBuffer, sizeof(packetBuffer));
}

void parsePacket(AsyncUDPPacket packet)
{
  struct tm  ts;
  char       buf[80];
  
  memcpy(packetBuffer, packet.data(), sizeof(packetBuffer));

  Serial.print("Received UDP Packet Type: ");
  Serial.println(packet.isBroadcast() ? "Broadcast" : packet.isMulticast() ? "Multicast" : "Unicast");
  Serial.print("From: ");
  Serial.print(packet.remoteIP());
  Serial.print(":");
  Serial.print(packet.remotePort());
  Serial.print(", To: ");
  Serial.print(packet.localIP());
  Serial.print(":");
  Serial.print(packet.localPort());
  Serial.print(", Length: ");
  Serial.print(packet.length());
  Serial.println();

  unsigned long highWord = word(packetBuffer[40], packetBuffer[41]);
  unsigned long lowWord = word(packetBuffer[42], packetBuffer[43]);

  // combine the four bytes (two words) into a long integer
  // this is NTP time (seconds since Jan 1 1900):
  unsigned long secsSince1900 = highWord << 16 | lowWord;
  
  Serial.print(F("Seconds since Jan 1 1900 = "));
  Serial.println(secsSince1900);

  // now convert NTP time into )everyday time:
  Serial.print(F("Epoch/Unix time = "));
  
  // Unix time starts on Jan 1 1970. In seconds, that's 2208988800:
  const unsigned long seventyYears = 2208988800UL;
  
  // subtract seventy years:
  unsigned long epoch = secsSince1900 - seventyYears;
  time_t epoch_t = epoch;   //secsSince1900 - seventyYears;
 
  // print Unix time:
  Serial.println(epoch);

  // print the hour, minute and second:
  Serial.print(F("The UTC/GMT time is "));       // UTC is the time at Greenwich Meridian (GMT)

  ts = *localtime(&epoch_t);
  strftime(buf, sizeof(buf), "%a %Y-%m-%d %H:%M:%S %Z", &ts);
  Serial.println(buf);

  // send a reply, to the IP address and port that sent us the packet we received
  sendACKPacket();
}

void setup()
{
  Serial.begin(115200);
  while (!Serial && (millis() < 5000));

  Serial.print(F("\nStart AsyncUDPSendReceive on "));
  Serial.print(ARDUINO_BOARD);
  Serial.print(F(" with "));
  Serial.println(SHIELD_TYPE);
  Serial.println(WEBSERVER_ESP32_ENC_VERSION);
  Serial.println(ASYNC_UDP_ESP32_ENC_VERSION);

  Serial.setDebugOutput(true);

  ET_LOGWARN(F("Default SPI pinout:"));
  ET_LOGWARN1(F("MOSI:"), MOSI_GPIO);
  ET_LOGWARN1(F("MISO:"), MISO_GPIO);
  ET_LOGWARN1(F("SCK:"),  SCK_GPIO);
  ET_LOGWARN1(F("CS:"),   CS_GPIO);
  ET_LOGWARN1(F("INT:"),  INT_GPIO);
  ET_LOGWARN1(F("SPI Clock (MHz):"), SPI_CLOCK_MHZ);
  ET_LOGWARN(F("========================="));

  ///////////////////////////////////

  // To be called before ETH.begin()
  ESP32_ENC_onEvent();

  // start the ethernet connection and the server:
  // Use DHCP dynamic IP and random mac
  uint16_t index = millis() % NUMBER_OF_MAC;

  //bool begin(int MISO_GPIO, int MOSI_GPIO, int SCLK_GPIO, int CS_GPIO, int INT_GPIO, int SPI_CLOCK_MHZ,
  //           int SPI_HOST, uint8_t *ENC28J60_Mac = ENC28J60_Default_Mac);
  //ETH.begin( MISO_GPIO, MOSI_GPIO, SCK_GPIO, CS_GPIO, INT_GPIO, SPI_CLOCK_MHZ, SPI_HOST );
  ETH.begin( MISO_GPIO, MOSI_GPIO, SCK_GPIO, CS_GPIO, INT_GPIO, SPI_CLOCK_MHZ, SPI_HOST, mac[index] );

  // Static IP, leave without this line to get IP via DHCP
  //bool config(IPAddress local_ip, IPAddress gateway, IPAddress subnet, IPAddress dns1 = 0, IPAddress dns2 = 0);
  //ETH.config(myIP, myGW, mySN, myDNS);

  ESP32_ENC_waitForConnect();

  ///////////////////////////////////

  // Client address
  Serial.print("AsyncUDPSendReceive started @ IP address: ");
  Serial.println(ETH.localIP());

  Serial.println(F("\nStarting connection to server..."));

  //NTP requests are to port NTP_REQUEST_PORT = 123
  if (Udp.connect(timeServerIP, NTP_REQUEST_PORT))
  {
    Serial.println("UDP connected");

    Udp.onPacket([](AsyncUDPPacket packet)
    {
      parsePacket(packet);
    });
  }
}

void loop()
{
  sendNTPPacket();

  // wait 60 seconds before asking for the time again
  delay(60000);
}
```

---
---

### Examples

 1. [AsyncUDPClient](examples/AsyncUDPClient)
 2. [AsyncUdpNTPClient](examples/AsyncUdpNTPClient)
 3. [AsyncUdpSendReceive](examples/AsyncUdpSendReceive) 
 4. [AsyncUDPServer](examples/AsyncUDPServer)
 5. [AsyncUDPMulticastServer](examples/AsyncUDPMulticastServer)
 6. [**multiFileProject**](examples/multiFileProject)
---

### Example [AsyncUdpNTPClient](examples/AsyncUdpNTPClient)

#### File [AsyncUdpNTPClient.ino](examples/AsyncUdpNTPClient/AsyncUdpNTPClient.ino)

https://github.com/khoih-prog/AsyncUdp_ESP32_ENC/blob/6647e6ad4a3e3ac996301e5a415687fd227aa893/examples/AsyncUdpNTPClient/AsyncUdpNTPClient.ino#L11-L244

---

### Debug Terminal Output Samples

#### 1. AsyncUdpNTPClient on ESP32_DEV with ESP32_ENC28J60

This is terminal debug output when running [AsyncUdpNTPClient](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC/tree/main/examples/AsyncUdpNTPClient) on  **ESP32_ENC (ESP32 + ENC28J60)**. It connects to NTP Server using AsyncUdp_ESP32_ENC library, and requests NTP time every 60s. The packet is then **received and processed asynchronously** to print current UTC/GMT time.

##### Connect to NTP server 0.ca.pool.ntp.org (IP=208.81.1.244)

```cpp
Start AsyncUdpNTPClient on ESP32_DEV with ESP32_ENC28J60
WebServer_ESP32_ENC v1.5.1 for core v2.0.0+
AsyncUdp_ESP32_ENC v2.0.0 for core v2.0.0+

ETH Started
ETH Connected
ETH MAC: DE:AD:BE:EF:FE:09, IPv4: 192.168.2.94
FULL_DUPLEX, 10Mbps
AsyncUdpNTPClient started @ IP address: 192.168.2.94
UDP connected
============= createNTPpacket =============
Received UDP Packet Type: Unicast
From: 208.81.1.244:123, To: 192.168.2.94:60421, Length: 48
Seconds since Jan 1 1900 = 3878658567
Epoch/Unix time = 1669669767
The UTC/GMT time is Mon 2022-11-28 21:09:27 GMT
```


---

#### 2. AsyncUDPSendReceive on ESP32_DEV with ESP32_ENC28J60

This is terminal debug output when running [AsyncUDPSendReceive](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC/tree/main/examples/AsyncUdpSendReceive) on **ESP32_ENC (ESP32 + ENC28J60)**. It connects to NTP Server `0.ca.pool.ntp.org` (IP=208.81.1.244) using `AsyncUdp_ESP32_ENC` library, and requests NTP time every 60s. The packet is **received and processed asynchronously** to print current UTC/GMT time. The ACK packet is then sent.

```cpp
Start AsyncUDPSendReceive on ESP32_DEV with ESP32_ENC28J60
WebServer_ESP32_ENC v1.5.1 for core v2.0.0+
AsyncUdp_ESP32_ENC v2.0.0 for core v2.0.0+

ETH Started
ETH Connected
ETH MAC: DE:AD:BE:EF:FE:09, IPv4: 192.168.2.94
FULL_DUPLEX, 10Mbps
AsyncUDPSendReceive started @ IP address: 192.168.2.94

Starting connection to server...
UDP connected
============= createNTPpacket =============
Received UDP Packet Type: Unicast
From: 208.81.1.244:123, To: 192.168.2.94:55586, Length: 48
Seconds since Jan 1 1900 = 3878660103
Epoch/Unix time = 1669671303
The UTC/GMT time is Mon 2022-11-28 21:35:03 GMT
============= sendACKPacket =============
```

---
---

### Debug

Debug is enabled by default on Serial. To disable, use level 0

```cpp
#define ASYNC_UDP_ESP32_ENC_DEBUG_PORT      Serial

// Use from 0 to 4. Higher number, more debugging messages and memory usage.
#define _ASYNC_UDP_ESP32_ENC_LOGLEVEL_      1
```

You can also change the debugging level from 0 to 4

```cpp
#define ASYNC_UDP_ESP32_ENC_DEBUG_PORT      Serial

// Use from 0 to 4. Higher number, more debugging messages and memory usage.
#define _ASYNC_UDP_ESP32_ENC_LOGLEVEL_      4
```

---

### Troubleshooting

If you get compilation errors, more often than not, you may need to install a newer version of Arduino IDE, the Arduino `STM32` core or depending libraries.

Sometimes, the library will only work if you update the `STM32` core to the latest version because I am always using the latest cores /libraries.

---
---


### Issues ###

Submit issues to: [AsyncUdp_ESP32_ENC issues](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC/issues)

---

## TO DO

 1. Fix bug. Add enhancement


## DONE

 1. Initial port to to ESP32_ENC (ESP32 + ENC28J60)
 2. Add more examples.
 3. Add debugging features.
 4. Fix bug in ESP32_ENC examples to reduce connection time
 5. Fix `multiple-definitions` linker error.
 6. Add example [multiFileProject](examples/multiFileProject) to demo for multiple-file project
 
 
---
---

### Contributions and Thanks

1. Based on and modified from [Hristo Gochkov's AsyncUDP](https://github.com/espressif/arduino-esp32/tree/master/libraries/AsyncUDP). Many thanks to [Hristo Gochkov](https://github.com/me-no-dev) for great [AsyncUDP Library]((https://github.com/espressif/arduino-esp32/tree/master/libraries/AsyncUDP))
2. Thanks to [tobozo](https://github.com/tobozo) to make the library [ESP32-ENC28J60](https://github.com/tobozo/ESP32-ENC28J60) from which this library uses some ideas and codes

<table>
  <tr>
    <td align="center"><a href="https://github.com/me-no-dev"><img src="https://github.com/me-no-dev.png" width="100px;" alt="me-no-dev"/><br /><sub><b>⭐️⭐️ Hristo Gochkov</b></sub></a><br /></td>
    <td align="center"><a href="https://github.com/tobozo"><img src="https://github.com/tobozo.png" width="100px;" alt="tobozo"/><br /><sub><b>tobozo</b></sub></a><br /></td>
  </tr>
  </tr> 
</table>

---

### Contributing

If you want to contribute to this project:

- Report bugs and errors
- Ask for enhancements
- Create issues and pull requests
- Tell other people about this library

---

### License

- The library is licensed under [GPLv3](https://github.com/khoih-prog/AsyncUdp_ESP32_ENC/blob/main/LICENSE)

---

## Copyright

Copyright (c) 2018- Hristo Gochkov

Copyright (c) 2022- Khoi Hoang


