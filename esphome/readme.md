# ESPHome – Valence U-BMS CAN Decoder

This folder contains ESPHome configuration and custom code used to read **raw CANBus data** from a **Valence U-BMS** and publish the decoded battery values into **Home Assistant**.

The ESP32 listens directly to the U-BMS CAN lines, parses the Valence-specific CAN frames, and exposes sensors such as voltage, current, SOC, temperatures, and fault states.

## What This Does

- Connects an ESP32 to a Valence U-BMS using a standard CAN transceiver
- Captures raw CAN frames broadcast by the U-BMS
- Decodes Valence-specific message formats
- Publishes all fields as ESPHome sensors
- Sends values to Home Assistant through the ESPHome API
- Provides logging to inspect raw frames during development/debugging

## Metrics Decoded

Depending on the messages present on the CAN bus, the decoder extracts:

- Pack voltage  
- Pack current  
- State of Charge (SOC)  
- Module temperatures  
- Cell minimum/maximum voltages  
- Balance status  
- Fault bits / alarms  
- General U-BMS status messages  

## Hardware Needed

- ESP32 (required for native CAN support)
- CAN transceiver (SN65HVD230, TJA1050, MCP2551, etc.)
- Twisted-pair wiring to the U-BMS CAN port
- Proper CAN termination if needed

## Folder Contents

esphome/  
├── *.yaml                 (ESPHome device configs)  
└── custom components      (CANBus decoder for Valence U-BMS)

## How It Works

1. ESPHome config sets up the CAN interface on the ESP32.  
2. A custom component subscribes to incoming CAN frames.  
3. Incoming frames are matched against known Valence U-BMS CAN IDs.  
4. Payload bytes are decoded, scaled, and converted into real-world values.  
5. ESPHome publishes the decoded values as sensors which appear in Home Assistant.  

## Notes

- Valence uses a proprietary CAN structure, so decoding is implemented manually.
- Additional message IDs can be supported by extending the decoder.
- This project focuses on **read-only** CAN traffic for safety.
