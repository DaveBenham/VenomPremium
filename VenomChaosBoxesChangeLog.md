# Venom Chaos Boxes Change Log

## 2.1.0 (2026-08-??)
### Enhancements
- Hybrid Knot
  - Envelope outputs are no longer band limited by default. The low pass filter used by band limiting could prevent very fast envelopes from reaching zero.
    - A module context menu option was added to enable envelope output band limiting.
    - Old patches default to envelope output band limiting enabled to preserve old behavior.
  - Synced envelope triggers no longer abort if the 0 crossing occurs after the triggering gate goes low. Old behavior could lead to dropped triggers if using low frequency audio with high clock speeds.
    - A "Restrict synced envelope triggers to high gates" module context menu option was added to restore old behavior. Old patches default to this option enabled.

### Bug Fixes
- Hybrid Knot
  - Fixed spelling of "Shift register" in Add and Del button names
- Venjolin+
  - Fixed spread control names on initial placement
- Hybrid Knot, Venjolin+, and Vlippoo Box+
  - Removed string allocation during knob reconfiguration from the dsp thread. Should be a transparent change, but old code had the potential to cause rare hiccups in the sound.

## 2.0.0 (2026-04-09)
### Initial Release
- Venjolin
- Venjolin+
- Vlippoo Box
- Vlippoo Box+
- Hybrid Knot
- Chaos Gates Expander
- Chaos Volts Expander
