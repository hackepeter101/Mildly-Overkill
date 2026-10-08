# Simple PLC-like Controller for Simple 24V Logic

## Specifications I am aiming for:

### Power & Protection
- 3S to 6S LiPo input support (including 24V)
- Onboard buck converter
- Reverse polarity protection
- Integrated fuse

### Motor Control
- 2x DC motor drivers (H-bridges)
- 3A current rating per bridge
- Current sensing at the H-bridges

### I/O & Interfaces
- Minimum of 8 total Inputs/Outputs (including H-bridge pins)
- STEMMA QT (I2C) connector
- USB-C port
- Physical Boot and Reset buttons

## Why
I have a project I need this for. I also did something similar for my day job and, to be honest, I wasn't entirely satisfied with my own work there—so this is my chance to improve on it.
