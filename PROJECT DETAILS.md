Smart Heater Using ESP8285 – A DIY IoT Project
Introduction
I built an intelligent smart heater that you can control using your smartphone — and it’s not just about turning it on or off. This heater can monitor its own temperature and turn itself off automatically if it gets too hot, making it safer and smarter. At the heart of this project is the ESP8285 module, a tiny Wi-Fi-enabled microcontroller that works like a mini computer. Alongside it, I used a temperature sensor, a relay, a push button, and even designed a custom PCB (Printed Circuit Board) to neatly hold everything together. I also programmed the system using an FTDI232 module connected to my laptop.

This write-up will walk you through all the components, how it works, how I built it, and what I learned — in a clear and easy-to-understand way.

Components Used
ESP8285 Module: This is the brain of the system. It handles Wi-Fi communication, controls all other components, and connects to the mobile app.

DS18B20 Temperature Sensor: A digital sensor connected to a data pin. It constantly checks the heater’s temperature and sends the data to the ESP8285.

Relay Module: Acts like a switch. It turns the heater on or off based on commands from the ESP8285.

Push Button: A reset button that re-enables the heater if it shuts off due to high temperature.

LED Indicator: Shows Wi-Fi connection status by blinking during setup and turning solid once connected.

FTDI232 USB to Serial Adapter: Used to program the ESP8285 from a computer via USB.

PCB (Printed Circuit Board): I custom-designed this board to mount and connect all parts neatly using copper traces instead of wires.

Power Supply: I used 3.3V for the ESP8285 and sensor, and 5V for the relay, sourced from USB or a battery.

Resistors and Capacitors: Used for proper voltage control and signal stability (e.g., 4.7kΩ with the sensor, 10kΩ with the button, 220Ω with the LED, and 0.1µF capacitors for filtering noise).

How It Works
Wi-Fi Setup
The ESP8285 connects to home Wi-Fi using a method called SmartConfig, which allows it to receive Wi-Fi credentials from a smartphone without hardcoding. Once connected, it remembers the Wi-Fi details in its internal memory.

App Communication
The heater communicates with a mobile app using the MQTT protocol (a lightweight messaging method). The ESP8285 subscribes to specific message topics and reacts to commands like "Turn heater ON" or "Turn heater OFF."

Automatic Temperature-Based Control
The DS18B20 sensor keeps checking the temperature. If it crosses 37°C, the ESP8285 immediately turns off the relay, which switches off the heater for safety. It also sends a log message to the cloud informing about the shutdown.

Manual Restart with Button
If the heater is shut down due to high temperature, I can press the physical button to turn it back on. This resets a software flag and also sends a message to the log server noting that the heater was restarted manually.

Wi-Fi Indicator LED
The LED connected to the ESP8285 blinks during Wi-Fi setup and stays ON once connected, helping visually confirm the status.

Building Process
1. PCB Design
I used EasyEDA (or similar software) to design a custom PCB layout. I placed the ESP8285, sensor, relay, button, and other components in positions that kept connections short and clean. I also added:

A dedicated FTDI header for programming,

A ground plane to reduce electrical noise,

A voltage regulator circuit to convert 5V input to 3.3V.

2. Circuit Connections
DS18B20 connected to pin 13 with a 4.7kΩ pull-up resistor.

Relay connected to pin 12. A diode (like 1N4007) was added to prevent backflow of current.

Push Button connected to pin 15 with a 10kΩ pull-up resistor.

LED on pin 3 with a 220Ω resistor.

Added capacitors (10µF and 0.1µF) near the voltage regulator for stable power.

3. Programming the ESP8285
Using the FTDI232 adapter:

Connected TX/RX and powered the ESP8285 with 3.3V.

Pulled GPIO 0 to LOW during programming using a switch or DTR pin trick.

Used the Arduino IDE with ESP8266 board support to upload the code.

Monitored output via Serial Monitor for real-time updates and debugging.

4. Wi-Fi and Cloud Setup
Used SmartConfig through the mobile app to connect the ESP8285 to Wi-Fi.

Stored AWS IoT credentials securely in the code to enable cloud communication using encrypted certificates.

Messages like temperature logs and control events were published to specific MQTT topics for live tracking.

Smart Features
Overheat Protection: Auto shut-off if temperature exceeds 37°C.

App-Based Control: Heater can be turned ON/OFF remotely.

Memory Retention: Remembers last status and Wi-Fi credentials after power loss.

Live Monitoring: Sends temperature and event updates to cloud logs.

Problems Faced and Solutions
Code Upload Failed: Resolved by properly pulling GPIO 0 LOW during upload.

Relay Didn’t Work: Fixed by using a separate 5V supply, as the FTDI couldn't provide enough power.

Wi-Fi Disconnects: Added reconnection logic every 20 seconds in the code.

Library Conflicts: Removed duplicate MQTT libraries from project folders.

What I Learned
This project taught me:

How to work with the ESP8285 module and flash firmware using FTDI.

How to design and manufacture PCBs.

How MQTT works to control devices over the internet.

How to troubleshoot hardware issues like power, GPIO, and library errors.

Future Improvements
Remote Control: Add 433 MHz RF remote for offline control.

OLED Display: Show real-time temperature on a small screen.

OTA Updates: Add Over-The-Air firmware update capability.

Multiple Sensors: Monitor temperature in different areas.

Conclusion
Building this smart heater was an exciting journey into electronics and IoT. I created a system that connects to Wi-Fi, responds to my phone, monitors temperature, and keeps itself safe — all controlled by the compact ESP8285 and supported by a custom-designed PCB. I feel proud of bringing together both hardware and software to make something useful, reliable, and future-ready. Today is Sunday, August 3, 2025 — and I'm excited to test it more and share it with friends!
