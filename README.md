# TLV MK-01 — Real-Time Rocket Telemetry & Ground Station

**A real-time telemetry system for atmospheric data acquisition, flight monitoring, and sensor visualization using an STM32 microcontroller and a JavaFX-based ground station.**

<p align="center">
  <strong>Project:</strong> TLV MK-01<br>
  <strong>Organization:</strong> Spaceverse Technologies Pvt. Ltd.<br>
  <strong>Microcontroller:</strong> STM32F411CEU6 Black Pill<br>
  <strong>Communication:</strong> 433 MHz SiK Telemetry Radio<br>
  <strong>Ground Station:</strong> JavaFX Desktop Application
</p>

---

## Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Project Objectives](#2-project-objectives)
- [3. System Architecture](#3-system-architecture)
- [4. Hardware Components](#4-hardware-components)
- [5. Software and Technologies](#5-software-and-technologies)
- [6. Sensor Specifications and Connections](#6-sensor-specifications-and-connections)
- [7. Telemetry Communication](#7-telemetry-communication)
- [8. Telemetry Packet Format](#8-telemetry-packet-format)
- [9. STM32 Firmware Setup](#9-stm32-firmware-setup)
- [10. Ground Station Setup](#10-ground-station-setup)
- [11. Running the Dashboard](#11-running-the-dashboard)
- [12. Dashboard Features](#12-dashboard-features)
- [13. Repository Structure](#13-repository-structure)
- [14. Testing and Validation](#14-testing-and-validation)
- [15. Troubleshooting](#15-troubleshooting)
- [16. Development Workflow](#16-development-workflow)
- [17. Future Improvements](#17-future-improvements)
- [18. Safety Considerations](#18-safety-considerations)
- [19. Contributors](#19-contributors)
- [20. License](#20-license)

---

## 1. Project Overview

TLV MK-01 is a rocket telemetry and ground monitoring project designed to acquire sensor measurements, transmit telemetry packets wirelessly, and visualize received information through a desktop dashboard.

The onboard telemetry system uses an STM32F411CEU6 Black Pill microcontroller to interface with environmental sensors, inertial sensors, and a GPS receiver.

The firmware collects sensor measurements, formats them into telemetry packets, calculates a CRC-16 checksum, and transmits the packets through a 433 MHz SiK telemetry radio.

At the ground station, a JavaFX desktop application receives serial telemetry data, processes the packets, and presents available measurements through a graphical user interface.

The system is intended to support atmospheric data collection, telemetry experimentation, and rocket flight monitoring.

### Key Capabilities

- Environmental data acquisition.
- Pressure-based altitude estimation.
- Accelerometer and gyroscope measurements.
- GPS position and navigation data.
- Wireless serial telemetry.
- Structured telemetry packet generation.
- CRC-16 checksum generation.
- Real-time ground station monitoring.
- JavaFX-based graphical interface.

> **Note:** Actual capabilities depend on the connected hardware, firmware configuration, sensor initialization, radio link, and dashboard implementation. GPS and battery measurements may be unavailable when the corresponding data is not being received or the relevant hardware is not configured.

---

## 2. Project Objectives

The primary objectives of TLV MK-01 are:

1. Develop an onboard telemetry acquisition system using an STM32 microcontroller.
2. Integrate environmental and inertial sensors.
3. Acquire GPS navigation data through a UART interface.
4. Transmit structured telemetry packets over a wireless serial link.
5. Develop a desktop ground station for receiving and visualizing telemetry.
6. Monitor sensor readings and identify communication failures.
7. Establish a modular hardware and software architecture that can be expanded in future versions.

---

## 3. System Architecture

The system consists of two major subsystems:

1. **Onboard Telemetry Unit**
2. **Ground Station**

### 3.1 Onboard Telemetry Unit

The onboard unit collects measurements from the connected sensors and transmits the resulting telemetry packets.

```text
          ONBOARD TELEMETRY UNIT

    +-----------------------------+
    |       STM32F411CEU6          |
    |         Black Pill           |
    +--------------+--------------+
                   |
          +--------+--------+
          |                 |
          v                 v
  +---------------+  +---------------+
  | Environmental |  | Inertial      |
  | Sensors       |  | Sensors       |
  |               |  |               |
  | SHT31         |  | MPU6050       |
  | MS5611        |  | ADXL375       |
  +---------------+  +---------------+
          |
          +-------------------------+
                                    |
                          +---------v---------+
                          |   GPS Receiver    |
                          |     NEO-M8N       |
                          +---------+---------+
                                    |
                                    v
                          +-------------------+
                          | Telemetry Packet  |
                          | Generation + CRC  |
                          +---------+---------+
                                    |
                                    v
                          +-------------------+
                          | 433 MHz SiK Radio |
                          +---------+---------+
                                    |
                             Wireless Link
                                    |
                                    v
                           GROUND STATION
```

### 3.2 Ground Station

The ground station receives telemetry through a serial connection to the ground-side SiK radio.

```text
          GROUND STATION

    +----------------------+
    | 433 MHz SiK Radio    |
    +----------+-----------+
               |
               | USB Serial
               v
    +----------------------+
    | Serial Communication |
    +----------+-----------+
               |
               v
    +----------------------+
    | Telemetry Processing |
    | and Packet Parsing   |
    +----------+-----------+
               |
               v
    +----------------------+
    | JavaFX Dashboard     |
    |                      |
    | Sensor Readings      |
    | Telemetry Monitoring |
    | Data Visualization   |
    +----------------------+
```

The diagram illustrates the intended data flow. It does not imply that every sensor, dashboard feature, or flight function has been validated in hardware.

---

## 4. Hardware Components

| Component | Purpose | Interface |
|---|---|---|
| STM32F411CEU6 Black Pill | Main telemetry controller | I²C, UART, USB |
| SHT31 | Temperature and relative humidity | I²C |
| MS5611 | Atmospheric pressure and barometric altitude estimation | I²C |
| MPU6050 | Three-axis acceleration and angular velocity | I²C |
| ADXL375 | High-g acceleration measurement | I²C |
| NEO-M8N GPS | Position, speed, course, altitude, and satellite information | UART |
| SiK telemetry radio | Wireless serial telemetry | UART |
| ST-Link V2 | Firmware programming and debugging | SWD |
| Computer | Firmware development and ground station operation | USB |

The listed components reflect the intended project configuration. Verify the actual hardware revision and installed components before using the wiring instructions.

---

## 5. Software and Technologies

### Embedded Firmware

- Arduino IDE
- STM32 Arduino core
- C/C++
- STM32F411CEU6
- I²C communication
- UART communication
- NMEA GPS sentence parsing
- CRC-16 checksum generation

### Ground Station

- Java
- JavaFX
- Apache Maven
- jSerialComm, if included in the project dependencies
- Serial port communication
- Telemetry packet parsing
- Graphical data presentation

### Development and Debugging Tools

- STM32CubeProgrammer
- ST-Link V2
- Arduino Serial Monitor
- CoolTerm
- Git
- GitHub

**Important:** Confirm the Java, JavaFX, Maven, and jSerialComm versions from the project's `pom.xml` and source files. Do not assume that every development tool is required at runtime.

---

## 6. Sensor Specifications and Connections

### 6.1 I²C Connections

The current firmware configuration uses the following pins:

| STM32 Pin | Signal | Connected Device |
|---|---|---|
| PB6 | SCL | I²C clock |
| PB7 | SDA | I²C data |
| 3V3 | Power | Sensors requiring 3.3 V |
| GND | Ground | Common ground |

The firmware initializes I²C at 100 kHz in the current configuration.

### 6.2 Expected I²C Addresses

| Sensor | Expected Address |
|---|---|
| SHT31 | `0x44` |
| MPU6050 | `0x68` |
| ADXL375 | `0x1D` |
| MS5611 | `0x77` |

These are the addresses configured in the firmware. Actual addresses may vary with sensor module configuration.

### 6.3 GPS UART Connections

The firmware declares:

```cpp
Uart GPSSerial(PA_3, PA_2);
```

This represents RX followed by TX.

| STM32 Pin | Signal | GPS Connection |
|---|---|---|
| PA3 | UART RX | GPS TX |
| PA2 | UART TX | GPS RX |
| GND | Ground | GPS GND |
| Appropriate supply | Power | GPS VCC, according to module specifications |

Configured baud rate:

```cpp
GPSSerial.begin(9600);
```

The GPS receiver must have suitable antenna placement and sufficient satellite visibility to obtain a fix.

### 6.4 SiK Radio UART Connections

The firmware declares:

```cpp
Uart SiKSerial(PA_10, PA_9);
```

This represents RX followed by TX.

| STM32 Pin | Signal | Radio Connection |
|---|---|---|
| PA10 | UART RX | Radio TX |
| PA9 | UART TX | Radio RX |
| GND | Ground | Radio GND |
| Appropriate supply | Power | Radio power input, according to module specifications |

Configured baud rate:

```cpp
SiKSerial.begin(57600);
```

The onboard and ground-side radio configurations must be compatible.

> **Electrical safety:** Check the exact radio module's supply voltage and current requirements. Do not assume every SiK-compatible module can be powered directly from the same pin.

---

## 7. Telemetry Communication

The onboard firmware uses serial communication for telemetry transmission.

### 7.1 Data Acquisition

The firmware reads measurements from the environmental and inertial sensors.

- SHT31: temperature and relative humidity.
- MS5611: atmospheric pressure and estimated barometric altitude.
- MPU6050: acceleration and gyroscope measurements.
- ADXL375: high-g acceleration.
- NEO-M8N: GPS navigation data.

### 7.2 Packet Generation

The firmware combines the available readings into a comma-separated packet.

A sequence number helps identify packet progression, and a CRC-16 checksum is appended to the packet.

### 7.3 Wireless Transmission

The packet is transmitted using:

```cpp
SiKSerial.println(p);
```

The firmware also prints the packet to USB serial using:

```cpp
Serial.println(p);
```

These are two separate serial outputs.

### 7.4 Ground Reception

The ground-side radio forwards received serial data to the computer. The JavaFX application can then process the received packets, provided its serial configuration and parser match the firmware.

### 7.5 Configured Transmission Interval

The firmware uses:

```cpp
if (now - lastTelemetry >= 100)
```

This sets a nominal target interval of 100 milliseconds, equivalent to 10 telemetry updates per second.

Actual transmission timing may be slower because sensor reads, conversion delays, serial transmission, and other operations take time.

---

## 8. Telemetry Packet Format

The current firmware defines the following packet structure:

```text
$TLV,SEQ,TEMP,HUM,PRES_HPA,ALT_BARO,
MPU_AX,MPU_AY,MPU_AZ,
GYRO_X,GYRO_Y,GYRO_Z,
ADXL_X,ADXL_Y,ADXL_Z,
GPS_FIX,GPS_SATS,GPS_LAT,GPS_LON,
GPS_ALT,GPS_SPEED,GPS_COURSE,
STATUS,RSSI,BATT,CRC
```

The actual packet is transmitted as one line, without the formatting line breaks shown above.

### 8.1 Field Descriptions

| Field | Description |
|---|---|
| `$TLV` | Packet identifier |
| `SEQ` | Incrementing sequence number |
| `TEMP` | Temperature in °C |
| `HUM` | Relative humidity in % |
| `PRES_HPA` | Atmospheric pressure in hPa |
| `ALT_BARO` | Estimated barometric altitude in metres |
| `MPU_AX` | MPU6050 X-axis acceleration in g |
| `MPU_AY` | MPU6050 Y-axis acceleration in g |
| `MPU_AZ` | MPU6050 Z-axis acceleration in g |
| `GYRO_X` | X-axis angular velocity in °/s |
| `GYRO_Y` | Y-axis angular velocity in °/s |
| `GYRO_Z` | Z-axis angular velocity in °/s |
| `ADXL_X` | ADXL375 X-axis acceleration using the configured scale |
| `ADXL_Y` | ADXL375 Y-axis acceleration using the configured scale |
| `ADXL_Z` | ADXL375 Z-axis acceleration using the configured scale |
| `GPS_FIX` | GPS fix indicator |
| `GPS_SATS` | Number of satellites reported |
| `GPS_LAT` | Latitude in decimal degrees |
| `GPS_LON` | Longitude in decimal degrees |
| `GPS_ALT` | GPS altitude in metres |
| `GPS_SPEED` | GPS speed in metres per second |
| `GPS_COURSE` | GPS course in degrees |
| `STATUS` | Bitmask representing sensor/GPS status |
| `RSSI` | RSSI placeholder unless implemented |
| `BATT` | Battery-voltage placeholder unless enabled |
| `CRC` | CRC-16 checksum in hexadecimal |

### 8.2 Status Bitmask

The firmware sets these bits:

| Bit | Value | Meaning |
|---|---:|---|
| Bit 0 | 1 | SHT31 status flag |
| Bit 1 | 2 | MS5611 status flag |
| Bit 2 | 4 | MPU6050 status flag |
| Bit 3 | 8 | ADXL375 status flag |
| Bit 4 | 16 | GPS fix available |

The `STATUS` field is the sum of the active bit values.

For example, a status value of `15` indicates that the four sensor status flags are set, while the GPS fix bit is not set.

**Note:** These flags indicate the firmware's current status variables. They should not be treated as proof of measurement accuracy or complete hardware validation.

### 8.3 CRC-16

The firmware uses a CRC-16 calculation with:

- Initial value: `0xFFFF`
- Polynomial: `0x1021`

The checksum is calculated over the packet string before the checksum field is appended.

The ground station must implement the same algorithm and calculate the checksum over the same bytes to validate packets correctly.

---

## 9. STM32 Firmware Setup

### 9.1 Install Required Tools

Install:

1. Arduino IDE.
2. STM32 board support package.
3. Required libraries, if any are added to the firmware.
4. STM32CubeProgrammer.
5. ST-Link V2 drivers, where required.

### 9.2 Configure the Board

In Arduino IDE, select the STM32 board configuration matching the Black Pill F411CEU6.

The previously used configuration included:

- Board family: Generic STM32F4 series.
- Board part number: BlackPill F411CE.
- Upload method: STM32CubeProgrammer (SWD).

Menu labels may vary by STM32 core version.

### 9.3 Connect ST-Link V2

Typical SWD connections:

| ST-Link V2 | STM32 Black Pill |
|---|---|
| SWDIO | PA13 / SWDIO |
| SWCLK | PA14 / SWCLK |
| GND | GND |
| VTref / 3.3 V reference | Target reference, according to programmer setup |
| NRST | RST, if available and required |

Verify the board's pinout before connecting anything.

### 9.4 Upload Firmware

1. Connect the sensors and inspect the wiring.
2. Connect the ST-Link to the STM32.
3. Select the correct board and upload method.
4. Compile the firmware.
5. Upload the firmware.
6. Check the upload result.
7. Power-cycle the board and observe its startup behavior.

BOOT or RESET should not be needed for every normal telemetry transmission.

### 9.5 Verify Telemetry

Open the appropriate serial terminal.

For USB serial:

```text
Baud rate: 115200
Data bits: 8
Parity: None
Stop bits: 1
```

For the SiK radio serial link:

```text
Baud rate: 57600
Data bits: 8
Parity: None
Stop bits: 1
```

Verify the port name in your operating system. It may differ between computers or after reconnecting USB devices.

A changing sequence number indicates that packets are being generated. It does not, by itself, prove that all sensor readings are updating correctly.

---

## 10. Ground Station Setup

### 10.1 Prerequisites

Install the following:

- A compatible JDK.
- Apache Maven.
- A working serial interface for the ground-side radio.
- The required JavaFX dependencies defined by the project.

Check Java:

```powershell
java -version
```

Check Maven:

```powershell
mvn -version
```

### 10.2 Clone the Repository

Replace `YOUR_USERNAME` with your GitHub username and `YOUR_REPOSITORY` with the repository name.

```powershell
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

Navigate to the project directory:

```powershell
cd YOUR_REPOSITORY
```

### 10.3 Install Dependencies and Compile

Run:

```powershell
mvn clean compile
```

If compilation succeeds, Maven should report a successful build.

If compilation fails, inspect the reported source file and line number before trying to run the application.

---

## 11. Running the Dashboard

The project has previously been launched using:

```powershell
mvn clean javafx:run -DserialPort=COM9
```

Run this command from the directory containing `pom.xml`.

The example uses `COM9`, but the correct port depends on the ground-side radio and computer.

### Serial Port Configuration

Before launching:

1. Connect the ground-side SiK radio.
2. Open Device Manager on Windows.
3. Expand **Ports (COM & LPT)**.
4. Identify the radio's serial port.
5. Close CoolTerm or any other application currently using that port.
6. Launch the JavaFX dashboard.

If the application supports a configurable baud rate, set it to match the ground-side serial configuration.

> **Important:** The command-line property `-DserialPort=COM9` only works if the application reads that property. Confirm the implementation in the Java source code.

### Build the Application

To build the project without launching JavaFX:

```powershell
mvn clean package
```

The resulting JAR file, if packaging is configured, will be located under `target/`.

A runnable JAR may require additional JavaFX modules or a configured launcher, depending on the Maven project setup.

---

## 12. Dashboard Features

The dashboard is intended to provide a graphical interface for monitoring telemetry received from the onboard system.

The documented dashboard fields include:

- Temperature.
- Humidity.
- MPU6050 acceleration.
- MPU6050 gyroscope measurements.
- ADXL375 acceleration.
- Telemetry rate.

Additional fields, graphs, packet logging, and navigation displays should be documented here after confirming that they exist in the current JavaFX source code.

### Suggested Ground Station Workflow

1. Connect the ground-side radio.
2. Identify the correct COM port.
3. Launch the dashboard.
4. Confirm that telemetry packets are received.
5. Check whether the sequence number increments.
6. Check whether sensor readings change when the sensors are moved or environmental conditions change.
7. Inspect the status field for sensor communication failures.
8. Save logs if logging functionality is implemented.

---

## 13. Repository Structure

The expected JavaFX project structure is:

```text
TLV_MK01_REALTIME_DASHBOARD/
│
├── pom.xml
├── README.md
├── .gitignore
│
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── spaceverse/
│       │           └── tlv/
│       │               └── App.java
│       │
│       └── resources/
│           └──
│
└── target/
```

The STM32 firmware can be maintained in the same repository or in a separate repository.

If firmware is included, a recommended structure is:

```text
TLV_MK01/
│
├── README.md
├── .gitignore
│
├── firmware/
│   └── TLV_MK01_Complete_Telemetry_Firmware.ino
│
├── ground-station/
│   ├── pom.xml
│   └── src/
│
├── hardware/
│   ├── wiring.md
│   └── diagrams/
│
└── docs/
    ├── system-architecture.md
    └── test-results.md
```

Adapt this structure to the files that actually exist in the repository.

**Do not commit generated `target/` files, IDE caches, compiled binaries, or sensitive configuration files unless there is a specific reason to keep them.**

---

## 14. Testing and Validation

Testing should be performed incrementally, starting with individual sensors and progressing to the complete telemetry link.

### 14.1 Sensor Tests

- Verify each sensor's supply voltage.
- Run an I²C scanner.
- Confirm that the expected device addresses are detected.
- Read each sensor independently.
- Check that values respond to changes in the environment or movement.
- Confirm that sensor failures are reflected in the status field.

### 14.2 Firmware Tests

- Verify that the firmware starts after a normal power cycle.
- Confirm that the telemetry sequence number increments.
- Confirm that packets are emitted at the expected approximate rate.
- Check for `nan` readings.
- Check whether status flags match sensor read results.
- Verify the CRC calculation.

### 14.3 Radio Tests

- Confirm compatible baud rates and radio configurations.
- Test at a short, safe distance first.
- Compare the transmitted USB serial output with the received ground-side serial output.
- Check for missing, corrupted, or delayed packets.
- Record the test conditions and measured results.

### 14.4 Dashboard Tests

- Verify that the application opens successfully.
- Confirm that the correct COM port is selected.
- Check that the packet parser matches the firmware's field order.
- Verify that the sequence number and sensor fields are interpreted correctly.
- Test the dashboard using recorded sample data before relying on live telemetry.

### 14.5 Flight Readiness

A successful serial connection or ground test does not establish flight readiness. Hardware reliability, radio performance, sensor accuracy, power integrity, environmental suitability, and applicable safety requirements must be validated separately.

---

## 15. Troubleshooting

### Problem 1: Dashboard Does Not Compile

**Possible cause:** Java syntax errors, missing dependencies, or an incompatible Java configuration.

Run:

```powershell
mvn clean compile
```

Inspect the first `[ERROR]` entry and fix the reported source file and line number.

For example:

```text
App.java:[43,32] illegal start of type
```

This indicates a Java syntax error at or near line 43. Inspect the surrounding braces, method declarations, and statements.

### Problem 2: COM Port Is Not Available

**Possible causes:**

- The radio is disconnected.
- The COM port changed.
- Another program is using the port.
- The USB-to-serial driver is unavailable.

**Solutions:**

1. Check Windows Device Manager.
2. Reconnect the radio.
3. Close other serial applications.
4. Use the currently assigned COM port.

### Problem 3: Sequence Number Increments but Sensor Values Stay Fixed

**Possible causes:**

- Sensor readings are not being refreshed.
- I²C communication has failed.
- Sensor status flags are not being updated correctly.
- The firmware is transmitting stale values.
- The dashboard is displaying cached or incorrectly parsed data.

**Solutions:**

1. Inspect the raw telemetry output in CoolTerm.
2. Check the `STATUS` field.
3. Run the I²C scanner.
4. Test individual sensors.
5. Verify the firmware's read functions.
6. Compare raw serial packets with the dashboard values.

Do not assume that changing the radio baud rate will fix sensor values that are already incorrect in the onboard serial output.

### Problem 4: Sensor Values Become `nan`

The firmware uses `nan` to represent unavailable or invalid floating-point readings.

**Possible causes:**

- Sensor initialization failed.
- A sensor read operation failed.
- A sensor is disconnected.
- The sensor address is incorrect.
- The I²C bus is not operating correctly.

**Solutions:**

1. Verify sensor power and wiring.
2. Check I²C addresses.
3. Inspect status flags.
4. Test sensors independently.
5. Reinitialize or recover the affected sensor in firmware.

### Problem 5: GPS Fix Is Zero

**Possible causes:**

- No satellite fix has been obtained.
- The GPS antenna does not have adequate sky visibility.
- UART wiring or baud rate is incorrect.
- NMEA sentences are not being received or parsed.

**Solutions:**

1. Check GPS TX-to-STM32 RX wiring.
2. Verify the configured baud rate.
3. Test the GPS receiver outdoors with a suitable antenna.
4. Confirm that valid NMEA sentences are arriving.

### Problem 6: Telemetry Packets Are Delayed

**Possible causes:**

- Blocking sensor conversion delays.
- Slow I²C communication.
- UART buffer or transmission constraints.
- Radio link errors.
- Dashboard parsing or rendering overhead.

**Solutions:**

1. Measure packet intervals using sequence numbers and timestamps.
2. Check onboard USB serial output separately from radio reception.
3. Measure sensor read duration.
4. Reduce unnecessary blocking operations.
5. Consider asynchronous or scheduled sensor acquisition.
6. Ensure that the firmware and dashboard agree on packet structure.

### Problem 7: STM32 Does Not Connect to ST-Link

**Possible causes:**

- Incorrect SWD wiring.
- Missing target power or ground.
- Incorrect programmer configuration.
- Firmware interfering with debugging.
- Readout protection or other device protection settings.

**Solutions:**

1. Verify PA13/SWDIO and PA14/SWCLK.
2. Confirm target voltage and common ground.
3. Use STM32CubeProgrammer with a low SWD frequency.
4. Try **Connect Under Reset** when appropriate.
5. Inspect the device's option bytes before changing protection settings.

**Warning:** Changing STM32 readout protection settings may erase flash contents. Do not change RDP levels without understanding the consequences.

---

## 16. Development Workflow

### Clone and Update

```powershell
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

To download updates later:

```powershell
git pull
```

### Check Repository Changes

```powershell
git status
```

### Stage and Commit Changes

```powershell
git add README.md
git commit -m "Update project documentation"
```

To stage all intended source changes:

```powershell
git add .
```

Review `git status` before committing to ensure generated files, local settings, and secrets are not included.

### Push to GitHub

```powershell
git push
```

If the repository has not been configured with a remote, follow GitHub's repository setup instructions before pushing.

### Suggested Commit Messages

```text
feat: add telemetry sensor support
fix: improve sensor initialization recovery
fix: correct telemetry packet parsing
feat: update JavaFX ground station
docs: improve project README
test: document I2C sensor verification
```

Use commit messages that accurately describe the changes.

---

## 17. Future Improvements

Potential future enhancements include:

- More robust sensor error recovery.
- Non-blocking sensor acquisition.
- Improved telemetry timing.
- Packet-loss and checksum error statistics.
- Live sensor plots.
- CSV telemetry logging.
- GPS position visualization.
- Configurable serial-port selection.
- Configurable telemetry baud rate.
- Battery voltage monitoring, when hardware support is implemented.
- Better diagnostics for I²C and UART failures.
- Unit tests for packet parsing and checksum validation.
- Automated builds using GitHub Actions.
- Hardware-in-the-loop testing.
- Improved ground-station reconnection handling.

These are proposed improvements, not claims that the features already exist.

---

## 18. Safety Considerations

TLV MK-01 is a telemetry development project. Its successful operation on a desktop or during a ground test does not guarantee safe rocket flight.

- Validate all electrical connections before powering the system.
- Verify the current and voltage requirements of each component.
- Secure electronics against vibration and mechanical loads.
- Keep antennas and radio modules within their specified operating limits.
- Comply with applicable Indian radio-frequency regulations.
- Follow the relevant rocket testing, launch-site, and range-safety requirements.
- Do not perform hazardous propulsion or flight tests without appropriate authorization, supervision, and safety controls.
- Maintain an independent recovery and safety plan appropriate to the test.
- Do not rely on a single telemetry link as the only means of ensuring flight safety.

---

## 19. Contributors

**Project:** TLV MK-01 Real-Time Telemetry System

**Organization:** Spaceverse Technologies Pvt. Ltd.

Contributors, roles, and institutional affiliations should be added here after confirming the final team list.

If you are maintaining this repository as a team, document:

- Embedded firmware development.
- Sensor integration.
- Radio communication.
- JavaFX ground station development.
- Testing and validation.
- Documentation and project coordination.

---

## 20. License

No license has been specified for this project yet.

Before publishing the repository for reuse, choose an appropriate license and add a `LICENSE` file.

If you intend to retain proprietary ownership of the firmware or ground station, do not add an open-source license without confirming that decision with the project owner or organization.

---

## Acknowledgements

This project brings together embedded systems, sensor integration, wireless communication, and desktop software development to support experimental rocket telemetry and atmospheric data acquisition.

Developed as part of the TLV MK-01 telemetry project associated with Spaceverse Technologies Pvt. Ltd.