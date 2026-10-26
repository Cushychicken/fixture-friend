# fixture-friend

A Kicad 10 design block and board implementation to help make bed-of-nails programming fixtures quickly. Intentionally implemented in this as two layers, so it's cheap and quick to produce assembled at JLCPCB. 

Features:

* 100mm x 100mm board outline
* M3 mounting holes in each corner
* Core RP2040 design as hierarchical sheet 
* Connections to target PCBA and target programming connectors implemented

Design block is fanned out to a few common connector specs, for easy implementation as a reference design:

* SWD: Samtec FTSH 10 pin (standard ARM)
* UART: Molex PicoBlade 4-pin debug connector (keyed differently from QWIIC)
* I2C: QWIIC connector
* SPI: JST-SH 6-pin debug connector

Also included: USB-C to pogo pin interface, to easily land on a set of pogo pins. 

- Note: not tested with USB 2.0 high speed links. Only USB 1.1 operation is assured. 

## RP2040 debug interfaces

The dedicated debug connectors are on the root schematic and are exported from the
`ff-core` hierarchical sheet. They use unassigned RP2040 GPIOs and the following
alternate functions:

| Interface | Connector | Pin order | RP2040 GPIO | RP2040 function |
| --- | --- | --- | --- | --- |
| UART | J8, Molex PicoBlade 53261-0471 | 1 GND, 2 VTREF (+3V3), 3 TX, 4 RX | GPIO4, GPIO5 | UART1 TX (probe to target RX), UART1 RX (target TX to probe) |
| SPI | J9, JST SM06B-SRSS-TB(LF)(SN) | 1 GND, 2 VTREF (+3V3), 3 SCLK, 4 MOSI, 5 MISO, 6 CS_N | GPIO10, GPIO11, GPIO8, GPIO9 | SPI1 SCK, TX, RX, CSn |

J8 uses the KiCad `Connector_Molex` right-angle, 1.25 mm pitch PicoBlade footprint;
its incompatible keying prevents a QWIIC cable from being plugged into UART. J9 uses
the KiCad `Connector_JST` right-angle, 1.00 mm pitch JST-SH footprint. Both are
grouped along accessible board edges. VTREF is a 3.3 V reference output; it is not
intended as an external power input.

The on-board RP2040 is intended to run Raspberry Pi Debug Probe firmware. Its target
SWD engine is GPIO12 (SWCLK drive), GPIO13 (SWD sample input), and GPIO14
(SWD drive); GPIO4/GPIO5 are the firmware's UART1 TX/RX pins. This implementation
is intentionally limited to 3.3 V targets, so GPIO13 and GPIO14 share the target
SWDIO net directly. The RP2040 package
pins named SWCLK and SWDIO are reserved for programming this probe itself and are
not the target-programmer outputs.

