# Smart Irrigation System - Detailed Block Diagram

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    SMART IRRIGATION SYSTEM                                        │
│                                    ARM LPC2129 MICROCONTROLLER                                   │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## Complete System Block Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    POWER SUPPLY & REGULATION                                     │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                      │
│  │   12V DC    │───▶│   Voltage   │───▶│    5V DC    │───▶│  3.3V DC    │                      │
│  │   Input     │    │  Regulator  │    │   Supply    │    │   Supply    │                      │
│  └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘                      │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    SENSOR INPUT MODULE                                           │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐                              │
│  │   Soil Moisture │    │   DHT22 Temp &  │    │   Push Button   │                              │
│  │     Sensor      │    │   Humidity      │    │     Interface   │                              │
│  │                 │    │     Sensor      │    │                 │                              │
│  │  ┌───────────┐  │    │  ┌───────────┐  │    │  ┌───────────┐  │                              │
│  │  │   Analog  │  │    │  │  Digital  │  │    │  │   Digital │  │                              │
│  │  │   0-5V    │  │    │  │  1-Wire   │  │    │  │   Input   │  │                              │
│  │  │   Output  │  │    │  │ Protocol  │  │    │  │  (3x)     │  │                              │
│  │  └───────────┘  │    │  └───────────┘  │    │  └───────────┘  │                              │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘                              │
│           │                        │                        │                                    │
│           ▼                        ▼                        ▼                                    │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐                              │
│  │   ADC Channel   │    │   GPIO P0.3     │    │  GPIO P0.0-P0.2 │                              │
│  │     AD0.2       │    │   (DHT22 Pin)   │    │  (Button Pins)  │                              │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘                              │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    ARM LPC2129 MICROCONTROLLER                                   │
│  ┌─────────────────────────────────────────────────────────────────────────────────────────────┐ │
│  │                             32-bit ARM7TDMI-S Core (60MHz)                                 │ │
│  │                                                                                             │ │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐        │ │
│  │  │   ADC Module    │  │   GPIO Module   │  │   I2C Module    │  │   UART Module   │        │ │
│  │  │                 │  │                 │  │                 │  │                 │        │ │
│  │  │ • 10-bit ADC    │  │ • 32 GPIO Pins  │  │ • I2C0 Interface│  │ • UART0 (9600)  │        │ │
│  │  │ • 8 Channels    │  │ • Configurable  │  │ • 100kHz Clock  │  │ • 8-bit Data    │        │ │
│  │  │ • 0-1023 Range  │  │ • Input/Output  │  │ • Master Mode   │  │ • 1 Stop Bit    │        │ │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘        │ │
│  │                                                                                             │ │
│  │  ┌─────────────────────────────────────────────────────────────────────────────────────────┐ │ │
│  │  │                        CONTROL & PROCESSING LOGIC                                       │ │ │
│  │  │                                                                                         │ │ │
│  │  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐    │ │ │
│  │  │  │   Sensor Data   │  │   Control       │  │   Communication │  │   User Interface│    │ │ │
│  │  │  │   Processing    │  │   Algorithm     │  │   Protocol      │  │   Management    │    │ │ │
│  │  │  │                 │  │                 │  │                 │  │                 │    │ │ │
│  │  │  │ • ADC Reading   │  │ • Threshold     │  │ • I2C Commands  │  │ • Button Debounce│    │ │ │
│  │  │  │ • DHT22 Parse   │  │   Logic         │  │ • UART Commands │  │ • Menu Navigation│    │ │ │
│  │  │  │ • Data Validation│  │ • Mode Control  │  │ • WiFi Protocol │  │ • LCD Display   │    │ │ │
│  │  │  │ • Error Check   │  │ • Pump Control  │  │ • Data Logging  │  │ • Status Update  │    │ │ │
│  │  │  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘    │ │ │
│  │  └─────────────────────────────────────────────────────────────────────────────────────────┘ │ │
│  └─────────────────────────────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    OUTPUT CONTROL MODULE                                         │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐                              │
│  │   LCD Display   │    │   Relay Module  │    │   WiFi Module   │                              │
│  │                 │    │                 │    │                 │                              │
│  │  ┌───────────┐  │    │  ┌───────────┐  │    │  ┌───────────┐  │                              │
│  │  │   16x2    │  │    │  │   5V      │  │    │  │  ESP8266   │  │                              │
│  │  │   LCD     │  │    │  │  Relay    │  │    │  │   WiFi     │  │                              │
│  │  │  Display  │  │    │  │  Module   │  │    │  │  Module    │  │                              │
│  │  └───────────┘  │    │  └───────────┘  │    │  └───────────┘  │                              │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘                              │
│           │                        │                        │                                    │
│           ▼                        ▼                        ▼                                    │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐                              │
│  │  4-bit Interface│    │   GPIO P1.16    │    │   UART P0.6-P0.7│                              │
│  │  P1.17-P1.19    │    │   (Relay Pin)   │    │  (WiFi Pins)    │                              │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘                              │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    IRRIGATION ACTUATOR                                           │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐                              │
│  │   Water Pump    │    │  Solenoid Valve │    │   Irrigation    │                              │
│  │                 │    │                 │    │     System      │                              │
│  │  ┌───────────┐  │    │  ┌───────────┐  │    │  ┌───────────┐  │                              │
│  │  │   High    │  │    │  │   Flow    │  │    │  │   Field   │  │                              │
│  │  │  Power    │  │    │  │  Control  │  │    │  │ Irrigation│  │                              │
│  │  │   Pump    │  │    │  │   Valve   │  │    │  │   Zone    │  │                              │
│  │  └───────────┘  │    │  └───────────┘  │    │  └───────────┘  │                              │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘                              │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## Data Flow Diagram

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Sensor    │───▶│   ADC/I2C   │───▶│  Data       │───▶│  Control    │───▶│   Actuator  │
│   Input     │    │   Interface │    │  Processing │    │  Algorithm  │    │   Output    │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │                   │                   │
       ▼                   ▼                   ▼                   ▼                   ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ • Soil      │    │ • 10-bit    │    │ • Threshold │    │ • AUTO Mode │    │ • Pump ON   │
│   Moisture  │    │   ADC       │    │   Check     │    │ • MANUAL    │    │ • Pump OFF  │
│ • Temp      │    │ • I2C Read  │    │ • Mode      │    │   Mode      │    │ • LCD       │
│   Humidity  │    │ • UART Rx   │    │   Logic     │    │ • SCHEDULED │    │   Display   │
│ • Buttons   │    │ • GPIO Read │    │ • Error     │    │   Mode      │    │ • WiFi      │
│             │    │             │    │   Detection │    │ • Emergency │    │   Data      │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

## Control Algorithm Flow

```
                    ┌─────────────┐
                    │   START     │
                    └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ Initialize  │
                    │   System    │
                    └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ Read Sensors│
                    │ • Moisture  │
                    │ • Temp      │
                    │ • Humidity  │
                    └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ Check Mode  │
                    └─────────────┘
                           │
                    ┌──────┴──────┐
                   │              │
                   ▼              ▼
            ┌─────────────┐  ┌─────────────┐
            │   AUTO      │  │   MANUAL    │
            │   MODE      │  │   MODE      │
            └─────────────┘  └─────────────┘
                   │              │
                   ▼              ▼
            ┌─────────────┐  ┌─────────────┐
            │ Threshold   │  │ Button      │
            │ Check       │  │ Control     │
            └─────────────┘  └─────────────┘
                   │              │
                   ▼              ▼
            ┌─────────────┐  ┌─────────────┐
            │ Moisture <  │  │ Pump ON/OFF │
            │ 30% ?       │  │ Based on    │
            └─────────────┘  │ Button      │
                   │         └─────────────┘
            ┌──────┴──────┐
           │              │
           ▼              ▼
    ┌─────────────┐  ┌─────────────┐
    │   PUMP ON   │  │   PUMP OFF  │
    └─────────────┘  └─────────────┘
           │              │
           └──────┬───────┘
                  ▼
            ┌─────────────┐
            │ Update      │
            │ Display     │
            └─────────────┘
                  │
                  ▼
            ┌─────────────┐
            │ Send Data   │
            │ via WiFi    │
            └─────────────┘
                  │
                  ▼
            ┌─────────────┐
            │   DELAY     │
            │   1 Second  │
            └─────────────┘
                  │
                  ▼
            ┌─────────────┐
            │   LOOP      │
            └─────────────┘
```

## Communication Protocol Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    COMMUNICATION LAYERS                                          │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    APPLICATION LAYER                                             │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐      │
│  │   Irrigation    │    │   Data Logging  │    │   Remote        │    │   System        │      │
│  │   Control       │    │   & Monitoring  │    │   Monitoring    │    │   Diagnostics   │      │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘      │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    PROTOCOL LAYER                                                │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐      │
│  │   I2C Protocol  │    │   UART Protocol │    │   GPIO Protocol │    │   ADC Protocol  │      │
│  │   (RTC)         │    │   (WiFi)        │    │   (LCD/Relay)   │    │   (Moisture)    │      │
│  │   • 100kHz      │    │   • 9600 baud   │    │   • Digital I/O │    │   • 10-bit      │      │
│  │   • Master Mode │    │   • 8N1         │    │   • Configurable│    │   • 0-1023      │      │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘      │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────────────────────────┘
│                                    PHYSICAL LAYER                                                │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐      │
│  │   I2C Pins      │    │   UART Pins     │    │   GPIO Pins     │    │   ADC Pins      │      │
│  │   P0.4 (SCL)    │    │   P0.6 (TXD)    │    │   P1.16-P1.19   │    │   AD0.2         │      │
│  │   P0.5 (SDA)    │    │   P0.7 (RXD)    │    │   P0.0-P0.3     │    │   (Moisture)    │      │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘      │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## Pin Assignment Matrix

| Pin | Function | Component | Direction | Description |
|-----|----------|-----------|-----------|-------------|
| P0.0 | GPIO | Button 1 | Input | Manual Override |
| P0.1 | GPIO | Button 2 | Input | Mode Selection |
| P0.2 | GPIO | Button 3 | Input | Parameter Adjustment |
| P0.3 | GPIO | DHT22 | I/O | Temperature/Humidity Sensor |
| P0.4 | I2C SCL | RTC | I/O | Real-Time Clock |
| P0.5 | I2C SDA | RTC | I/O | Real-Time Clock |
| P0.6 | UART TXD | WiFi | Output | WiFi Communication |
| P0.7 | UART RXD | WiFi | Input | WiFi Communication |
| P1.16 | GPIO | Relay | Output | Water Pump Control |
| P1.17 | GPIO | LCD RS | Output | LCD Register Select |
| P1.18 | GPIO | LCD R/W | Output | LCD Read/Write |
| P1.19 | GPIO | LCD EN | Output | LCD Enable |
| AD0.2 | ADC | Moisture Sensor | Input | Soil Moisture Reading |

## System Timing Diagram

```
Time (ms)    0    100   200   300   400   500   600   700   800   900   1000
             │     │     │     │     │     │     │     │     │     │     │
Sensor Read  │─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────
             │     │     │     │     │     │     │     │     │     │     │
Control      │     │─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────
Logic        │     │     │     │     │     │     │     │     │     │     │
             │     │     │     │     │     │     │     │     │     │     │
LCD Update   │     │     │─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────
             │     │     │     │     │     │     │     │     │     │     │
WiFi Data    │     │     │     │─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────
             │     │     │     │     │     │     │     │     │     │     │
Button Check │     │     │     │     │─────┼─────┼─────┼─────┼─────┼─────┼─────
             │     │     │     │     │     │     │     │     │     │     │
System Loop  │─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────
```

## Error Handling Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    ERROR DETECTION & HANDLING                                    │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘

┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Sensor    │───▶│   Range     │───▶│   Error     │───▶│   Error     │───▶│   Recovery   │
│   Reading   │    │   Check     │    │   Detection │    │   Logging   │    │   Action    │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │                   │                   │
       ▼                   ▼                   ▼                   ▼                   ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ • ADC       │    │ • Moisture  │    │ • Out of    │    │ • LCD Error │    │ • Retry     │
│   Value     │    │   0-100%    │    │   Range     │    │   Display   │    │ • Default   │
│ • Temp      │    │ • Temp      │    │ • Sensor    │    │ • WiFi      │    │   Values    │
│   -10°C to  │    │   -10°C to  │    │   Failure   │    │   Alert     │    │ • Safe Mode │
│   +60°C     │    │   +60°C     │    │ • Comm      │    │ • Data Log  │    │ • Restart   │
│ • Humidity  │    │ • Humidity  │    │   Error     │    │ • Timestamp │    │   System    │
│   0-100%    │    │   0-100%    │    │             │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

## Performance Specifications

| Parameter | Specification | Unit |
|-----------|---------------|------|
| **Processing Speed** | 60 MHz | Hz |
| **ADC Resolution** | 10-bit | bits |
| **ADC Sampling Rate** | 1 sample/second | samples/sec |
| **I2C Clock Frequency** | 100 kHz | Hz |
| **UART Baud Rate** | 9600 | bps |
| **Response Time** | < 1 | seconds |
| **Data Logging Interval** | 15 | minutes |
| **Operating Temperature** | -10 to +60 | °C |
| **Power Consumption** | < 2 | Watts |
| **Moisture Accuracy** | ±2 | % |
| **Temperature Accuracy** | ±0.5 | °C |
| **Humidity Accuracy** | ±2-5 | % RH |

## System Integration Points

1. **Sensor Integration**: Analog and digital sensors with proper signal conditioning
2. **Communication Integration**: I2C, UART, and GPIO protocols
3. **Power Integration**: Multiple voltage levels with proper regulation
4. **User Interface Integration**: LCD display and push button controls
5. **Actuator Integration**: Relay control for high-power devices
6. **Remote Monitoring Integration**: WiFi connectivity for cloud data

This comprehensive block diagram provides a complete technical overview of the Smart Irrigation System for your project synopsis.
