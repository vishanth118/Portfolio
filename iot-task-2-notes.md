# IoT Session — Task 2: Adafruit IO Dashboard & MQTT LED Control

## 1. Overview

### Objective

The objective of this task is to develop a cloud-connected IoT system using an **ESP32, MQTT protocol, and Adafruit IO**. The system allows me to control an LED remotely through an Adafruit IO dashboard.

### Problem Being Addressed

In the previous task, I controlled the LED through a webpage hosted by the ESP32. That method required the device and ESP32 to be connected to the same local network.

In this task, I extended the project by using **Adafruit IO as a cloud platform**. MQTT is used to send the ON and OFF commands between the Adafruit IO dashboard and the ESP32.

### Project Summary

The ESP32 connects to Wi-Fi and then connects to Adafruit IO. A feed named **light** is used for LED control.

The Adafruit IO dashboard contains an ON/OFF control connected to the `light` feed. When I select ON or OFF, Adafruit IO sends the value through MQTT. The ESP32 receives the message and changes the LED state.

### Basic Operation

```text
Adafruit IO Dashboard
          ↓
      MQTT / Cloud
          ↓
         ESP32
          ↓
       GPIO 2
          ↓
          LED
```

---

# 2. Concepts

## Adafruit IO

**Adafruit IO** is a cloud-based IoT platform that allows devices to send, receive, visualize, and control data over the Internet.

In my project, Adafruit IO is used to provide a remote dashboard for controlling the ESP32 LED.

The main features used in this project are:

- Feed
- Dashboard
- MQTT communication
- Cloud-based control
- ESP32 connectivity

## MQTT

**MQTT (Message Queuing Telemetry Transport)** is a lightweight messaging protocol commonly used for IoT applications.

MQTT uses a **publish/subscribe communication model**.

The main components are:

1. Publisher
2. Subscriber
3. Broker

## MQTT Publisher

The Adafruit IO dashboard acts as the publisher when I change the LED control value.

For example:

```text
ON
```

or

```text
OFF
```

is published to the `light` feed.

## MQTT Subscriber

The ESP32 acts as the subscriber.

It subscribes to the `light` feed and waits for commands from Adafruit IO.

## MQTT Broker

The Adafruit IO MQTT service acts as the broker. It receives the message from the dashboard and delivers it to the ESP32.

The communication can be represented as:

```text
Adafruit IO Dashboard
          ↓
       Publisher
          ↓
   Adafruit IO MQTT
       Service
          ↓
       MQTT Feed
          ↓
      Subscriber
          ↓
         ESP32
          ↓
          LED
```

## MQTT Feed

Adafruit IO uses feeds as communication channels.

The feed used in my project is:

```text
light
```

The dashboard publishes the LED command to this feed, and the ESP32 receives the command from the same feed.

---

# 3. System Design

## System Architecture

The complete system is:

```text
User
 ↓
Adafruit IO Dashboard
 ↓
Adafruit IO MQTT Service
 ↓
light Feed
 ↓
ESP32
 ↓
GPIO 2
 ↓
LED
```

## Data Flow

1. The ESP32 connects to Wi-Fi.
2. The ESP32 connects to Adafruit IO.
3. The ESP32 subscribes to the `light` feed.
4. I open the Adafruit IO dashboard.
5. I select ON or OFF.
6. Adafruit IO publishes the selected value.
7. The MQTT service forwards the message.
8. The ESP32 receives the message.
9. The ESP32 checks whether the value is ON or OFF.
10. GPIO 2 changes the LED state.

## Communication Diagram

```text
                    INTERNET
                       │
                       ▼
              ┌─────────────────┐
              │   Adafruit IO   │
              │    Dashboard    │
              └────────┬────────┘
                       │
                   MQTT Message
                       │
                       ▼
              ┌─────────────────┐
              │  Adafruit IO    │
              │  MQTT Service   │
              └────────┬────────┘
                       │
                    light Feed
                       │
                       ▼
              ┌─────────────────┐
              │      ESP32      │
              │ MQTT Subscriber │
              └────────┬────────┘
                       │
                    GPIO 2
                       │
                       ▼
                      LED
```

---

# 4. Hardware & Software

## Hardware Components

| Component | Purpose |
|---|---|
| ESP32 Development Board | Main IoT controller |
| Built-in LED / LED | Output controlled through Adafruit IO |
| USB Cable | Programming and powering the ESP32 |
| Wi-Fi Network / Hotspot | Internet connectivity |
| Computer / Mobile | Accessing the Adafruit IO dashboard |

For my current setup, I am controlling the LED directly from the ESP32, so an external relay or AC bulb is not required.

## Software and Platforms

| Software / Platform | Purpose |
|---|---|
| Arduino IDE | Programming and uploading the ESP32 code |
| ESP32 Board Package | ESP32 support in Arduino IDE |
| Adafruit IO Arduino Library | Communication with Adafruit IO |
| Adafruit IO | Cloud IoT platform |
| MQTT | IoT messaging protocol |
| Web Browser | Accessing the Adafruit IO dashboard |

---

# 5. Adafruit IO Account and Configuration

## Step 1 — Adafruit IO Account

I use an Adafruit IO account to provide the cloud service required for the project.

The account provides:

- Adafruit IO username
- AIO Key
- Feeds
- Dashboards
- MQTT communication

The username and AIO Key are entered into the ESP32 program.

## Step 2 — Create the Feed

I created an Adafruit IO feed named:

```text
light
```

This feed is used to send the LED control commands.

### Feed Values

```text
ON
OFF
```

The feed acts as the communication channel between the Adafruit IO dashboard and the ESP32.

---

# 6. Dashboard Configuration

I created an Adafruit IO dashboard to control the LED.

The dashboard control is connected to the:

```text
light
```

feed.

### Dashboard Operation

When I select:

```text
ON
```

the dashboard publishes:

```text
ON
```

to the `light` feed.

When I select:

```text
OFF
```

the dashboard publishes:

```text
OFF
```

to the `light` feed.

### Evidence

I can include screenshots showing:

- Adafruit IO account
- `light` feed
- Adafruit IO dashboard
- Dashboard with ON selected
- Dashboard with OFF selected

---

# 7. Wiring / Setup

For my setup, I use **GPIO 2** to control the LED.

### Pin Connection

| ESP32 Pin | Component | Function |
|---|---|---|
| GPIO 2 | LED / Built-in LED | Digital output |
| GND | LED circuit | Ground |

If the ESP32's built-in LED is used, no external LED wiring is required.

### Circuit Operation

```text
ESP32 GPIO 2
     │
     ▼
    LED
     │
    GND
```

When GPIO 2 is HIGH:

```text
LED ON
```

When GPIO 2 is LOW:

```text
LED OFF
```

---

# 8. MQTT Communication Flow

The complete communication process is:

```text
User
 │
 ▼
Adafruit IO Dashboard
 │
 │ Publish
 ▼
light Feed
 │
 ▼
Adafruit IO MQTT Service
 │
 │ MQTT
 ▼
ESP32
 │
 ▼
GPIO 2
 │
 ▼
LED
```

## Example — LED ON

```text
User selects ON
       ↓
Dashboard publishes "ON"
       ↓
light feed
       ↓
Adafruit IO MQTT service
       ↓
ESP32 receives "ON"
       ↓
GPIO 2 = HIGH
       ↓
LED ON
```

## Example — LED OFF

```text
User selects OFF
       ↓
Dashboard publishes "OFF"
       ↓
light feed
       ↓
Adafruit IO MQTT service
       ↓
ESP32 receives "OFF"
       ↓
GPIO 2 = LOW
       ↓
LED OFF
```

---

# 9. MQTT Feed Structure

The Adafruit IO MQTT feed is based on the account username and feed name.

The feed follows this structure:

```text
YOUR_ADAFRUIT_USERNAME/feeds/light
```

### MQTT Parameters

| Parameter | Description |
|---|---|
| MQTT Server | io.adafruit.com |
| Feed | light |
| Publisher | Adafruit IO Dashboard |
| Subscriber | ESP32 |
| Message | ON / OFF |
| Transport | Wi-Fi / Internet |

---

# 10. Implementation

## Program Logic

The ESP32 program follows these steps:

1. Define the Wi-Fi credentials.
2. Define the Adafruit IO username and AIO Key.
3. Create the Adafruit IO connection.
4. Define the `light` feed.
5. Set GPIO 2 as an output.
6. Connect the ESP32 to Wi-Fi.
7. Connect the ESP32 to Adafruit IO.
8. Subscribe to the `light` feed.
9. Wait for a message.
10. If the message is `ON`, turn the LED ON.
11. If the message is `OFF`, turn the LED OFF.

## Complete ESP32 Source Code

```cpp
#include "AdafruitIO_WiFi.h"

// ==============================
// Wi-Fi Details
// ==============================
#define WIFI_SSID "YOUR_WIFI_NAME"
#define WIFI_PASS "YOUR_WIFI_PASSWORD"

// ==============================
// Adafruit IO Details
// ==============================
#define IO_USERNAME "YOUR_ADAFRUIT_USERNAME"
#define IO_KEY      "YOUR_ADAFRUIT_IO_KEY"

// ==============================
// LED
// ==============================
#define LED_PIN 2

// Create Adafruit IO connection
AdafruitIO_WiFi io(IO_USERNAME, IO_KEY, WIFI_SSID, WIFI_PASS);

// Create the light feed
AdafruitIO_Feed *lightFeed = io.feed("light");

// ==============================
// Receive message from Adafruit IO
// ==============================
void handleMessage(AdafruitIO_Data *data) {

  String command = data->toString();

  Serial.print("Received: ");
  Serial.println(command);

  if (command == "ON") {

    digitalWrite(LED_PIN, HIGH);

    Serial.println("LED ON");
  }

  else if (command == "OFF") {

    digitalWrite(LED_PIN, LOW);

    Serial.println("LED OFF");
  }
}

// ==============================
// Setup
// ==============================
void setup() {

  Serial.begin(115200);

  pinMode(LED_PIN, OUTPUT);

  // LED OFF initially
  digitalWrite(LED_PIN, LOW);

  Serial.println();
  Serial.println("Connecting to Adafruit IO...");

  // Connect to Adafruit IO
  io.connect();

  // Wait for connection
  while (io.status() < AIO_CONNECTED) {

    Serial.print(".");
    delay(500);
  }

  Serial.println();
  Serial.println("Connected to Adafruit IO!");

  // Listen for changes in the light feed
  lightFeed->onMessage(handleMessage);

  // Get the current value
  lightFeed->get();
}

// ==============================
// Main Loop
// ==============================
void loop() {

  io.run();
}
```

---

# 11. Explanation of Important Code Sections

## Wi-Fi and Adafruit IO Configuration

```cpp
#define WIFI_SSID "YOUR_WIFI_NAME"
#define WIFI_PASS "YOUR_WIFI_PASSWORD"

#define IO_USERNAME "YOUR_ADAFRUIT_USERNAME"
#define IO_KEY "YOUR_ADAFRUIT_IO_KEY"
```

These values allow the ESP32 to connect to Wi-Fi and authenticate with Adafruit IO.

## LED Pin

```cpp
#define LED_PIN 2
```

GPIO 2 is used to control the LED.

## Adafruit IO Connection

```cpp
AdafruitIO_WiFi io(IO_USERNAME, IO_KEY, WIFI_SSID, WIFI_PASS);
```

This creates the connection between the ESP32, Wi-Fi, and Adafruit IO.

## Feed

```cpp
AdafruitIO_Feed *lightFeed = io.feed("light");
```

This defines the Adafruit IO feed used for controlling the LED.

## Connecting to Adafruit IO

```cpp
io.connect();

while (io.status() < AIO_CONNECTED) {
  Serial.print(".");
  delay(500);
}
```

The ESP32 waits until it successfully connects to Adafruit IO.

## Receiving MQTT Data

```cpp
lightFeed->onMessage(handleMessage);
```

This tells the ESP32 to run `handleMessage()` whenever a new value is received from the `light` feed.

## LED ON/OFF Control

```cpp
if (command == "ON") {
  digitalWrite(LED_PIN, HIGH);
}
```

The LED turns ON when the received value is `ON`.

```cpp
else if (command == "OFF") {
  digitalWrite(LED_PIN, LOW);
}
```

The LED turns OFF when the received value is `OFF`.

---

# 12. Evidence

## Hardware Setup

I can include photographs showing:

- ESP32 development board
- USB connection
- LED / built-in LED
- Complete hardware setup

## Adafruit IO Dashboard

I can include screenshots showing:

- Adafruit IO dashboard
- `light` feed
- ON control
- OFF control
- Feed activity/data

## Demonstration

The demonstration can show:

1. ESP32 powered ON.
2. ESP32 connected to Wi-Fi.
3. Adafruit IO dashboard opened.
4. `light` control selected.
5. ON command sent.
6. LED turns ON.
7. OFF command sent.
8. LED turns OFF.
9. Serial Monitor showing the received ON/OFF commands.

---

# 13. Challenges & Fixes

## Challenge 1 — Adafruit IO Connection Failure

**Problem:**  
The ESP32 could not connect to Adafruit IO.

**Solution:**  
I checked the Wi-Fi credentials, Adafruit IO username, AIO Key, and Internet connection.

## Challenge 2 — Incorrect Feed Name

**Problem:**  
The ESP32 was connected to Adafruit IO but did not respond to the dashboard command.

**Solution:**  
I checked that the feed name in the code matched the feed created in Adafruit IO.

The feed used in my project is:

```text
light
```

## Challenge 3 — Authentication Error

**Problem:**  
Adafruit IO did not accept the ESP32 connection.

**Solution:**  
I checked the Adafruit IO username and AIO Key entered in the program.

## Challenge 4 — LED Did Not Respond

**Problem:**  
The message was received but the LED did not change state.

**Solution:**  
I checked the GPIO 2 configuration and verified the received command using the Arduino Serial Monitor.

## Challenge 5 — Internet Connectivity

**Problem:**  
Adafruit IO communication requires Internet access.

**Solution:**  
I connected the ESP32 to a Wi-Fi network with Internet access before connecting to Adafruit IO.

---

# 14. Learning Reflection

This task helped me understand how an IoT device can communicate with a cloud platform using the **MQTT protocol**.

I learned how Adafruit IO can be used to create a cloud dashboard and how an ESP32 can receive commands from an Adafruit IO feed.

I also learned the basic roles of an MQTT publisher, subscriber, and broker.

Compared with my previous ESP32 web-server task, this project introduced **cloud-based LED control**, allowing the ESP32 to communicate with Adafruit IO through the Internet.

This task improved my understanding of:

- ESP32 programming
- MQTT communication
- Adafruit IO
- Wi-Fi connectivity
- Cloud IoT platforms
- Feed configuration
- Dashboard control
- GPIO control
- ESP32-to-cloud communication

---

# Final Result

The completed system demonstrates **cloud-based LED control using ESP32, MQTT, and Adafruit IO**.

The ESP32 connects to Wi-Fi and Adafruit IO. The user controls the `light` feed from the Adafruit IO dashboard. The ESP32 receives the ON or OFF command and changes the LED state through GPIO 2.

### Final System

```text
Adafruit IO Dashboard
          ↓
     MQTT / Cloud
          ↓
         ESP32
          ↓
       GPIO 2
          ↓
          LED
```

This project demonstrates the basic working of a **cloud-connected IoT system** and provides a foundation for more advanced IoT projects using sensors, dashboards, and remote device control.
