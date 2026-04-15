# Smart Plant Monitoring System

## Overview

This project is a smart plant monitoring system built for an ESP8266-based board using Blynk IoT for remote monitoring and control. It reads environmental data from a DHT11 sensor, a DS18B20 temperature probe, and a soil moisture sensor, then sends the values to the Blynk app. It also controls a water pump relay manually through the Blynk interface.

## About Blynk IoT Platform

Blynk IoT is a cloud-based platform for building IoT applications and dashboards quickly. It enables remote monitoring, control, and automation through:

- Mobile and web dashboards with customizable widgets
- Virtual pins for sending and receiving sensor data
- Auth tokens to securely connect devices to the cloud
- Cloud-side rules, notifications, and data logging

In this project, Blynk is used to display plant sensor values and to control the water pump relay from a smartphone or tablet. The device sends DHT11 environmental data and DS18B20/soil moisture values to Blynk virtual pins, and it receives pump control commands via a Blynk switch widget.

## Key Features

- Real-time humidity and temperature monitoring via DHT11
- Soil moisture measurement using an analog moisture sensor
- Soil temperature monitoring with a Dallas/DS18B20 sensor
- Water pump relay control from the Blynk app
- Serial debug output for sensor readings

## Hardware Components

- ESP8266 board (NodeMCU, Wemos D1 mini, or equivalent)
- DHT11 temperature and humidity sensor
- DS18B20 waterproof temperature sensor
- Analog soil moisture sensor
- Relay module to drive a water pump
- Water pump or solenoid valve
- 4.7kΩ pull-up resistor for DS18B20 data line

## Wiring

Based on the project sketch in `Project.ino`:

- `DHT11` data pin -> `D2`
- `DS18B20` data pin -> `D2` (configured via OneWire on the same board pin)
- Soil moisture sensor analog output -> `A0`
- Relay control pin -> `D5`
- Relay VCC/GND -> board VCC/GND
- DHT11 VCC/GND -> board 3.3V/GND
- DS18B20 VCC/GND -> board 3.3V/GND

> Note: The sketch currently defines both the DHT11 and DS18B20 bus on `D2`. If you use separate pins for these sensors, update the pin definitions in `Project.ino` accordingly.

## Software and Libraries

The firmware is written in Arduino C++ for the ESP8266 platform.

Required libraries:

- `BlynkSimpleEsp8266.h`
- `DHT.h`
- `OneWire.h`
- `DallasTemperature.h`
- `SPI.h` (included by the board core)

## Code Behavior

- `setup()`
  - Initializes serial output at 9600 baud
  - Configures the relay pin and disables the pump at startup
  - Starts the DHT11 sensor and the DS18B20 sensor bus
  - Connects to Wi-Fi and Blynk using provided credentials
  - Schedules `sendSensor()` to run every second via `SimpleTimer`

- `sendSensor()`
  - Reads humidity and temperature from the DHT11
  - Sends humidity to Blynk virtual pin `V5`
  - Sends temperature to Blynk virtual pin `V6`

- `sendTemps()`
  - Reads the analog soil moisture sensor on `A0`
  - Maps the raw analog reading to a moisture percentage
  - Requests the DS18B20 temperature and sends it to Blynk `V1`
  - Sends soil moisture percentage to Blynk `V2`
  - Prints debug output to Serial

- `BLYNK_WRITE(V12)`
  - Listens for relay control commands from the Blynk app
  - Turns the water pump on when `V12` is set to `1`
  - Turns the pump off otherwise

## Blynk Interface Mapping

- `V1`: DS18B20 soil temperature
- `V2`: Soil moisture percentage
- `V5`: DHT11 humidity
- `V6`: DHT11 temperature
- `V12`: Water pump relay control switch

## Setup Instructions

1. Open the project in the Arduino IDE.
2. Install the ESP8266 board support package and the required libraries.
3. Update the Wi-Fi credentials in `Project.ino`:
   - `ssid[]`
   - `pass[]`
4. Replace the Blynk auth token in `Project.ino` with your own token:
   - `auth[]`
5. Verify the wiring and upload the sketch to the ESP8266 board.
6. Create a Blynk project and add widgets for the matching virtual pins.
7. Run the Blynk app and monitor the plant environment remotely.

## Notes

- The current sketch uses a 1-second sensor update interval for both sensor groups.
- Because the relay is active-low in this circuit, writing `LOW` turns the pump on and `HIGH` turns it off.
- Keep your Blynk auth token and Wi-Fi credentials secure and do not commit them to public repositories.

## Project Report

A detailed project report is included in `Project Report.docx`. It contains the system architecture, component descriptions, design objectives, and testing notes for this smart plant monitoring system.

## License

This repository does not include a license file. Use the code for personal or educational purposes, and add an appropriate license if you plan to share it publicly.
