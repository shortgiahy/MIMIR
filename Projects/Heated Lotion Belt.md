# Heated Lotion Belt

Portable heated lotion-bottle belt for massage therapists. Friend's commercial concept, greenlit June 2026. Scope/timeline undefined.

## Requirements

- Hold 110°F (bottle exterior 75–80°F band acceptable), 8-hour shift on one charge
- Holster with adjustable strap; duty-cycled heating (on ~20–30% of the time), not continuous

## Design

| Aspect | Spec |
|--------|------|
| Bottle | 2" dia × 5.5" cylinder (~35 sq in surface) |
| Heating element | 5.5 ft of 28 AWG Nichrome 80 (~24 Ω) |
| Power | 12V USB-C PD power bank + 12V decoy trigger cable (or flat 12V Li-Po); 0.5 A / 6 W |
| Control | Closed-loop (ATTiny85 or analog op-amp) holding flat 110°F; ~48 Wh covers 8 hrs |
| Layering, inside→out | Aluminum/HDPE bottle → aluminum foil tape (heat spreading) → Nichrome wire → Kapton tape → 3–5mm perforated neoprene belt shell |
| Housing | Components distributed around the waist; coin-sized rigid-flex PCB in flexible 3D-printed TPU enclosure |

Heat-up: center of a 2" bottle takes 40–45 min to reach 110°F from outside-in; shaking cuts it under 15 min.
