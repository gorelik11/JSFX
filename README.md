# RCBit JSFX Suite

Bit-accurate dynamics processing plugins for REAPER. All gain values use exact powers of 2 (`2^x`) instead of traditional dB conversions (`10^(dB/20)`), preserving the binary structure of digital audio.

## Plugins

### RCBitBrickwall V4.0 — Brickwall Limiter (recommended)

True brickwall lookahead limiter with switchable Light/HQ modes.

- **Light mode** (default) — near-zero CPU. Envelope runs on live signal, applied to delayed audio. Perfect for monitoring and mixing
- **HQ mode** — full lookahead scan + cosine-windowed gain reshaping for maximum transparency. Use for final renders
- **Bit-accurate ceiling** — exact power of 2, no rounding artifacts
- **Oversampled peak detection** — 1x/2x/4x/8x via Hermite cubic interpolation. Catches inter-sample peaks without oversampling the whole signal path
- **Safety clamp** — hard ceiling guarantee as last resort; with 5–10ms lookahead it rarely fires
- **Stereo Linked / Dual Mono** — independent per-channel envelopes and gain buffers in dual mono mode
- **Auto-release** — program-dependent: deeper limiting = slower release
- **PDC** — delay compensation reported to REAPER

| Parameter | Range | Default | Description |
|-----------|-------|---------|-------------|
| Ceiling | 0–8 bits | 0 | Bits below 0 dBFS |
| Input Gain | -16–16 bits | 0 | Bit-accurate input gain |
| Ceiling Micro | -100–100% | 0 | Fine-tune ceiling (% of a bit) |
| Lookahead | 0.1–10 ms | 5 | Lookahead window size |
| Release | 1–1000 ms | 100 | Release time |
| Release Mode | Manual/Auto | Manual | Auto = program-dependent |
| Stereo Mode | Linked/Dual Mono | Linked | Independent per-channel limiting |
| Peak Oversampling | 1x/2x/4x/8x | 4x | Inter-sample peak detection |
| Quality | Light/HQ | Light | Light = low CPU, HQ = cosine-windowed attack |

### RCBitLimiter V1.0 — Soft Lookahead Limiter

Lookahead limiter with PurestGain smoothing. Softer character — gain envelope is smoothed via IIR filter (AirWindows PurestGain technique), so output may slightly exceed ceiling on extreme transients. Good for transparent gain riding.

### RCBitComp V1.0 — Compressor

Bit-accurate compressor with downward and upward compression, RMS detection, and sidechain HPF/LPF filtering. Threshold, makeup, and knee specified in bits.

## Older Versions

- **RCBitBrickwall Light V1.0** — standalone Light mode version
- **RCBitBrickwall V3.0** — HQ only, no Light/HQ switch
- **RCBitBrickwall V2.0** — dual mono, no oversampling
- **RCBitBrickwall V1.0** — stereo linked only, no oversampling

## Installation

Copy the desired plugin files to your REAPER Effects folder:
- macOS: `~/Library/Application Support/REAPER/Effects/`
- Windows: `%APPDATA%\REAPER\Effects\`
- Linux: `~/.config/REAPER/Effects/`

Then add via FX browser in REAPER (search "RCBit").

## Philosophy

Traditional digital audio processing uses `10^(dB/20)` for gain conversions, which introduces floating-point rounding at every step. By using `2^x` (bit-shifting), gain changes map exactly to IEEE 754 exponent adjustments — the mantissa (the actual audio data) passes through untouched. This matters most at low levels where rounding errors are proportionally largest.

The brickwall limiter extends this philosophy: the **ceiling** is bit-accurate (the reference point is exact), while **dynamic gain reduction** uses continuous float values with cosine-windowed shaping for smooth, alias-free transitions.

## License

GPL v3 — http://www.gnu.org/licenses/gpl.html
