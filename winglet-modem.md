# Winglet modem

This is a hardware standard to enable multiple modem support for embedded platforms, while maintaining low power support. The modem is in a mini-PCIe form factor. They are loosely compatible with other PCIe form factor modems.

The generalized pinout is below:

| Function    | Pin | Pin | Function   |
| ----------- | --- | --- | ---------- |
| WAKE        | 1   | 2   | VDDIO      |
| *AUX_RX*    | 3   | 4   | GND        |
| *AUX_TX*    | 5   | 6   | -          |
| -           | 7   | 8   | -          |
| GND         | 9   | 10  | -          |
| RX          | 11  | 12  | -          |
| TX          | 13  | 14  | -          |
| GND         | 15  | 16  | -          |
| -           | 17  | 18  | GND        |
| -           | 19  | 20  | -          |
| GND         | 21  | 22  | RESET      |
| -           | 23  | 24  | *USB_VBUS* |
| -           | 25  | 26  | GND        |
| GND         | 27  | 28  | -          |
| GND         | 29  | 30  | I2C_SCL    |
| DTR         | 31  | 32  | I2C_SDA    |
| DCD         | 33  | 34  | GND        |
| GND         | 35  | 36  | *USB_DM*   |
| GND         | 37  | 38  | *USB_DP*   |
| VDD         | 39  | 40  | GND        |
| VDD         | 41  | 42  | *NETLIGHT* |
| GND         | 43  | 44  | -          |
| *PROG_DATA* | 45  | 46  | -          |
| *PROG_CLK*  | 47  | 48  | -          |
| *PROG_NRST* | 49  | 50  | GND        |
| *PROG_VDD*  | 51  | 52  | VDD        |

Not all pins are available on all modems, and only a subset are reccomended to be connected.
The specific behavior of many of the control pins is dependent on the specific modem installed.

# Power supplies

It is not guaranteed that all modems will have identical supply requirements.
3.3V will always be supported on all rails.

| Rail      | V min | V Max | I Max | Description |
| --------- | ----- | ----- | ----- | ----------- |
| VDD       | 2.8V  | 4.2V  | 600mA | Power input. Intended to support direct connection to a battery. |
| VDDIO     | 1.7V  | 3.6V  | 20mA  | Voltage reference for IO pins. |

# Signals

The specific function of each pin will depend on the specific modem.

| Pin       | Reccommended | Direction | Description |
| --------- | ------------ |---------- | ----------- |
| WAKE      | Yes          | Input     | Wakes the modem from off/sleep modes |
| RESET     | Yes          | Input     | Resets the modem |
| TX        | Yes          | Output    | Modem control UART transmit |
| RX        | Yes          | Input     | Modem control UART receive |
| DCD       | Yes          | Output    | Indicates modem and ready for commands |
| DTR       | Yes          | Input     | Signals when modem may enter sleep modes |
| NETLIGHT  |              | Output    | Active high output to indicate network activity. |
| I2C_SCL   | Yes          | I/O       | I2C clock line |
| I2C_SDA   | Yes          | I/O       | I2C data line |
| AUX_RX    |              | Input     | Auxilary UART receive |
| AUX_TX    |              | Output    | Auxilary UART transmit |
| USB_VBUS  |              | Input     | USB 5V input for USB detection |
| USB_DM    |              | I/O       | USB data negative line |
| USB_DP    |              | I/O       | USB data positive line | 
| PROG_VDD  |              | Output    | Programmer VDD reference |
| PROG_NRST |              | Input     | Programmer active low reset |
| PROG_CLK  |              | Input     | Programmer clock line |
| PROG_DATA |              | I/O       | Programmer data line |

# Device detecton
An 24AA01T-I/OT EEPROM is available on the I2C bus. The contents of this eeprom can be used to identify the connected modem type. The winglet modem will not include I2C pullups.

All other pins should remain high-z until the modem type is known, and the appropriate software behavior can be determined.

# Optional interfaces

Thiese interfaces may or may not be implemented on any specific modem. These are primarially intended for programming and debugging purposes.

## Netlight
An output signal to drive a network activity LED. No particular drive current is guaranteed, so a transistor is required if this pin is to be used.

## Auxilary UART
This is an auxilary UART which may be configured as a GPS NMEA stream on some modems. Usually this functionality is available from the control UART, so use of this is not reccommended.

## USB Pins
The USB interface is provided for some modems. This primarially is to facilitate firmware updates.

## Programming pins
These pins are reserved for connection to a debugger, such as serial wire debug.
