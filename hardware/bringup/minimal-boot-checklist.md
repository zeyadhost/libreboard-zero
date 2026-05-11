# LibreBoard Zero Minimal Boot Checklist

## Title and Scope

This checklist gates the path from the current planning schematic to a future pin-level schematic for the minimal boot milestone.

It is a planning/status checklist only. It is not a production checklist, does not imply manufacturability, and does not lock any final BOM or PCB decisions.

## Target Boot Path

USB-C 5V input -> protected `SYS_5V` -> correct power rails -> reset release -> T113-S3 boots from microSD -> UART serial console shows logs

## Current Design State

- [x] KiCad schematic exists as a planning-only schematic.
- [x] No real pins, production symbols, pin-level nets, or footprints exist yet.
- [x] No PCB layout exists yet.
- [x] No Gerbers or manufacturing files exist yet.
- [x] No final BOM exists yet.
- [x] Reference PDFs/text extracts are local research copies and ignored.
- [x] Tracked reference notes live in `hardware/datasheets/reference-designs/README.md`.

## Minimal Boot Blocks Checklist

### USB-C 5V Input

- [x] Planning decision: use USB-C as power-only 5V input for v0.
- [x] Planning decision: use 5.1k CC pulldowns on CC1 and CC2.
- [ ] Choose exact USB-C receptacle.
- [ ] Verify footprint and JLCPCB/LCSC availability.

### Input Protection / SYS_5V

- [x] Planning decision: protected input becomes `SYS_5V`.
- [ ] Choose fuse, resettable fuse, or current-limit switch.
- [ ] Choose proper VBUS-rated TVS/protection part.
- [ ] Choose bulk input capacitance.
- [ ] Verify protection parts are suitable for power input, not just USB data ESD.

### Power Rails

- [x] Planning decision: keep rail strategy explicit and reviewable.
- [x] Planning note: T113-S3 DDR3 DRAM should be treated as `VCC_DRAM_1V5` candidate.
- [ ] Verify exact T113-S3 rail names and voltage tolerances.
- [ ] Verify whether `VDD_SYS_1V0` is actually required as a separate rail.
- [ ] Verify regulator current requirement for each rail.
- [ ] Decide PMIC / multi-output regulator vs. discrete buck regulators.

### Reset / Power-Good Release

- [x] Planning decision: reset must not release until rails are stable.
- [ ] Verify `RESET_N` pull-up voltage domain.
- [ ] Choose reset supervisor, PMIC power-good, or RC/button reset approach.
- [ ] Verify reset timing against datasheet/reference designs.

### Boot Source / Boot Straps

- [x] Planning note: `SEL0` appears on `PC4/MOSI/D2/SEL0`.
- [x] Planning note: `SEL1` appears on `PC5/MISO/D1/SEL1`.
- [x] Reference note: MangoPi MQ Dual shows `SEL[1:0] = 10` as `SD > NAND > Other`.
- [ ] Verify exact boot strap resistor population.
- [ ] Verify exact boot order and FEL behavior in T113-S3 docs.

### microSD / SMHC0

- [x] Planning decision: use microSD on SMHC0 / SDC0 for first boot.
- [x] Reference note: MangoPi uses 10k SD pullups.
- [x] Reference note: TinyEmbedded uses 4.7k SD pullups.
- [x] Reference note: MangoPi shows 33R series resistors on several SDC0 lines.
- [ ] Choose final CMD/DAT pullup values.
- [ ] Decide whether to use 22R-33R series resistors near the SoC.
- [ ] Choose exact microSD socket.
- [ ] Decide whether card detect is required for v0.

### UART Debug

- [x] Planning decision: UART debug should use UART3 on PB6/PB7, not UART0.
- [x] Planning decision: UART debug is 3.3V logic only.
- [x] Planning decision: start with a simple 4-pin debug header.
- [ ] Verify final header pin order.
- [ ] Confirm UART3 console configuration in bootloader/software notes.

### 24MHz Clock

- [x] Planning decision: provide a 24MHz clock close to the SoC.
- [x] Reference note: capacitor examples are around 20pF-22pF depending on board/crystal.
- [ ] Choose exact 24MHz crystal or oscillator.
- [ ] Calculate load capacitors from the chosen crystal datasheet and PCB assumptions.
- [ ] Decide whether 32.768kHz RTC crystal is needed for v0.

### Test Points

- [x] Planning decision: expose important rails and debug signals for bring-up.
- [ ] Add test points for `SYS_5V`.
- [ ] Add test points for all required regulator outputs.
- [ ] Add test point for `RESET_N`.
- [ ] Add test points for UART3 TX/RX.
- [ ] Add test point for `SMHC0_CLK` / `SDC0_CLK`.

### Future Expansion Header

- [x] Planning decision: future expansion header should remain unpopulated for v0.
- [ ] Review pinmux before exposing signals.
- [ ] Avoid exposing boot-critical pins casually.
- [ ] Avoid exposing unknown-voltage or 1.8V-only pins as if they are 3.3V GPIO.

## Known Planning Decisions

- UART debug should use UART3 on PB6/PB7, not UART0.
- UART debug is 3.3V only and should not be treated as 5V tolerant unless verified.
- T113-S3 DDR3 DRAM rail should be treated as `VCC_DRAM_1V5` candidate, not `VCC_DRAM_1V8`.
- Do not copy D1s DDR2 1.8V assumptions into the T113-S3 design.
- `SEL0` appears on `PC4/MOSI/D2/SEL0`.
- `SEL1` appears on `PC5/MISO/D1/SEL1`.
- USB-C CC pulldowns should be 5.1k.
- 24MHz crystal capacitor values are expected around 20pF-22pF depending on the chosen crystal.

## Candidate / Needs-Verification Items

- [ ] Exact T113-S3 power rail names and voltage tolerances.
- [ ] Whether `VCC_DRAM_1V5` is the final correct DRAM rail implementation for the selected T113-S3 package/module assumptions.
- [ ] Power sequencing requirements and whether a PMIC/multi-output regulator or discrete regulators are preferred.
- [ ] Exact reset supervisor or reset/power-good implementation.
- [ ] `SEL0` / `SEL1` resistor population for SD-first boot.
- [ ] MangoPi MQ Dual shows `SEL[1:0] = 10` as `SD > NAND > Other`, but resistor population still needs verification.
- [ ] microSD pullup choice: MangoPi uses 10k, TinyEmbedded uses 4.7k.
- [ ] Whether to use 33R series resistors on SDC0 lines.
- [ ] Final 24MHz crystal and load capacitor calculation.
- [ ] Final regulator part choices.

## Observed Reference Parts

- `RY1303`: observed in MangoPi MQ Dual as a main multi-output regulator candidate.
- `SY8008`: observed in MangoPi MQ-R as a discrete buck regulator candidate.
- `SY8088`: observed in TinyEmbedded-Dual as a discrete buck regulator candidate.
- `EA3036C`: unverified research candidate only, not selected.
- `AXP2101`: unverified research candidate only, not selected.
- `XC6206`: observed in reference designs as an LDO family to study.

## Do-Not-Do-Yet Boundaries

- Do not create PCB layout yet.
- Do not generate Gerbers.
- Do not create a final BOM.
- Do not claim production readiness.
- Do not add real schematic symbols or pin-level nets until open verification items are resolved.
- Do not commit downloaded reference PDFs unless licensing/source policy allows redistribution.

## Pre-Schematic Exit Criteria

Before starting real pin-level schematic work:

- [ ] T113-S3 power pins and rail requirements verified against datasheet/user manual.
- [ ] Boot strap pins and resistor populations verified.
- [ ] microSD/SMHC0 required pins and passive network verified.
- [ ] UART3 PB6/PB7 routing and voltage level confirmed.
- [ ] 24MHz crystal requirements verified.
- [ ] Reset sequencing approach chosen.
- [ ] Regulator topology chosen.
- [ ] Test point strategy confirmed.
