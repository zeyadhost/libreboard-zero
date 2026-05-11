# Reference Design Research

This folder is for local reference-design research material used while planning LibreBoard Zero.

Downloaded PDFs, screenshots, extracted text files, archives, and temporary research files should remain untracked unless their licenses clearly allow redistribution. Keep those files local for inspection.

Track public source links and review notes here instead of committing vendor or third-party documents.

## Public References

- MangoPi MQ / MQ Dual: https://www.mangopi.org/mangopi_mq
- MangoPi MQ v1.6 schematic: https://mangopi.org/_media/mq_sch_v1.6.pdf
- MangoPi MQ Dual v1.6 schematic: https://mangopi.org/_media/mq-dual_sch_v1.6.pdf
- MangoPi MQ-R: https://www.mangopi.org/mqr
- MangoPi MQ-R schematic: https://mangopi.org/_media/mq-r_sch_v1.0.pdf
- TinyEmbedded-Dual: https://github.com/yuansco/TinyEmbedded-Dual
- TinyEmbedded-Dual schematic: https://raw.githubusercontent.com/yuansco/TinyEmbedded-Dual/main/Document/TinyEmbedded_Dual_A.pdf
- T113-S3 datasheet: https://mangopi.org/_media/t113-s3_datasheet_v1.6.pdf
- T113-S3 user manual: https://mangopi.org/_media/t113-s3_user_manual_v1.3_.pdf

## Current Minimal-Boot Review Findings

- UART debug should use UART3 on PB6/PB7, not UART0.
- T113-S3 DDR3 DRAM rail should be treated as `VCC_DRAM_1V5` candidate, not `VCC_DRAM_1V8`.
- Do not copy D1s DDR2 1.8V assumptions into the T113-S3 design.
- `SEL0` appears on `PC4/MOSI/D2/SEL0`.
- `SEL1` appears on `PC5/MISO/D1/SEL1`.
- MangoPi MQ Dual shows `SEL[1:0] = 10` as `SD > NAND > Other`, but resistor population still needs verification.
- Observed reference power parts include `RY1303`, `SY8008`, and `SY8088`.
- `EA3036C` and `AXP2101` are only unverified research candidates.
- MangoPi uses 10k SD pullups.
- TinyEmbedded uses 4.7k SD pullups.
- MangoPi shows 33R series resistors on several SDC0 lines.
- USB-C CC pulldowns should be 5.1k.
- 24MHz clock references use about 20pF-22pF capacitors depending on the chosen crystal.

## Notes

These findings are planning notes only. They do not lock parts, rails, resistor values, or schematic implementation details. Verify everything against the T113-S3 datasheet, user manual, and selected component datasheets before creating a pin-level schematic.
