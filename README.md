KiwiEFI Dual LM1815 Toyota VR Conditioner for 24+1 tooth distributor type sensors, works with both shared ground and separate isolated sensors.

Also suitable for:
- 12 tooth crank + 1 tooth cam sensors as used in the non-vvt 1UZFE
- Mazda RX7 S4,5,6 13B and 20B sensors
- Should also work with Honda 24 + 1 dissy sensors but not tested
- 

![TOYOTA_VR_v1](https://github.com/Neil427/KiwiEFI-ToyotaVR/assets/67580691/6198988d-7bd4-459c-b521-f22a4d1509c5)

Note1: This LM1815 design is not a universal VR conditioner, the components are optimised for the pulse rate of the 24 / 1 tooth rates. 
Other pulse rates will require changes to the input filtering, one shot, and peak detector components.

Note2: V0.1 Gerbers are the original version V1.0 Gerbers have silkscreen update only.

---

## Fork notes: component values and JLCPCB assembly files

Values for the two trigger setups. The 24+1 column follows the original component placement sheet, the 36-1 column follows `ComponentPlacement_(36-1CNK+1CAM).jpg`. Entries marked **(changed)** differ from those sheets.

| Function | Ref | 24+1 | 36-1 crank + 1 cam |
|---|---|---|---|
| Crank input series resistor | R11 | 22k | 56k |
| Crank input filter | C11 / C12 | 10n / 10n | 1n / 1n |
| Crank peak detector | C13 / R12 | 470n / 1M5 | 10n / 1M5 |
| Crank one-shot | R13 / C14 | 150k / 1n | **82k (changed)** / 1n |
| Crank output pull-up / filter | R14 / C15 | **2k2 (changed)** / **1n (changed)** | **2k2 (changed)** / 1n |
| Cam input series resistor | R1 | 22k | 22k |
| Cam input filter | C1 / C2 | 47n / 47n | 47n / 47n |
| Cam peak detector | C3 / R2 | 470n / 1M5 | 470n / 1M5 |
| Cam one-shot | R3 / C4 | 150k / 1n | 150k / 1n |
| Cam output pull-up / filter | R4 / C5 | **2k2 (changed)** / 10n | **2k2 (changed)** / 10n |
| Decoupling | C6, C8, C16 / C7, C17 / C9 | 100n / 1u / 10u | same |
| LEDs | R5, R6 / D1, D2 | 1k / red 0805 | same |

Why the changes:

- **R13 = 82k on 36-1.** The LM1815 one-shot sets the maximum input frequency, Fmax = 1 / (1.346 x R x C). 150k / 1n gives about 4.95 kHz, which is 8250 rpm on a 36-tooth crank wheel, and the datasheet pulse width tolerance (70 to 130 us) can pull that down to about 6400 rpm. 82k / 1n gives about 55 us and 9 kHz. On 24+1 the tooth frequency is only 1600 Hz at 8000 rpm, so 150k stays.
- **R14, R4 = 2k2.** Pin 12 is an open-collector output specified at 2 mA sink; 1k to 5 V asks for about 5 mA.
- **C15 = 1n on 24+1.** Shortens the pulse edge delay through the pull-up.

These values are calculated, not bench tested.

### `jlcpcb/` folder

| File | Use |
|---|---|
| `BOM_ToyotaVR_24+1.csv` | BOM for the 24+1 build |
| `BOM_ToyotaVR_36-1.csv` | BOM for the 36-1 crank + 1 cam build |
| `CPL_ToyotaVR_V1.0.csv` | Pick and place file, same for both builds |

Upload `KiwiEFI_ToyotaVR_V1.0GERBERS.zip` as the PCB, then one BOM and the CPL in the assembly step (top side).

Things to check before ordering:

- The CPL was rebuilt from the paste layer of the V1.0 gerbers, because the repository has no KiCad source. Check every part in the JLCPCB placement preview, in particular the pin 1 dot of U1, U2, U3 and the cathode of D1, D2 (cathode is on the board-edge side).
- U1/U3 rotation is 270 and U2 is 180, which already includes the usual +270 offset for JLCPCB SOIC parts.
- J1 (2x4 pin header) is through-hole and is not in the BOM or CPL.
- LM1815MX/NOPB (C2878578) is not a stocked basic part and stock is low, so it may need to be pre-ordered.
- The LCSC number for the 82k resistor (C17840) was not confirmed against the part page; check the description JLCPCB shows for it.
