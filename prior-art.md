# HDR-to-high-SDR prior art and claim audit

## Owner-provided research lead

On 2026-09-22 Daniel supplied a Gemini answer identifying “real-time
HDR-to-SDR tone mapping with high bit-depth output” as the established way to
use HDR masters on high-quality SDR projectors and professional displays. Its
central engineering claim is retained: tone/gamut mapping should run at high
internal precision and the final SDR signal should avoid an unnecessary 8-bit
bottleneck when the display path supports more.

The answer is recorded as a research lead, not a primary source. Four phrases
are narrowed for the public technical record:

- dynamic tone mapping may be frame-, scene-, histogram-, or metadata-driven;
- higher output bpc preserves finer code-value precision, but does not by
  itself preserve the source gamut or mastering display;
- well-designed dithering can make 8-bit output useful, so 8-bit is not
  automatically “destructive,” though it remains the narrower representation;
- “gold standard” and “massive upgrade” are value judgements; the measurable
  claims are gradient error, banding, gamut mapping, real-time performance,
  scanout format, and achieved link depth.

It also usefully separates luminance mapping from precision preservation, but
the public record needs three separate numbers wherever it says “16-bit”:

| stage | what the number describes | current project target |
|---|---|---|
| processing | shader/intermediate arithmetic | FP16 or higher justified precision |
| scanout | pixels stored in the DRM framebuffer/plane | 16-bit integer/float when supported; XR30 fallback |
| link | code words physically transported to the sink | highest verified sink- and bandwidth-valid 10/12/16 bpc |

A 16-bit processing or scanout surface can still be quantized onto a 10- or
12-bpc HDMI link. Conversely, a 10-bit framebuffer can be transported in a
12-bpc link container without creating source precision. Every report must
name the stage instead of collapsing all three into “16-bit output.”

## Software precedent

- [mpv's current manual](https://mpv.io/manual/master/) documents HDR peak
  analysis, multiple tone/gamut-mapping algorithms, explicit target transfer
  functions, and dithering to a requested output depth. It also states the
  integration problem directly: outside D3D11 it cannot detect the on-wire bit
  depth, and it cannot guarantee what happens after the signal leaves mpv.
- [libplacebo](https://code.videolan.org/videolan/libplacebo) supplies the GPU
  colour pipeline beneath mpv's `gpu-next` path.
- [gamescope's compositor source](https://github.com/ValveSoftware/gamescope/blob/master/src/steamcompmgr.cpp)
  contains PQ-to-gamma-2.2 tone mapping, proving that HDR-to-SDR already exists
  in an open real-time compositor. A current
  [10-bit output fallback report](https://github.com/ValveSoftware/gamescope/issues/2075)
  illustrates the remaining end-to-end problem: correct shader math does not
  guarantee the selected DRM/link depth.

## Hardware precedent

- [madVR Envy's public control protocol](https://madvr.com/EnvyIpControl.pdf)
  exposes 8/10/12-bit output state and SDR/HDR10/HLG modes, evidence that a
  commercial processor treats transfer system and link depth as separate
  controlled quantities.
- [Lumagen Radiance Pro](https://www.lumagen.com/products-sales/p/explore-radiance-pro)
  documents HDR dynamic tone mapping and a 12-bit 4:2:2 video pipeline.
- [Lumagen's update history](https://www.lumagen.com/software-updates/radiance-pro-updates)
  records continuing dynamic-tone-mapping work and 12-bit LUT precision.

These products prove demand and feasibility. They do not close the Linux gap:
their processing and hardware policy are proprietary, and they do not provide
a shared DRM contract across GPU vendors.

## What remains new and useful

The project target is an open, vendor-neutral path that:

1. consumes real source transfer/gamut/mastering metadata;
2. tone- and gamut-maps live in FP16 or higher justified working precision;
3. negotiates a verified 10- or 16-bit scanout format;
4. selects the highest sink- and bandwidth-valid 10/12/16-bpc link depth;
5. exposes the achieved depth rather than only the requested ceiling;
6. preserves display calibration and performs a deliberate final dither;
7. produces the same evidence table on amdgpu, i915, nouveau, and proprietary
   NVIDIA.

That integration and observability layer—not the invention of tone mapping—is
the cross-maker gap this project aims to close.
