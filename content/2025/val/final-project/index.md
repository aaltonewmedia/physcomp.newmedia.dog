---
title: Final Project
date: 2025-11-10T13:38:00.000+02:00
authors:
  - Valerie Jeyeon YONG
image: featured.jpg
showBgImage: false
---
# Final Project Ideas

## 1. Scentient Machine

#### Concept

Scentient Machine is a device that “reads the atmosphere” of a room. It interprets the invisible atmosphere of a space as emotional data.

Using MEMS gas sensors, the system reads the chemical composition of air, including human breath, perfume, humidity, and CO₂ levels, and translates these into emotional states.

ex) When the air feels heavy and warm, it displays: “The air smells tense.”

→ Tuto: [Building an Electronic Nose with MEMS Gas Detection Sensor](https://www.hackster.io/DFRobotOfficial/building-an-electronic-nose-with-mems-gas-detection-sensor-de5269) 

[](https://www.hackster.io/DFRobotOfficial/building-an-electronic-nose-with-mems-gas-detection-sensor-de5269)[](https://www.hackster.io/DFRobotOfficial/building-an-electronic-nose-with-mems-gas-detection-sensor-de5269)

#### Emotion Labels and Training Conditions Examples

![](table.png)

→ Edge Impulse model for smell/'feeling of the air'

#### Installation Design

Like the interface of an air purifier or weather application. 

Digital display showing live emotional status (ex. Tense – 82% Confidence)

![](ref.png)

References

[Adnose, *Adnan Aga*](https://adnanaga.com/Adnose) - predict the smell of any object using AI

![](nose.jpeg)

[Smeller 2.0, *Wolfgang Georgsdorf -*](https://smeller.net/about/) deliver complex sequences of smells played in place of music notes. 
→ Maybe I can treats scent as a medium and transforms invisible chemical data into a performative art piece

![](smeller.jpg)

## **2. Apology Jacket**

#### Inspiration

I’m interested in the kind of simple and humorous interactive works that Matti showed us, which echo the spirit of Chindogu, a playful useless invention inventions born from everyday inconveniences.

{{<youtube rKhbUjVyKIc>}}

#### Concept

This project originates from an experience that everyone has: in crowded places, I often bump into people’s shoulders but can’t possibly apologize to everyone.

*Apology Jacket* is a wearable device, embedded in the shoulder of a jacket, that automatically apologizes whenever it detects physical contact. 
→ using AI TTS ?

The project explores the automation of social etiquette and the absurd extension of AI assistance into even the smallest human gestures of politeness. We become more and more reliant on machines and AI, and now even to apologize. *Apology Jacket* exaggerates this dependency by outsourcing an intimate human behavior to an AI that reacts faster and more obediently than we ever could. The result is both comical and unsettling: an endlessly polite jacket, apologizing to the world.

#### How it works

When the wearer bumps into someone, the embedded sensor detects the collision’s intensity. According to the force, the AI system generates and plays an apology with different tone and repetition. From a calm “Sorry” to an anxious stream of “I’m so sorry! Sorry! Sorry!”.

![](sketch.jpg)

<hr>

\*After reviewing and organizing my ideas, I find the use of an electric nose interesting, but I’m not very satisfied with the concept itself. So for now, I’m considering either simply running the **electric nose experiment** or moving forward with the **Apology Jacket.***

<hr>

# Apology Jacket Working Progress

I decided to choose Apology Jacket because I’m more confident in it (and it received more reactions from my classmates).

**Sensors**

* velostat
* conductive fabric
* small audio player

## STEP 1 — Collision Detection

First Test with Pressure Sensor and Pico 2W

* Light collision = 400–700
* Strong collision = 800–1000

```cpp
int pressureValue;
int thresholdValue = 600;
int thresholdValue2 = 900;

bool hasApologized = false;

void setup() {
  Serial.begin(9600);
}

void loop() {

  // Read pressure sensor (FSR or force sensor)
  pressureValue = analogRead(26);    

  // Collision detected 
  if (pressureValue > thresholdValue) {

    if (!hasApologized) { // only say sorry once per bump

      if (pressureValue < thresholdValue2) {
        Serial.println("Sorry");
      } 
      else if (pressureValue >= thresholdValue2) {
        Serial.println("SORRY SORRY!");
      }

      hasApologized = true;  // prevent repeating
    }
  }

  // RESET when pressure goes back to normal 
  else {
    hasApologized = false;
  }

  // Print readings
  // Serial.print("Pressure: ");
  // Serial.println(pressureValue);

  delay(10);
}
```

![](test1.jpg)

## STEP 2 — Add Audio (AI text-to-speech)

Connecting to a speaker

![](sound.jpg)

**Add audio**

* Install BackgroundAudio Library 

<https://www.arduinolibraries.info/libraries/background-audio> <https://github.com/earlephilhower/BackgroundAudio> 

* Used WebradioMP3PlusWebUI example model which plays an MP3 web radio using HTTPS connectivity. Includes a serial and HTTP WebServer interface to allow the user to change URLs, volumes, and see the ICY metadata.

```cpp
// StreamMP3 - Earle F. Philhower, III <earlephilhower@yahoo.com>
// Released to the public domain, December 2024
//
// Connects to an HTTPS streaming MP3 radio station and decodes to PWMAudio.
// Connect earphone L/R to GPIO 0/1 and GND and configure your SSID and password.
// Provides basic Serial text and http web based control

#include <WiFi.h>
#include <HTTPClient.h>
#include <BackgroundAudio.h>
#include <WebServer.h>

#ifndef STASSID
#define STASSID "ssid"
#define STAPSK "password"
#endif

const char *ssid = "aalto open";
const char *pass = "";

#ifdef ESP32
#include <ESP32I2SAudio.h>
ESP32I2SAudio audio(4, 5, 6); // BCLK, LRCLK, DOUT (, MCLK)
// Or for 1-pin PDM with external LPF
//#include <ESP32PDMAudio.h>
//ESP32PDMAudio audio(5);
#else
#include <I2S.h>
#include <PWMAudio.h>
// Uncomment either the PWM or I2S version, being sure to adjust the PWM pin or (BCLK,DATA) pins.
//I2S audio(OUTPUT, 0, 2);
PWMAudio audio(0);
#endif

#ifdef ESP32
// The ESP32 devices seem to have very variable HTTP performance.  Increase the buffer here to sort-of compensate
#define STREAMBUFF (32 * 1024)
#else
// Pico and PicoW work well with much smaller compressed/raw buffer...
#define STREAMBUFF (16 * 1024)
#endif

// Instantiate a MP3 player with the specified raw (compressed) data buffer
BackgroundAudioMP3Class<RawDataBuffer<STREAMBUFF>> mp3(audio);

#ifdef ESP32
NetworkClientSecure client;
#else
WiFiClientSecure client;  // Because URL is HTTPS, need a WiFiSecureClient.  Plain HTTP can use WiFiClient
#endif

String url = "https://ice.audionow.com/485BBCWorld.mp3"; // Check out https://fmstream.org/index.php?c=FT for others
HTTPClient http;
uint8_t buff[512]; // HTTP reads into this before sending to MP3
WebServer web(80); // The HTTP interface for remote control

int icyMetaInt = 0;
int icyDataLeft = 0;
int icyMetadataLeft = 0;
int gain = 100;
String status;
String title;

// C++11 multiline string constants are neato...
static const char PAGE[] = R"KEWL(
<head>
<title>BackgroundAudio Web Radio</title>
<script type="text/javascript">
  function updateTitle() {
    var x = new XMLHttpRequest();
    x.open("GET", "title");
    x.onload = function() { document.getElementById("titlespan").innerHTML=x.responseText; setTimeout(updateTitle, 1000); }
    x.onerror = function() { setTimeout(updateTitle, 1000); }
    x.send();
  }
  setTimeout(updateTitle, 1000);
  function showValue(n) {
    document.getElementById("volspan").innerHTML=n;
    var x = new XMLHttpRequest();
    x.open("POST", "setvol");
    x.send(n);
  }
  function updateStatus() {var x = new XMLHttpRequest();
    x.open("GET", "status");
    x.onload = function() { document.getElementById("statusspan").innerHTML=x.responseText; setTimeout(updateStatus, 1000); }
    x.onerror = function() { setTimeout(updateStatus, 1000); }
    x.send();
  }
  setTimeout(updateStatus, 1000);
  function updateURL() {var x = new XMLHttpRequest();
    x.open("GET", "url");
    x.onload = function() { document.getElementById("urlspan").innerHTML=x.responseText; setTimeout(updateURL, 1000); }
    x.onerror = function() { setTimeout(updateURL, 1000); }
    x.send();
  }
  setTimeout(updateURL, 1000);
</script>
</head>

<body>
BackgroundAudio Web Radio!
<hr>
Currently Playing: <span id="titlespan">--</span><br>
Volume: <input type="range" name="vol" min="0" max="200" steps="10" value="100" onchange="showValue(this.value)"/> <span id="volspan">100</span>%
<hr>
Status: <span id="statusspan">--</span>
<hr>
<iframe name="dummy" id="dummy" style="display: none;"></iframe>
Current URL: <span id="urlspan">--</span><br>
<form action="seturl" method="POST" target="dummy">
Change URL: <input type="text" name="url">
<input type="submit" size="60" value="Change"></form>
</body>)KEWL";


void ConnectWiFi() {
#ifndef ESP32
  WiFi.end();
#endif
  Serial.print("Connecting to WiFi...");
  WiFi.begin(ssid);
  while (!WiFi.isConnected()) {
    Serial.print("..");
    delay(100);
  }
  Serial.print("http://");
  Serial.println(WiFi.localIP());
  web.begin();
}

void printHelp() {
  Serial.println("MP3 Streamer");
  Serial.println("+ = volume up");
  Serial.println("- = volume down");
  Serial.println("n<newurl> = change stream");
}

void handleVolume() {
  if ((web.method() != HTTP_POST) || !web.hasArg("plain")) {
    web.send(501, "text/plain", "Error");
  } else {
    gain = std::min(200, (int)web.arg("plain").toInt());
    gain = std::max(10, gain);
    mp3.setGain(gain / 100.0);
    web.send(200, "text/plain", "OK");
  }
}

void handleURL() {
  if ((web.method() != HTTP_POST) || !web.hasArg("url")) {
    web.send(501, "text/plain", "Error");
  } else {
    url = web.arg("url");
    http.end(); // Disconnect old, will connect on next loop()
    web.send(200, "text/plain", "OK");
  }
}

void setup() {
  Serial.begin(115200);
  delay(5000);
  printHelp();
  client.setInsecure(); // Don't worry about certs, just use encryption
  web.on("/", [] { web.send(200, "text/html", PAGE);});
  web.on("/status", [] { web.send(200, "text/plain", status.c_str());});
  web.on("/title", [] { web.send(200, "text/plain", title.c_str());});
  web.on("/url", [] { web.send(200, "text/plain", url.c_str());});
  web.on("/setvol", handleVolume);
  web.on("/seturl", handleURL);
  mp3.begin();
}


void loop() {
  static uint32_t last = 0;

  // Ensure WiFi is up.  If not, retry
  if (!WiFi.isConnected()) {
    ConnectWiFi();
    return;
  }

  web.handleClient();

  // If the HTTP stream drops, reconnect
  if (!http.connected()) {
    Serial.printf("(Re)connecting to '%s'...\n", url.c_str());
    http.end();
    http.begin(client, url);
    http.setReuse(true);
    http.setFollowRedirects(HTTPC_FORCE_FOLLOW_REDIRECTS);
    const char *icyHdrs[] = { "icy-metaint" }; // What is the MD interval?
    http.collectHeaders(icyHdrs, 1);
    http.addHeader("Icy-MetaData", "1");
    int code = http.GET();
    if (code != HTTP_CODE_OK) {
      http.end();
      Serial.printf("Can't GET: '%s'\n", url.c_str());
      delay(1000);
      return;
    }
    if (http.hasHeader("icy-metaint")) {
      icyMetaInt = http.header("icy-metaint").toInt();
      icyDataLeft = icyMetaInt;
    } else {
      icyMetaInt = 0;
    }
  }


  // Pump the MP3 player data.  Read what's available from the web and send to the MP3 object
  WiFiClient *stream = http.getStreamPtr();
  do {
    size_t httpavail = stream->available();
    httpavail = std::min(sizeof(buff), httpavail); // We can only read up to the buffer size
    size_t mp3avail = mp3.availableForWrite();
    if (!httpavail || !mp3avail) {
      break;
    }
    size_t toRead = std::min(mp3avail, httpavail); // Only read as much as we can send to MP3
    if (icyMetaInt) {
      toRead = std::min(toRead, (size_t)icyDataLeft);
    }
    int read = stream->read(buff, toRead);
    if (read < 0) {
      return; // Error in the read
    }
    mp3.write(buff, read);

    // If we drop too low, pause playback to let us catch up
    if (mp3.available() < 1024) {
      mp3.pause();
    } else if (mp3.paused() && mp3.available() > (STREAMBUFF / 2)) { // When paused wait until kind-of full before restarting
      mp3.unpause();
    }

    icyDataLeft -= read;
    if (icyMetaInt && !icyDataLeft) {
      while (!stream->available() && stream->connected()) {
        delay(1);
      }
      if (!stream->connected()) {
        return;
      }
      int totalCnt = stream->read() * 16;
      int cnt = totalCnt;

      // Read up to buff[] of metadata
      int buffCnt = std::min(sizeof(buff), (size_t)cnt); // The size of data to stuff in buff[]
      uint8_t *p = buff;
      while (buffCnt && stream->connected()) {
        read = stream->read(p, buffCnt);
        p += read;
        buffCnt -= read;
        cnt -= read;
      }

      // Throw out the rest
      while (cnt && stream->connected()) {
        stream->read(); // Throw out metadata larger than 512b buffer
        cnt--;
      }
      if (totalCnt) {
        buff[std::min(sizeof(buff) - 1, (size_t)totalCnt)] = 0;
        Serial.printf("md: '%s'\n", buff);
        char *titlestr = strstr((const char *)buff, "StreamTitle='");
        if (titlestr) {
          titlestr += 13; // Skip the StreamTitle=' bit
          // Really we should parse and check this isn't inside a ' section, but we're not that concerned right now
          char *end = strchr(titlestr, ';');
          if (end) {
            *(end - 1) = 0;
          }
          title = titlestr;
        }
      }
      icyDataLeft = icyMetaInt;
    }
  } while (true);

  // Can do UI processing, etc. at this point  Just be sure to run loop() often enough to get the 20-30KB/s of transfers needed for MP3 streaming
  if ((millis() - last) > 1000) {
    last = millis();
    sprintf((char *)buff, "buffer: %d, frames %lu, shifts %lu, underflows %lu, errors %lu, dumps %lu, uptime %lu", mp3.available(), mp3.frames(), mp3.shifts(), mp3.underflows(), mp3.errors(), mp3.dumps(), last);
    Serial.println((char*)buff);
    status = (char*)buff;
  }

  if (Serial.available()) {
    int chr = Serial.read();
    switch (chr) {
      case '\r': break;
      case '\n': break;
      case '?': printHelp(); break;
      case '+': gain = std::min(200, gain + 10); mp3.setGain(gain / 100.0); break;
      case '-': gain = std::max(10, gain - 10); mp3.setGain(gain / 100.0); break;
      case 'n': url = Serial.readString(); url.trim(); http.end(); break; // Will reconnect next pass
    }
  }
}
```

**Text-to-Speech**

* Used SerialSpeak example model which uses speech API to talk when I type to Serial Monitor. 

  → I changed to speak words that are inside the code and combined with the pressure sensors.

```cpp
// SerialSpeak - Earle F. Philhower, III <earlephilhower@yahoo.com>
// Released to the public domain January 2025

// Reads from the serial port and plays what's typed over the output
// asynchronously.  Can queue up work while still speaking.
// Demonstrates dictionary and voice usage

#include <BackgroundAudioSpeech.h>

// Choose the voice you want
#include <libespeak-ng/voice/en_029.h>
#include <libespeak-ng/voice/en_gb_scotland.h>
#include <libespeak-ng/voice/en_gb_x_gbclan.h>
#include <libespeak-ng/voice/en_gb_x_gbcwmd.h>
#include <libespeak-ng/voice/en_gb_x_rp.h>
#include <libespeak-ng/voice/en.h>
#include <libespeak-ng/voice/en_shaw.h>
#include <libespeak-ng/voice/en_us.h>
#include <libespeak-ng/voice/en_us_nyc.h>
BackgroundAudioVoice v[] = {
  voice_en_029, // 0
  voice_en_gb_scotland, //1
  voice_en_gb_x_gbclan, //2
  voice_en_gb_x_gbcwmd, //3
  voice_en, //4
  voice_en_shaw, //5
  voice_en_us, //6
  voice_en_us_nyc //7
};

#include <PWMAudio.h>
PWMAudio audio(0);
BackgroundAudioSpeech BMP(audio);

int pressureValue;
int thresholdValue = 600;
int thresholdValue2 = 900;

bool hasApologized = false;

void setup() {
  Serial.begin(115200);

  // We need to set up a voice before any output
  BMP.setVoice(v[4]);
  BMP.begin();

  delay(10);

  BMP.speak("Hello. I am your Apology Jacket.");
  delay(2000);
}

void loop() {
   // Read pressure sensor (FSR or force sensor)
  pressureValue = analogRead(26);    

  // Collision detected 
  if (pressureValue > thresholdValue) {

    if (!hasApologized) { // only say sorry once per bump

      if (pressureValue < thresholdValue2) {
        BMP.speak("Sorry");
      } 
      else if (pressureValue >= thresholdValue2) {
        BMP.speak("Sorry Sorry Sorry");
      }

      hasApologized = true;  // prevent repeating
    }
  }

  // RESET when pressure goes back to normal 
  else {
    hasApologized = false;
  }

  // Print readings
  // Serial.print("Pressure: ");
  // Serial.println(pressureValue);

  delay(10);
}
```

### STEP 3 — AI Apology Generation

* Run ChatGPT with openai API

<https://www.hackster.io/Shilleh/how-to-set-up-chatgpt-on-a-raspberry-pi-pico-w-5977bf> 

<https://www.youtube.com/watch?v=EAwh4ul-K0g>  

```
#include <WiFi.h>
#include <WiFiClientSecure.h>

const char* ssid = "aalto open";
const char* password = "";

const char* apiKey = "my keycode";

// New 2025 API endpoint
const char* host = "api.openai.com";
const int httpsPort = 443;

// Secure client
WiFiClientSecure client;

void setup() {
  Serial.begin(115200);
  delay(1000);

  Serial.println("Connecting to WiFi...");
  WiFi.begin(ssid);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nConnected!");

  // Required for SSL
  client.setInsecure();

  sendPrompt("Give one short, apology sentence to someone who bumped into your shoulder. No introduction, no explanation, no quotes. Output only the sentence.");
}

void loop() {
  // Nothing
}

void sendPrompt(String prompt) {
  Serial.println("\nConnecting to OpenAI...");

  if (!client.connect(host, httpsPort)) {
    Serial.println("Connection failed!");
    return;
  }

  Serial.println("Connected to OpenAI!");

  // JSON for new "responses" endpoint
  String requestBody = "{";
  requestBody += "\"model\": \"gpt-4.1-mini\",";
  requestBody += "\"input\": \"" + prompt + "\"";
  requestBody += "}";

  // Construct HTTP request
  String request = String("POST /v1/responses HTTP/1.1\r\n") + "Host: api.openai.com\r\n" + "Content-Type: application/json\r\n" + "Authorization: Bearer " + String(apiKey) + "\r\n" + "Content-Length: " + requestBody.length() + "\r\n\r\n" + requestBody;

  client.print(request);

  Serial.println("Request sent. Waiting for response...\n");

  String response = "";

  // Wait for the full response
  unsigned long timeout = millis();
  while (millis() - timeout < 5000) {  // wait up to 5 seconds
    while (client.available()) {
      char c = client.read();
      response += c;
      timeout = millis();  // extend timeout while data is still coming
    }
  }

  // Print the whole response for debugging
  Serial.println("===== RAW RESPONSE =====");
  Serial.println(response);
  Serial.println("========================");

  // Extract "text": "...."
  int pos = response.indexOf("\"text\":");
  if (pos != -1) {
    int start = response.indexOf("\"", pos + 7) + 1;
    int end = response.indexOf("\"", start);

    if (start > 0 && end > start) {
      String text = response.substring(start, end);

      Serial.println("\n===== AI MESSAGE =====");
      Serial.println(text);
      Serial.println("======================\n");
    } else {
      Serial.println("ERROR: Could not extract text.");
    }
  } else {
    Serial.println("ERROR: No 'text' field found.");
  }
}
```

![](ai-result.png)

Prompt: *Give one short, apology sentence to someone who bumped into your shoulder. No introduction, no explanation, no quotes. Output only the sentence.*

**Problem:** It takes 6 secs to run. (too long)

**Solution:** During this 6 sec → I’m calculating blablabla

[](https://www.hackster.io/giung-kim/how-to-use-openai-api-with-wizfi360-evb-pico-in-arduino-d10d5d)[](https://www.hackster.io/Shilleh/how-to-set-up-chatgpt-on-a-raspberry-pi-pico-w-5977bf)[](https://www.hackster.io/Shilleh/how-to-set-up-chatgpt-on-a-raspberry-pi-pico-w-5977bf)
