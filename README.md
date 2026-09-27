# FPGA Carrier Board

KiCad 10 design for an FPGA carrier board built around a Xilinx Artix-7
XC7A35T-FGG484.

## Status

Schematic complete, ERC clean (0 errors), BOM complete (56 lines). PCB
layout has not started yet.

## Sheets

| Sheet file | Contents |
|---|---|
| `powerrails.kicad_sch` | 4x TPS62130A regulators (1V0, 1V8, 1V2, 3V3) |
| `powerin.kicad_sch` | USB-C input, eFuse (TPS259251), protection |
| `fpgacore.kicad_sch` | XC7A35T-FGG484 (7 units A-G), MGT power filtering |
| `dcapacitors.kicad_sch` | FPGA decoupling capacitors (nested under FPGA Core) |
| `config_jtag.kicad_sch` | SPI flash (W25Q128), JTAG header, boot straps |
| `clocks.kicad_sch` | Y1 50 MHz system clock |
| `ethernet1g.kicad_sch` | RTL8211E-VB-CG PHY, RJ45 (HR911130C), Y3 25 MHz oscillator |
| `sfplane.kicad_sch` | Samtec SFPK-SL (SFP+ cage/connector kit), AC-coupling caps |
| `iobreakout.kicad_sch` | LEDs, buttons, spare-I/O header |

## Key parts (LCSC)

| Ref | Part | LCSC |
|---|---|---|
| U1-4 | TPS62130A | C337502 |
| U5 | eFuse | C527680 |
| U6 | XC7A35T-FGG484 | C6934285 |
| U7 | W25Q128 flash | C97521 |
| U8 | RTL8211E-VB-CG | C90735 |
| USB1 | USB-C connector | C456012 |
| J3 | RJ45 (HR911130C) | C50933 |
| J4 | SFPK-SL | C20691304 |
| L1-4 | Inductor | C719179 |
| Y1 | 50 MHz oscillator | C717684 |
| Y3 | 25 MHz oscillator | C669088 |
| J2 | SMA connector | C2693814 |

## Assembly split

JLC handles placement of ICs, the FPGA BGA, QFN parts, and decoupling
capacitors. Connectors, resistors, LEDs, switches, and test points are
hand-soldered.

## Generating the BOM

The GUI "Generate Bill of Materials" and Symbol Fields Table exporters are
unreliable in this KiCad build and can silently produce empty or partial
output. Use `kicad-cli` instead:

```bash
kicad-cli sch export bom \
  --output bom_cli.csv \
  --fields "Reference,Value,Footprint,QUANTITY,LCSC Part" \
  --labels "Reference,Value,Footprint,Qty,LCSC" \
  --group-by "Value,Footprint" \
  fpgacarrierboard.kicad_sch
```

## Libraries

Custom/imported symbol and footprint libraries live in `~/kicad-libs/`
(EasyEDA imports, Samtec parts, SFPK-SL, etc.), registered in the local
`sym-lib-table` and `fp-lib-table`.

## Next step

PCB layout: board outline, 6-layer stackup for BGA fanout and
controlled-impedance pairs (RGMII, SFP+ differential pairs), part
placement, and routing.
