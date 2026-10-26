# fixture-friend

A Kicad 10 design block and board implementation to help make bed-of-nails programming fixtures quickly. Intentionally implemented in this as two layers, so it's cheap and quick to produce assembled at JLCPCB. 

Features:

* 100mm x 100mm board outline
* M3 mounting holes in each corner
* Core RP2040 design as hierarchical sheet 
* Connections to target PCBA and target programming connectors implemented

Design block is fanned out to a few common connector specs, for easy implementation as a reference design:

* SWD: Samtec FTSH 10 pin (standard ARM)
* UART: JST-GH, 4–6 pin 
* I2C: QWIIC connector
* SPI: Digilent PMOD

Also included: USB-C to pogo pin interface, to easily land on a set of pogo pins. 

- Note: not tested with USB 2.0 high speed links. Only USB 1.1 operation is assured. 

