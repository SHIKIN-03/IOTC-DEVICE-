# Smart Sign Language Emergency Alert System Using IoT

## Project Overview

This project is a touch-activated alert prototype built using a
**NodeMCU 1.0 (ESP-12E) based on the ESP8266**, a digital touch sensor,
an LED and an active buzzer.

When the touch sensor detects contact, the NodeMCU processes its digital
signal. The LED blinks, the buzzer sounds, and the Arduino IDE Serial
Monitor displays **`Touch Detected!`**. When contact is removed, the LED
and buzzer switch off and the Serial Monitor displays
**`Not Detected`**.

The prototype demonstrates sensor interfacing, digital input processing,
state-based event detection and output control.

> **Implementation note:** The report title refers to a "Smart Sign
> Language Emergency Alert System." The hardware and software documented
> in the report implement touch detection and local alerts. Fingerprint
> recognition, sign-language recognition, cloud transmission, mobile
> notifications and remote alerts are not part of the current
> implementation. The ESP8266 has Wi-Fi capability, but Wi-Fi is a
> possible future enhancement, not a feature currently used by the
> prototype.

## Features

-   Detects touch through a digital touch sensor.
-   Activates an LED and an active buzzer when touch is detected.
-   Displays touch status in the Arduino IDE Serial Monitor.
-   Uses a previous-state variable to print status messages on state
    changes rather than continuously repeating them.
-   Uses the ESP8266 NodeMCU, which has built-in Wi-Fi and can be
    extended for network-based features.

## Hardware Requirements

  -----------------------------------------------------------------------
  Component                           Purpose
  ----------------------------------- -----------------------------------
  NodeMCU 1.0 (ESP-12E / ESP8266)     Reads the sensor and controls the
                                      outputs

  TTP223 digital touch sensor         Detects physical touch

  LED                                 Provides a visual alert

  220 Ω resistor                      Limits current through the LED

  Active buzzer                       Provides an audible alert

  Breadboard and jumper wires         Make circuit connections

  USB data cable                      Powers/programs the NodeMCU and
                                      provides serial communication

  Computer                            Runs Arduino IDE and Serial Monitor
  -----------------------------------------------------------------------

## Pin Connections

  Component                  NodeMCU connection
  -------------------------- -----------------------------
  TTP223 SIG / OUT           D2
  TTP223 VCC                 3V3
  TTP223 GND                 GND
  LED anode (+)              D5 through a 220 Ω resistor
  LED cathode (−)            GND
  Active buzzer signal (+)   D6
  Active buzzer GND (−)      GND
  NodeMCU                    Computer through USB

Check the labels and voltage requirements of the exact modules before
powering the circuit. ESP8266 GPIO uses 3.3 V logic. If the buzzer draws
more current than a GPIO can safely supply, use a suitable transistor
driver rather than powering it directly from the pin.

## Software Requirements

-   Arduino IDE (the report records version 2.3.10).
-   ESP8266 board package for Arduino IDE.
-   Board selection: **NodeMCU 1.0 (ESP-12E Module)**.
-   Correct serial port for the connected board (the report records
    COM3).
-   Serial Monitor baud rate: **115200**.

## How It Works

1.  Connect the NodeMCU to the computer using a USB data cable.
2.  Open the project sketch in Arduino IDE.
3.  Select **NodeMCU 1.0 (ESP-12E Module)** and the correct port.
4.  Upload the sketch.
5.  Open Serial Monitor and set the baud rate to **115200**.
6.  The `setup()` function configures D2 as input and D5/D6 as outputs.
    The LED and buzzer are initially switched off.
7.  The `loop()` function repeatedly reads the touch sensor using
    `digitalRead()`.
8.  When the sensor changes from LOW to HIGH, the program reports
    `Touch Detected!`, activates the buzzer and blinks the LED.
9.  When the sensor returns from HIGH to LOW, the program reports
    `Not Detected` and switches the buzzer and LED off.
10. The process repeats while the board is powered.

## State-Change Logic

The program uses two values:

-   `currentState`: the latest reading from the touch sensor.
-   `previousState`: the reading from the previous loop cycle.

  --------------------------------------------------------------------------
  Previous state    Current state     Meaning           System response
  ----------------- ----------------- ----------------- --------------------
  LOW               LOW               No touch          Outputs remain off

  LOW               HIGH              New touch         Print
                                                        `Touch Detected!`;
                                                        buzzer on; LED
                                                        blinks

  HIGH              HIGH              Touch continues   Keep the touch
                                                        condition active; do
                                                        not print a new
                                                        event message

  HIGH              LOW               Touch released    Print
                                                        `Not Detected`;
                                                        buzzer and LED off
  --------------------------------------------------------------------------

## Testing and Expected Results

  -----------------------------------------------------------------------
  Test condition                      Expected result
  ----------------------------------- -----------------------------------
  No touch                            LED off, buzzer off, `Not Detected`
                                      status

  Touch sensor activated              LED blinks, buzzer sounds,
                                      `Touch Detected!` appears

  Touch removed                       LED off, buzzer off, `Not Detected`
                                      appears

  Continuous touch                    The touch event message is not
                                      repeatedly printed for every loop
                                      cycle

  Serial Monitor                      Status messages appear at 115200
                                      baud
  -----------------------------------------------------------------------

The technical report records successful testing of these basic touch and
alert behaviours.

## Current Scope and Limitations

-   The current prototype provides local alerts and Serial Monitor
    output.
-   Although the ESP8266 supports Wi-Fi, the documented implementation
    does not send data to a cloud service or mobile application.
-   No web dashboard, permanent event database or remote notification
    service is implemented.
-   The prototype detects a touch; it does not identify a person,
    recognise a fingerprint pattern or translate sign language.
-   The prototype would need further development and validation before
    any real emergency use.

## Future Enhancements

-   Use ESP8266 Wi-Fi to send touch-event notifications to a suitable
    server or IoT platform.
-   Add a mobile application or web dashboard for remote monitoring.
-   Store event timestamps in a cloud database.
-   Add multiple input sensors or configurable emergency messages.
-   Add reliable power management and an enclosure for portable
    deployment.
-   If person identification is required, integrate a compatible
    fingerprint module and implement fingerprint enrolment, matching and
    ID-to-message mapping as a separate feature.

## Suggested Repository Structure

``` text
smart-sign-language-emergency-alert/
├── README.md
├── code/
│   └── touch_alert.ino
├── images/
│   ├── system_architecture.png
│   ├── hardware_setup.png
│   └── serial_monitor_output.png
└── docs/
    └── technical_report.pdf
```

Add your actual Arduino sketch and project photographs to these folders.
The supplied report describes the implementation but does not include a
complete standalone source-code listing.

## Troubleshooting

-   **No serial output:** Check the selected COM port and set Serial
    Monitor to 115200 baud.
-   **Unreadable characters:** Confirm that the Serial Monitor baud rate
    matches `Serial.begin(115200)`.
-   **Touch is always detected or never detected:** Check TTP223 VCC,
    GND and OUT wiring, and confirm the sensor's output behaviour.
-   **LED does not respond:** Check LED polarity, resistor connection,
    D5 wiring and common ground.
-   **Buzzer does not sound:** Confirm buzzer polarity and
    voltage/current requirements. Use a suitable driver circuit if
    needed.
-   **Upload fails:** Confirm the NodeMCU board selection, correct port
    and USB data cable.

## Acknowledgement

This project was developed as an academic IoT activity to explore
microcontroller programming, sensor data acquisition, digital event
processing and alert generation.

------------------------------------------------------------------------

**Project platform:** NodeMCU ESP8266 (ESP-12E)\
**Development environment:** Arduino IDE 2.3.10\
**Serial communication:** 115200 baud
