# Batman Cowl Technology System - Complete Specification

## 1. System Overview

The Batman cowl is engineered as a **three-layer nested shell** containing a full sensor and communication suite distributed across the head, neck, and belt.

### Design Philosophy
- **Head-scan driven:** Layout is determined by head anatomy, not silhouette
- **Modular:** Each system (radio, optics, sensors, power) is independent
- **Distributed:** Heavy compute and power are in the belt; cowl is a peripheral
- **Failsafe:** 2-second removal, no sealed components, active thermal management
- **Real:** Every component exists at scale today (2026)

---

## 2. Shell Architecture

### Layer 1: Outer Skin
**Function:** Aesthetic, weather protection, impact deflection  
**Thickness:** 1.5–3 mm  
**Material Options:**
- **PETG (prototyping):** 3D-print, sand, paint. Splits at jaw or crown for assembly.
- **Urethane (production):** Cast from mold. Used in movie props (Reeves, Nolan). Hard, glossy, lightweight.
- **TPU/Flexible vinyl (comic look):** Vacuum-formed or printed. Mimics fabric stretch.
- **Carbon/aramid (impact):** Laid into female mold. Only if impact protection is priority.

**Finish:**
- Gloss black exterior
- 1–2 mm padding behind outer skin (prevents rattle, absorbs sweat)
- Velcro or magnetic attachment points to inner liner

### Layer 2: Structural Core
**Function:** Rigidity, internal mount points, cable routing  
**Thickness:** 2–6 mm  
**Material:**
- Carbon fiber (lightest, stiffer, more expensive)
- Fiberglass (cheaper, adequate rigidity)
- 3D-printed PETG lattice (prototyping, lighter than solid)

**Features:**
- Sagittal cable channel down the center (from brow to nape)
- Lateral channels behind ears for antenna and sensor leads
- Recessed mounting bosses for: battery, MCU, lens frames, speaker pods
- Mesh or vents over temples for passive heat circulation
- Magnetic attachment points at jaw (quick-release for maintenance)

### Layer 3: Inner Liner
**Function:** Fit, comfort, electronics mounting, thermal path  
**Thickness:** 4–12 mm  
**Composition (layered from inside out):**
1. **Moisture barrier** (1–2 mm): Thin TPU or silicone backing to contain sweat
2. **Electronics platform** (2–4 mm): 3D-printed PETG or nylon skull cap with recessed bays
3. **Structural foam** (1–2 mm): EVA or closed-cell foam for rigidity
4. **Comfort padding** (2–4 mm): Memory foam or gel padding at high-pressure points (temples, jaw hinge, nape)
5. **Breathable liner** (0.5 mm): Thin nylon mesh facing skin

**Electronics Mounting in Liner:**
- Recessed rectangular bays for battery pack (2 cm × 7 cm × 1.5 cm)
- Cylindrical pockets for cameras (1–2 cm Ø)
- Planar mount for MCU and radio board (flex PCB traces)
- Bone conduction transducer pockets at zygomatic arch (cheekbone contact points)
- Throat mic mount inside jaw liner
- Wire channels sealed with silicone to prevent moisture ingress

**Ventilation Paths:**
- Airflow over temples (passive convection to cool MCU and camera)
- Jaw intake / exhaust for breathing and air circulation
- Filter pack at mouth (replaceable 3M filter material)

---

## 3. Internal Component Layout

### Crown & Nape (Primary Electronics Zone)
**Mounted in upper skull-cap liner bay:**

```
        [Camera Brow Mount]
              ↓
    ┌─────────────────────┐
    │   MICRO-OLED HUD    │ (Brow ridge, optical path into lens combiner)
    │   0.5" display      │
    ├─────────────────────┤
    │   ESP32-S3 MCU      │ (Crown, center)
    │   Flex PCB brain    │
    ├─────────────────────┤
    │   LoRa Radio Module │ (Crown, right side)
    │   470 MHz antenna   │
    ├─────────────────────┤
    │   IMU (BMI270)      │ (Crown, left side)
    │   Head tracking     │
    ├─────────────────────┤
    │   Battery Pack      │ (Nape, primary power)
    │   2× 18650 or LiPo  │ (7.4V, ~5Wh)
    ├─────────────────────┤
    │   Thermal Camera    │ (FLIR Lepton, brow or ear)
    │   160×120 px, I²C   │
    └─────────────────────┘
```

**Power Budget (Head):**
- ESP32-S3: 80 mA avg
- HUD (micro-OLED): 30 mA
- Radio (idle): 10 mA; (tx): 500 mA peak
- Thermal camera: 80 mA
- IMU: 3 mA
- Bone conduction audio: 40 mA
- **Total idle:** ~250 mA | **Peak tx:** ~700 mA
- **Runtime on 2× 18650 (5Wh):** 6–8 hours idle; 2–3 hours active radio

---

### Ears (Antenna & Audio Pods)
**Left Ear:**
- Primary antenna mast (LoRa 470 MHz whip, 5–8 cm)
- BLE/Wi-Fi secondary antenna (coiled inside ear structure)
- Structural foam (prevents snap-off under stress)
- Quick-connect pogo or u.FL connector at ear root

**Right Ear:**
- Bone conduction transducer (Transonic T-Bone or similar)
- Contact surface flush against mastoid/zygomatic ridge
- Driver circuit (amplifier, crossover) mounted in ear pod
- Audio cable down nape to MCU

**Both Ears:**
- Rigid triangular support structure (3D-printed PETG)
- Foam dampening inside structure
- Drainage holes at bottom (sweat management)
- Optional: MEMS microphone (Knowles SPM0423) facing rearward for ambient sound input

---

### Eyes (Lens Assembly & Display)
**Lenses:**
- Shape: Large white cowl lenses (full eye coverage)
- Material: Injection-molded or vacuum-formed acrylic or PETG
- **Opacity:** Translucent white exterior + dark inner cup (eyes invisible from outside)
- **Interior surface:** Anti-fog coating + airflow path
- **Thickness:** 2–3 mm (structural support without distortion)

**Mounting:**
- Bayonet or clip frame that locks into structural core
- Allows tool-free removal for replacement
- Flex gasket prevents fogging (sealed but allows passive air exchange)

**Optical Path (HUD):**
- Micro-OLED display (0.5", 1920×1080, ~500 nits) mounted in brow ridge
- Combiner lens (partially silvered optical surface) integrated into right lens or a thin waveguide
- Light path: OLED → combiner → eye; ambient → combiner → eye (50/50 beamsplitter)
- Display driver: Onboard MCU connects via MIPI DSI or parallel interface

**Optional: Thermal Overlay**
- FLIR Lepton thermal camera mounted in brow or ear
- Thermal data overlaid on HUD via software
- Shows heat signatures, threat detection, tactical assessment
- Requires separate USB-C power if using older Lepton 2.x; newer versions lower power

---

### Jaw (Intake, Comms, Microphone)
**Filter & Air Intake:**
- Intake port at mouth or jaw (0.5–1" Ø opening)
- Replaceable 3M filter cartridge (0.3 µm, gas + dust)
- Internal baffle to prevent rain ingress
- Passive airflow; can be active-pumped if needed

**Throat Microphone:**
- Lavalier-style boom mic inside jaw liner
- Vibration-coupled to throat (contact sensor)
- Input to MCU ADC; software filters ambient noise
- PTT (push-to-talk) button on chin or jaw hinge

**Chin/Jaw Lock:**
- Mechanical latch at chin point (prevents accidental jaw opening)
- Release lever hidden inside mouth (2-second egress)
- Padded hinge (memory foam) at temporomandibular joint
- Allows articulation but prevents rotation under impact

---

### Nape (Power Primary, Thermal Exhaust)
**Battery Pack:**
- Primary: 2× 18650 LiPo cells in series (7.4V, ~5Wh)
- OR flat LiPo pouch pack (same voltage, better shape-fit)
- Recessed bay in nape liner, contact-cooled to outer shell
- XT60 or Anderson SB connector for external charging

**Secondary Battery (Belt-Pack):**
- Additional 2× 18650 or 10Wh LiPo
- Stored in belt or tactical pack
- Tether cable (2-wire, shielded) runs up back of neck to cowl
- Automatic switchover if head battery drops below 6V

**Thermal Vent:**
- Passive heat exchanger or mesh vent over nape
- Airflow: MCU heat → spine channel → open air via nape vent
- If active cooling needed: tiny 20 mm silicone fan (5V, <0.5W) in nape channel

---

## 4. Electronics Bill of Materials

### Brain & Compute
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| ESP32-S3 DevKit | Espressif ESP32-S3-DevKitC-1 | 1 | Primary MCU, 240 MHz dual-core, 8 MB PSRAM |
| Flex PCB | Custom 4-layer | 1 | Shaped to skull cap, all components soldered |
| Conformal coat | Parylene or acrylic | 1 | Protects against sweat |

### Power
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| 18650 LiPo cell | Samsung 25R or Sanyo NCR18650GA | 2 | 2500 mAh, 20A discharge |
| Parallel/series holder | Keystone 1028 | 1 | Mechanical support, screw terminals |
| XT60 connector | Amass XT60 | 2 | Charge/discharge, rated 60A |
| Buck converter | XL4015 5A | 1 | 7.4V → 5V for logic, 3A max |
| Boost converter | MT3608 | 1 | 7.4V → 12V for optics (optional) |
| Charging circuit | TP4056 | 1 | 1A Li-ion charger with protection |

### Radio & Comms
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| LoRa module | Semtech SX1276 breakout | 1 | 470 MHz (or 2.4 GHz variant), SPI interface |
| Antenna | 470 MHz whip or helical | 1 | 5–8 cm mast, mounted in ear |
| Connector | u.FL RF coax | 2 | Radio to antenna, shielded |

### Audio & Microphone
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| Bone conduction driver | Transonic T-Bone or Aftershokz module | 1 | Vibration-coupled audio, 20 mW |
| Audio amplifier | PAM8302 1W class-D | 1 | Drives bone transducer, 5V input |
| Throat microphone | Lavalier or boom mic capsule | 1 | Electret, ~4 kΩ impedance |
| Microphone preamp | MAX4466 | 1 | Gain-adjustable, 0–5V ADC input |

### Display & Optics
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| Micro-OLED | Sony UM4K (0.5", 1920×1080) | 1 | 500 nits, MIPI DSI input, 0.8W |
| Combiner lens | Custom optical or 45° dichroic | 1 | 50/50 beamsplitter or waveguide |
| Display driver | Custom or Seeed XIAO BLE expansion | 1 | Handles MIPI DSI or SPI interface |
| Anti-fog coating | Nanodots or commercial hydrophobic | 1 | Applied to lens interior |

### Sensors
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| IMU (9-DOF) | Bosch BMI270 | 1 | Gyro, accel, I²C, low power |
| Thermal camera | FLIR Lepton 3.5 | 1 | 160×120 px, radiometric, SPI |
| 1080p camera | OV5647 or similar spy cam | 1 | 100° FOV, I²C config, MIPI CSI |
| Ambient light sensor | VEML7700 | 1 | I²C, auto-exposure reference |
| Laser rangefinder | LIDAR-Lite v4 or VL53L1X | 1 | I²C, 50–400 m range |

### Wiring & Connectors
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| Silicone wire | 24 AWG silicone | 20 m | Flex + sweat resistant |
| Flex PCB | 2-layer custom traces | 1 | Signal/power distribution along nape |
| Shielded cable | 2-wire twisted pair + shield | 2 m | Antenna, audio, sensor lines |
| Micro USB-C | Sealed pogo connector or magnetic | 1 | Charging only (head battery) |
| JST-PH connectors | 2.0 mm pitch | 10 | Quick-disconnect for sensors, modular |

### Structural & Materials
| Component | Part Number | Qty | Notes |
|-----------|-------------|-----|-------|
| 3D-print resin | Formlabs or Prusament PETG | 2 kg | Inner skull cap, mounting brackets |
| EVA foam sheet | 20 mm closed-cell | 1 | Structural support, thermal barrier |
| Memory foam padding | 10 mm | 1 | Comfort liner, high-pressure points |
| TPU seal strips | 5 mm × 2 m | 1 | Sweat barrier, airflow seals |
| Magnets | Neodymium N52, 8 mm Ø | 10 | Jaw quick-release, component mounting |
| Thermal pads | 3 mm silicone, 5 W/mK | 3 | Battery, MCU, thermal camera to shell |

---

## 5. Power Distribution Architecture

### Battery Chemistry & Voltage
- **Nominal:** 7.4V (2S LiPo)
- **Fully charged:** 8.4V
- **Cutoff:** 6.0V (safety)
- **Capacity:** 5–10 Wh (2–4 hour runtime depending on load)

### Power Rail Breakdown
```
7.4V Input (Battery)
    ├─→ [Charger Circuit (TP4056)] → Battery cell
    ├─→ [Buck 5V / 3A] → MCU, radio, sensors, audio amp
    ├─→ [Direct 7.4V] → Boost converter → 12V (optional: lens illumination, thermal camera PA)
    └─→ [Fuse 10A] → Protection against short
```

### Typical Power Budget (Active Operation)
| Component | Current | Duty | Contribution |
|-----------|---------|------|--------------|
| ESP32-S3 | 80 mA | 100% | 80 mA |
| Micro-OLED HUD | 30 mA | 50% | 15 mA |
| LoRa radio (idle) | 10 mA | 100% | 10 mA |
| LoRa radio (tx, 27 dBm) | 500 mA | 2% | 10 mA |
| Thermal camera | 80 mA | 50% | 40 mA |
| IMU | 3 mA | 100% | 3 mA |
| Bone audio | 40 mA | 20% | 8 mA |
| Microphone preamp | 5 mA | 100% | 5 mA |
| **Total Avg** | — | — | **~171 mA** |

**Estimated Runtime:**
- 5 Wh battery ÷ 171 mA avg = ~29 hours (theoretical)
- Practical: 12–16 hours (accounting for peak draws, inefficiency)
- With external belt battery: 24+ hours continuous operation

### Charging & Maintenance
- **Charging connector:** Sealed USB-C (pogo or magnetic) at jaw
- **Charge rate:** 1A (TP4056) = ~5 hours from empty
- **Off-board charger:** Connect external battery via XT60 while wearing (hot-swap)
- **Discharge curve monitoring:** MCU measures voltage; warns at 20%, 10%, critical at 6V

---

## 6. Communication Systems

### Primary: LoRa (Long-Range, Low Power)
- **Frequency:** 470 MHz (LoRa band, varies by region)
- **Module:** Semtech SX1276
- **Range:** 5–10 km (line-of-sight, depending on antenna gain and terrain)
- **Data rate:** 250 bps – 11 kbps (configurable)
- **Protocol:** LoRaWAN or raw LoRa packets
- **Antenna:** 5–8 cm whip in left ear, ~3 dB gain
- **Power:** 27 dBm peak (~500 mA for 100 ms bursts)

**Use Case:** Long-range tactical comms, team coordination, evidence upload

### Secondary: BLE 5.0 (Short-Range, Low Power)
- **Built-in:** ESP32-S3 has BLE radio
- **Range:** 100–400 m (depending on environment)
- **Data rate:** 2 Mbps (BLE 5.0)
- **Power:** ~50 mA peak tx
- **Antenna:** Coiled secondary antenna inside left ear

**Use Case:** Phone pairing, data sync, proximity alarms

### Tertiary: 802.11 Wi-Fi (Local Network)
- **Built-in:** ESP32-S3 has 802.11b/g/n
- **Range:** 100–300 m (typical home/office)
- **Power:** ~80–120 mA active
- **Antenna:** Shared with BLE or separate 2.4 GHz PCB antenna

**Use Case:** High-bandwidth data logging, video upload, remote access

### Optional: Voice Radio (UHF/VHF PTT)
- **Module:** Baofeng or Motorola compact radio with remote head
- **Frequency:** 400–470 MHz (tactical bands, requires licensing in most countries)
- **Antenna:** Run up spine to left ear
- **PTT:** Thumb button on belt or jaw switch
- **Range:** 1–3 km (depending on terrain and radio license)

**Use Case:** Voice communication with team (licensed operation only)

---

## 7. Optical & Display System

### HUD (Head-Up Display)
**Requirements:**
- Real-time telemetry display (compass, speed, threats)
- Thermal or night-vision overlay
- Mission timer and status lights
- Minimalist UI (high contrast, no eye strain)

**System:**
- **Display:** Sony UM4K 0.5" micro-OLED (1920×1080, 500 nits)
- **Optics:** Beamsplitter or waveguide combiner lens
- **Field of view:** 30–40° (not full vision, peripheral awareness only)
- **Input:** SPI or MIPI DSI from MCU
- **Power:** 800 mW (built-in boost converter)

**Optical Path:**
```
Micro-OLED (brow)
    ↓ (light emitted at 45°)
Combiner Lens (dichroic or beamsplitter)
    ↓ (50% reflects OLED, 50% transmits ambient)
Left Eye (sees both HUD overlay and real world)
```

**Rendering:**
- 24 FPS minimum (30 FPS preferred to match eye flicker fusion)
- Vector graphics library (low bandwidth: compass, vectors, text)
- Thermal data mapped to 16-color palette
- Automatic brightness adjust based on ambient light (VEML7700 sensor)

### Lenses (Eyes)
**Function:** Concealment + display integration + weather protection

**Material:** Injection-molded or vacuum-formed acrylic
- **Opacity:** White translucent (outside) + dark interior cup (eyes invisible)
- **Shape:** Wrap around eye, ~60 mm wide × 50 mm tall
- **Curvature:** Match eye orbit (reduce distortion)

**Coatings:**
- **Anti-scratch:** Hard coat (2–5 µm polyurethane)
- **Anti-fog:** Hydrophobic nanodots or commercial DesolVe (must be reapplied ~monthly)
- **UV blocking:** Optional (protects eye from bright ambient light)

**Ventilation:**
- Micro-channels (0.5 mm × 2 mm) around lens rim
- Passive airflow path (no active fans needed)
- Inner cup absorbs excess condensation (replaceable felt insert)

**Maintenance:**
- Remove lenses with 10-second twist lock
- Rinse, wipe, re-apply anti-fog monthly
- Replace worn cups annually (~$20 part)

---

## 8. Sensor Suite

### Inertial Measurement Unit (IMU)
**Component:** Bosch BMI270 (9-DOF)
- **Axes:** 3× gyroscope, 3× accelerometer, 3× magnetometer
- **Sample rate:** Up to 1600 Hz (though 100 Hz sufficient for head tracking)
- **Interface:** I²C or SPI
- **Power:** 3.5 mA (active); 0.3 mA (sleep mode)

**Function:**
- Head orientation tracking (roll, pitch, yaw)
- Jerk detection (impact alert)
- G-force logging (punch/collision force)
- Auto-rotate HUD based on head position
- Pedestrian dead-reckoning (drift estimate)

### Thermal Camera
**Component:** FLIR Lepton 3.5
- **Resolution:** 160 × 120 pixels (19,200 total)
- **Accuracy:** ±3°C (radiometric)
- **FOV:** 56° × 42°
- **Frame rate:** 9 Hz (60 Hz burst mode available)
- **Interface:** SPI (16 Mbps clock)
- **Power:** 150 mW (peak); 80 mW (idle)

**Function:**
- Thermal detection of warm bodies (red team)
- Equipment identification (hot vs. cold sources)
- Night operation (no visible light required)
- Overlay on HUD as 16-color thermal map
- Logging for post-mission analysis

**Considerations:**
- Thermal data is low-resolution; fuse with visible camera for context
- Calibration drift over time (on-board calibration every 30 sec)
- Sensitive to rain/fog (steam will appear as warm)

### Visible Light Camera
**Component:** OV5647 1080p module or similar
- **Resolution:** 2592 × 1944 @ 30 FPS (video: 1920 × 1080 @ 30 FPS)
- **FOV:** 100–120° (wide angle, good situational awareness)
- **Interface:** MIPI CSI-2 or USB
- **Power:** 60–100 mA (active)

**Function:**
- Situational awareness (CCTV feed)
- Evidence recording with GPS timestamp
- Threat identification (facial recognition optional, privacy-sensitive)
- Augment thermal with RGB context

**Mounting:** Brow ridge or ear (small form factor)

### Ambient Light Sensor
**Component:** Vishay VEML7700
- **Range:** 0.001–120,000 lux
- **Interface:** I²C
- **Power:** 1 mW (active)

**Function:**
- Auto-adjust HUD brightness
- Sun glint detection
- Lighting condition logging
- Trigger night-vision mode if dark

### Laser Rangefinder
**Component:** ST VL53L1X (optional)
- **Range:** 50–4000 mm
- **Accuracy:** ±4% (at 1 m)
- **FOV:** ~27° (wide cone)
- **Interface:** I²C
- **Power:** 20 mW (active)

**Function:**
- Distance to threat/object
- Ranged targeting for arrows, line throws, etc.
- Obstacle detection (urban navigation)
- Logged as part of HUD telemetry

---

## 9. Firmware Architecture

### Main Loop (100 Hz Core Tick)
```
1. Read IMU (gyro, accel, mag) @ 100 Hz
2. Update head orientation estimate
3. Sample thermal camera (9 Hz) if due
4. Check radio for incoming packets (non-blocking)
5. Update HUD render buffer
6. Handle PTT button (voice radio)
7. Log telemetry to SD card (time-stamped)
8. Check battery voltage; warn if low
9. Sleep 10 ms until next tick
```

### Module Architecture
```
batman-cowl/
├── main.ino                    # Core loop, setup, main state machine
├── sensors.cpp / .h            # IMU, thermal, ambient light
├── radio.cpp / .h              # LoRa SX1276 driver + protocol
├── hud.cpp / .h                # Micro-OLED rendering + overlay logic
├── audio.cpp / .h              # Bone conduction, mic input, voice queue
├── power.cpp / .h              # Battery monitoring, low-power modes
├── telemetry.cpp / .h          # Data logging, timestamping, compression
└── config.h                    # Pin definitions, calibration, tuning
```

### Key Algorithms
1. **Head Tracking:** Quaternion-based IMU fusion (Madgwick or Kalman)
2. **Thermal Overlay:** Bilinear interpolation (upscale 160×120 to 1920×1080)
3. **Night Vision:** Histogram equalization on visible camera, overlay on thermal
4. **Voice Detection:** Simple energy-based VAD (voice activity detection)
5. **GPS Integration:** Optional; receive from belt/phone via LoRa or BLE
6. **Evidence Log:** Timestamped ring buffer (circular, oldest entries overwritten)

---

## 10. Fabrication & Build Order

### Phase 1: Design & CAD (2–3 weeks)
1. **Head Scan:** CT scan, photogrammetry, or 3D-printed life-cast of wearer's head
2. **Outer Cowl CAD:** Model Batman silhouette around scanned head + 4–8 mm offset
3. **Inner Skull Cap CAD:** Design mounting bays for battery, MCU, cameras, etc.
4. **Component fit check:** CAD simulation of all electronics in bays (no interference)

### Phase 2: Outer Shell Prototype (1–2 weeks)
1. **Print PETG outer cowl** on FDM printer (e.g., Prusa or Creality)
   - Split at jaw or crown for easy assembly
   - Supports: Generate internal branching tree
   - Layer height: 0.1 mm (finer details on face/brow)
2. **Cure (if resin):** Wash, dry, UV cure
3. **Post-processing:** Sand (120 → 220 → 400 grit), primer, paint (flat black)
4. **Fit test:** Put on, check comfort and visibility

### Phase 3: Inner Chassis (1 week)
1. **Print PETG skull cap** (inner liner mount)
   - Lattice pattern (not solid) to reduce weight
   - Recessed bays: 2 cm × 7 cm × 1.5 cm (battery)
   - Mounting bosses for MCU, camera, lenses
2. **Glue thermal pads** to battery bay (contact cooling)
3. **Test fit:** Place in outer cowl, verify alignment
4. **Wear test (20 min):** Check for hot spots, adjust padding

### Phase 4: Electronics Breadboard (2 weeks)
1. **Bench build:** Assemble all components on breadboard or prototyping PCB
   - Battery → buck converter → MCU, radio, sensors
   - Test each subsystem independently
2. **Firmware development:** Code radio, HUD, IMU, thermal, audio
3. **Integration test:** All systems running simultaneously
4. **Power measurement:** Log current draw, optimize sleep states

### Phase 5: Flex PCB Design & Production (2 weeks)
1. **Design flex PCB** tracing power and signal along skull cap curves
   - Route traces to follow printed mounting bosses
   - Place vias for mechanical support
   - 2-layer PCB (power on layer 1, signal on layer 2)
2. **Order from JLCPCB, Flex PCB manufacturer**
3. **Receive and inspect:** Check traces, solder pads
4. **Solder components:** Use stencil + reflow or hand solder (small boards)

### Phase 6: Integration & Fit (1 week)
1. **Mount flex PCB into skull cap**
2. **Install components:**
   - Battery in nape bay (thermal pad contact)
   - MCU in crown bay
   - Radio module in side bay
   - Cameras in brow/ear pockets
3. **Wiring:** Connect battery, radio antenna, camera CSI cables
4. **Conformal coat:** Parylene or acrylic spray (protects against sweat)
5. **Thermal test (30 min):** Measure temperature at MCU, nape, battery under load
   - Target: <60°C at all sensors
   - If exceeding: Add vent fans or move compute to belt

### Phase 7: Optics & Lenses (1–2 weeks)
1. **Design lens combiner:** CAD model optical path, send to custom optics vendor
   - OR use off-the-shelf micro-OLED display + beamsplitter
2. **Vacuum form or inject lenses** (white translucent acrylic + dark inner cup)
3. **Apply anti-fog coating** (nanodots or commercial)
4. **Assemble lens frame** into cowl (click-in or screw-mount)
5. **Align HUD optical path:** Test display visibility from eye position

### Phase 8: Final Assembly & Cosmetics (1 week)
1. **Glue outer skin to structural core** (or use velcro/magnets for removability)
2. **Install memory foam padding** at high-contact points (temples, jaw, nape)
3. **Attach ear pods** (magnets or quick-connect pogo)
4. **Mount jaw assembly** with hinge, spring, and latch
5. **Final paint/weathering:** Add scuff marks, Bat-insignia, etc.
6. **Quality check:** All systems powered, test radio, thermal, HUD, audio

---

## 11. Material Selection & Sourcing

### Outer Skin
| Option | Pros | Cons | Cost |
|--------|------|------|------|
| PETG (3D-printed) | Easy, iterate fast, cheap molds | Brittle, prone to cracking, finish work tedious | $50–200 |
| Urethane cast | Professional look, durable, glossy | Requires mold, slower iteration, toxic fumes | $300–1000 |
| Carbon fiber (laid) | Lightest, stiffest, impressive | Expensive, requires female mold, brittle on edges | $500–2000 |
| Vacuum-formed vinyl (TPU) | Flexible like real suit, comfortable | Less rigid, looks plastic-y, slower to set | $100–400 |

**Recommendation:** Start with PETG (fast iteration), switch to urethane cast if you plan to make multiple copies.

### Structural Core
| Material | Thickness | Weight (head) | Stiffness | Cost |
|----------|-----------|--------------|----------|------|
| 3D-printed PETG lattice | 2–3 mm | 200–250 g | Medium | $20–50 |
| Fiberglass cloth + resin | 3–4 mm | 150–200 g | High | $30–80 |
| Carbon fiber weave | 2–3 mm | 80–120 g | Very high | $80–200 |

**Recommendation:** PETG lattice for first build (easy to iterate), carbon for production (lighter, stiffer).

### Inner Liner
| Layer | Material | Thickness | Source |
|-------|----------|-----------|--------|
| Moisture barrier | TPU sheet or silicone | 1–2 mm | Amazon, McMAster-Carr |
| Electronics platform | 3D-printed PETG | 2–4 mm | Home 3D printer |
| Structural foam | Closed-cell EVA | 15–20 mm | Industrial foam suppliers |
| Comfort padding | Memory foam or gel | 8–10 mm | Craft stores, EBay |
| Breathable liner | Nylon mesh | 0.5 mm | Fabric stores |

---

## 12. Testing & Validation Checklist

### Mechanical
- [ ] Fit: Cowl sits centered, no forward/backward sag
- [ ] Comfort: 30-minute continuous wear without hot spots
- [ ] Egress: Remove fully in 2 seconds, emergency release works
- [ ] Impact: Drop test 1 meter onto padded surface (no cracks)
- [ ] Jaw lock: Chin latch holds under 20 N pull force

### Thermal
- [ ] MCU temperature: <60°C after 30 min active operation
- [ ] Battery temperature: <55°C (safe for LiPo)
- [ ] Nape vent: Measurable airflow over thermal camera area
- [ ] Sweat management: No pooling on electronics after 20 min wear

### Electrical
- [ ] Battery voltage: Stable 7.4V nominal, no sag under 500 mA load
- [ ] Radio: LoRa transmits, receives at 1 km range (line-of-sight)
- [ ] HUD display: Visible in bright sunlight, readable from eye distance
- [ ] Microphone: Voice intelligible, VAD activates correctly
- [ ] Bone audio: Audible at normal volume, no feedback

### Optical
- [ ] Lens anti-fog: No condensation after 10 min breath test
- [ ] Visibility: Clear forward and ±45° lateral vision
- [ ] HUD rendering: 30 FPS minimum, no flicker
- [ ] Thermal overlay: Image updates at 9 Hz, no lag

### Software
- [ ] IMU calibration: Compass points to true north ±10°
- [ ] Thermal calibration: FLIR reports accurate temperature (±3°C)
- [ ] Telemetry logging: GPS, IMU, thermal data timestamped correctly
- [ ] Radio protocol: Packets encode/decode correctly, CRC valid
- [ ] Low-power mode: MCU sleeps when inactive, wakes on PTT button

### Safety
- [ ] No pinch points that trap skin
- [ ] All metal edges are filed smooth
- [ ] No exposure of battery terminals (insulated internally)
- [ ] Emergency release mechanisms are accessible with gloved hand
- [ ] Wearer can call for help immediately (radio or voice)

---

## 13. Known Limitations & Future Upgrades

### Current (MVP)
- Single-unit battery (head-mounted)
- Fixed HUD (no eye-tracking)
- Mono thermal (no RGB overlay fusion)
- LoRa only (no voice radio frequency)
- Manual logging (no autonomous edge AI)

### Near-term (3–6 months)
- Belt-pack with secondary battery (hot-swap)
- Eye-tracking camera (gaze-based HUD control)
- Thermal + RGB registration (software alignment)
- Licensed voice radio (PTT with team net)
- Local edge inference (person detection on MCU)

### Long-term (1–2 years)
- Full AR glasses integration (optical see-through HUD)
- Binocular thermal (two thermal cameras)
- Facial recognition (real-time threat database)
- Autonomous drone relay (remote recon)
- Neural interface exploration (EMG-based control)

---

## 14. References & Standards

### Design Inspiration
- Ben Nott's "Batman Cowl Engineering" (reference guide)
- fighter HUD design: Elbit Systems, BAE Systems
- AR combat helmets: Anduril, Integrated Visor Equipment (IVE)
- Drone thermal: FLIR Boson, Tau 2

### Components & Datasheets
- ESP32-S3: https://www.espressif.com/en/products/socs/esp32-s3
- Semtech SX1276: https://www.semtech.com/products/wireless-rf/lora-core/sx1276
- Bosch BMI270: https://www.bosch-sensortec.com/products/motion-sensors/imus/bmi270
- FLIR Lepton: https://www.flir.com/products/lepton/
- Sony UM4K micro-OLED: https://www.sony.com (reference; limited public datasheet)

### Fabrication & Supplier Resources
- 3D printing: Prusa, Ultimaker, Formlabs
- PCB fabrication: JLCPCB, PCBWay
- Flex PCB: Flex PCB Manufacturer (DFM for curved traces)
- Optics: Custom optics vendors (Edmund Optics, Thorlabs)
- Electronics components: DigiKey, Mouser, AliExpress (component sourcing)

---

**Document Version:** 1.0  
**Last Updated:** 2026-08-25  
**Status:** Complete system specification. CAD models and firmware in development.
