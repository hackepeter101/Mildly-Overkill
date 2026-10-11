# MildlyOverkill – Fix List (Rev 1 → Rev 1.1)

## ✅ Done (confirmed in netlist 2026-10-11 22:43)
- [x] R63 → pull-up to +3.3V (ST status readback now works)
- [x] R4 → 100k (gate network no longer burns; 100k is fine)
- [x] R8 / R11 → 2.37k (current trip ≈ 3 A, IPROPI stays below 3.3 V)
- [x] Q1 → AOD4185
- [x] D22 → B5819W (SOD-123 ✔)
- [x] FB1 → BLM21PG221SN1
- [x] C11 → 0805 (better than 0603)
- [x] D6/D7/D8/D10 → SOD-323 footprint
- [x] L1 → kept per TI design (only check Isat ≥ 2.5 A on the exact part)

---

## 🔴 Must do (BOM only, no layout work)
- [ ] **D4: SMAJ33A → SMAJ24CA** (bidirectional, same D_SMA footprint)
      - Starts clamping at ~27–29 V, before the TPS62932's 30 V limit
      - Reverse polarity no longer blows the fuse
- [ ] **U3: LD1117S33 → AP7361C-33ER-13** (SOT-223)
      - ⚠ Check the pinout in the datasheet: the "-ER" version should be GND/OUT(tab)/IN,
        the same as the LD1117. The non-R "-E" version is different!
      - C20 (22 µF) on the output covers its capacitor requirement
- [ ] **R1 / C1 (EN reset):** 5.1k / 100 nF → **10k / 1 µF**
- [ ] **R77:** value is still "R" → **10k 1%**
- [ ] **TH1:** generic → **10k NTC, B≈3380/3435** (e.g. NCP18XH103F03RB in 0603;
      it fits the 0805 hand-solder pads too)
- [ ] **F1:** no value → **7.5 A or 10 A mini blade fuse**

## 🆕 Footprint mismatches found in this netlist
- [ ] **D3 (BZT52C15):** BZT52 is **SOD-123**, footprint is 0805
      → use BZT52C15-SOD123 footprint, or switch the part to **BZT52C15S** (SOD-323)
- [ ] **D5 (B5819W):** B5819W is **SOD-123**, footprint is SOD-323
      → change footprint to SOD-123, or switch the part to **B5819WS** (SOD-323)
- [ ] **D6/D7/D8/D10:** value is still "3v" → give a real part, e.g. **BZT52C3V0S** / **MM3Z3V0**
      (both SOD-323, which matches the new footprint)

## 🟠 Voltage ratings (BOM fields: write the rating in!)
- [ ] All caps on VCC → **50 V**: C13, C14, C8, C9, C7, C15
- [ ] C6 / C18 (charge-pump caps) → **≥ 50 V** (pin swings to VM, DRV8874 datasheet says ≥ VM rating)
- [ ] C16 1000 µF → **≥ 35 V, low-ESR** (e.g. Panasonic FR / Nichicon HE)
- [ ] C11 47 µF 0805 → **10 V X5R** (expect ~15–20 µF effective at 5 V;
      add a second one if TI's design wants more Cout)
- [ ] C17 10 µF / C20 22 µF (0603) → **10 V X5R**

## 🟡 Value cleanup (so the parts can be ordered)
- [ ] R9 / R10: 660 → **665**
- [ ] R54: 400 → **402**
- [ ] R33/R35/R37/R39: 6k → **6.04k**
- [ ] R32/R34/R36/R38: 3k → **3.01k** (or 3.0k E24)
- [ ] R53 / R61: 210 → OK (E96), just make sure it's in the BOM
- [ ] R1/R3/R5: "5,1K" → **5.1k** (comma breaks some BOM tools)

## 🟠 Small layout additions (if there's room)
- [ ] **10 µF / 50 V (1206/1210)** at each DRV8874 VM pin (U8, U9); TI recommends bulk + 0.1 µF
- [ ] **100 nF + 1 µF / 50 V** close to U4 VS pins (TPS4H160)
- [ ] **2× 4.7k I2C pull-ups** SDA/SCL → 3.3 V (behind a solder jumper ideally)
- [ ] **1–4.7 µF** on VBUS after FB1
- [ ] (optional) 100–330 Ω series resistors on J10/J11 GPIOs

## ⚪ Skipped on purpose
- FAULT pins → no free GPIO; DRV8874 auto-retries on faults; watch IPROPI + NTC instead
  - (optional, no GPIO: BAT54A from both nFAULT pins + 2.2k into the ST node →
    any motor fault pulls CH7 below ~0.85 V)
- JTAG → built-in USB-Serial-JTAG over USB-C, nothing to add
- 60 V buck → not needed for 24 V / 6S with the SMAJ24CA

## 🧠 Firmware notes
- Ramp motor PWM down, no instant reversal at 6S (regen spikes vs 30 V buck limit)
- IPROPI only counts while the high-side FET is on → average / low-pass it under PWM
- Fault detection = current pinned at the limit + rising NTC temperature
- Battery voltage: no free channel → jumper VCC into an AIN terminal (1:11, ~36 V full scale)
- ST decode (CH7): 4095 = all OK; the lowest level ≈ 1445 (all 4 faulted); see lookup table
- Order an **ESP32-S3-WROOM-1 N8/N16 (3.3 V flash)**, NOT R16V / 1.8 V variants (IO47/48!)

## 🔍 Before ordering
- [ ] Run ERC + DRC
- [ ] Check Q1 pads match the AOD4185 (G=1, D=tab, S=3)
- [ ] Check the SMAJ24CA symbol (bidirectional, so orientation doesn't matter)
- [ ] Check L1 Isat ≥ 2.5 A for the exact WE-XHMI part number
- [ ] Re-export the BOM and spot-check that every part has a real MPN
