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

## Disclaimer: AI use
Every trace, every footprint and every questionable part choice on this board is mine. I placed and routed it all by hand. AI didn't draw a single line of copper. What I *am* doing is testing whether AI is any good at spotting my mistakes before a fab house turns them into expensive coasters.

`todo.md` was written entirely by AI. It's the list of everything it found. I read the suggestions, double-check them, and then fix them myself in KiCad, usually while muttering.

Still not sure a human made this? Read the commit history. It's an emotional journey. It goes from a confident "cirrcuit done" to "Insane amounts of routing" to "I cant do this anymore", then "Ill redo routing i guess", then "DONE!!!", and finally the very honest "Done for now". No AI writes commit messages that sound this tired.

*This section was also written with AI. The suffering in the commit history was not.*
