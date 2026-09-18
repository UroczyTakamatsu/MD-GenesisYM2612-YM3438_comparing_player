YM3438 VGM/VGZ seamless loop behavior fix.

Changed files:
- src/main.rs
- web/index.html
- web/worklet.js

Behavior:
- VGM loop information is always rendered/prepared when a loop exists, regardless of UI loop ON/OFF.
- Loop OFF stops at the first loop end and does not play the second loop pass.
- Loop ON can be enabled before or during the loop without rerunning WASM.
- While looped, the seek bar/progress reports the corresponding first-pass logical position, never accumulating loop repetitions.
- Turning loop OFF during the loop maps playback back to the corresponding first-pass position and then stops at the loop end.
- Seek duration excludes the repeated loop pass.


## YM2612 comparison addition

The browser player now renders the same VGM/VGZ command timeline through both YM3438 and YM2612.

- YM3438 keeps the existing per-channel renderer and its existing `128` output gain.
- YM2612 is rendered as one full-chip stereo stream.
- YM2612 receives the original YM register/value pairs from the VGM command stream without the YM3438 channel-isolation/panning rewrite.
- The YM3438 `128` output gain is **not** applied to YM2612. YM2612 uses its own neutral PCM conversion gain of `1`.
- The PSG streams remain shared and unchanged.
