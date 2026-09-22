# Open implementation roadmap

The first project is useful without an HDR sink: make nouveau train the Sony
KDL-46HX855 at 12 bpc, then use that high-precision transport for real-time
HDR-to-SDR tone mapping.

The executable experiment and recovery transaction are maintained in the
[nouveau HDMI deep-colour hardware plan](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/docs/research/nouveau-hdmi-deep-colour-plan-2026-09-22.md).

## Reference triangle

- **amdgpu** is the primary open implementation reference. Its DC code shows
  how DRM connector state, EDID deep-colour bits, bandwidth, pixel encoding,
  output depth, dithering, and HDMI packet state are selected together.
- **Proprietary NVIDIA** is a behavioural control on the same GA106 and sink.
  Its open `nvidia-drm` glue documents the KMS-facing property contract; the
  closed NVKMS result is measured at the television rather than guessed from
  unavailable source.
- **nouveau** is the repair target. The relevant display class headers already
  define 36-bpp RGB 4:4:4 pixel-depth values, while the live path defaults
  HDMI to the ordinary depth and exposes no `max bpc` control.

This is comparison for semantics and validation, not code transplantation
from a closed component.

## Phase A — 12-bpc HDMI on nouveau

### Source work

1. Derive the allowed RGB 4:4:4 depths from
   `edid_hdmi_rgb444_dc_modes` (`DC_30` and `DC_36`).
2. Attach a bounded `max bpc` connector property for HDMI.
3. Select 8, 10, or 12 bpc during atomic check, degrading only when the
   selected mode would exceed the sink/source TMDS limit. At 1080p60,
   148.5 MHz × 1.5 = 222.75 MHz, inside this sink's declared 225 MHz maximum.
4. Carry 12 bpc through the SOR and GA10x head-class depth mappings instead
   of leaving TMDS at the default depth.
5. Program or verify the HDMI General Control Packet deep-colour indication
   and pixel-packing phase required by the hardware.
6. Expose the resulting link depth through the kernel's immutable `link bpc`
   property when that common DRM series is present.

### Acceptance

- EDID SHA-256 remains fixed and `edid-decode` reports `DC_36bit`,
  `DC_30bit`, and 225 MHz.
- `modetest`/`drm_info` show the intended property range and selected value.
- An atomic 1080p60 request succeeds without corrupting AVI, audio, or the
  already verified HDMI 3D modes.
- The HX855 signal-information OSD reports 12-bit.
- Rollback returns to the stock module and known-good desktop.

That result is deep-colour transport evidence, not HDR output.

## Phase B — real-time HDR-to-SDR at high precision

### The cross-vendor gap

This is not a missing tone-mapping equation. The components exist but do not
form one dependable product path:

- libplacebo/mpv can tone-map HDR video to SDR in real time;
- [gamescope contains a PQ-to-gamma-2.2 path](https://github.com/ValveSoftware/gamescope/blob/master/src/steamcompmgr.cpp),
  yet output selection remains fragile enough for live reports of
  [HDR falling back to an 8-bit link](https://github.com/ValveSoftware/gamescope/issues/2075);
- DRM defines 16-bit integer (`XR48`/`AR48`) and 16-bit float
  (`XR4H`/`AR4H`) framebuffer formats;
- amdgpu and i915 implement several of those high-precision plane formats;
- nouveau 7.3-rc4 already lists 16-bit-float `XBGR16161616F` and
  `ABGR16161616F` on multiple base/window classes;
- HDMI and DP drivers independently choose link bpc, sometimes without making
  the achieved depth observable to userspace.

The all-maker gap is the integration contract: consume real HDR metadata,
tone/gamut-map on the fly, retain 16-bit-or-float working precision, select
the highest real SDR scanout and link depth, report any narrowing, and behave
the same way across vendors.

### Signal design

```text
HDR10/HLG elementary stream
        ↓ decode + mastering/content-light metadata
linear/high-precision libplacebo processing
        ↓ tone map + gamut map
BT.709 SDR, 16-bit/float scanout where supported; otherwise XR30
        ↓ nouveau HDMI output
highest verified 10/12/16-bpc RGB link → SDR display
```

The working target is FP16 or an equivalently justified high-precision format.
The preferred scanout tier is a supported 16-bit integer/float DRM format;
`XR30` is the practical 10-bit fallback. Scanout precision and cable precision
remain independent: a 10-bit buffer may be carried by a 12-bpc HDMI link, while
a 16-bit buffer may still be quantized to a 10- or 12-bpc link. Every narrowing
must be measured and reported.

### Prototype order

1. Use FFmpeg to establish decoded colour metadata and deterministic owned
   fixtures.
2. Use libplacebo directly, or mpv's `gpu-next` path as the first client, to
   perform real-time tone and gamut mapping to BT.709 SDR.
3. First verify rendered pixel values against an offline reference and retain
   hashes/settings for every fixture.
4. Bypass desktop ambiguity with a controlled DRM/GBM path if the active
   compositor cannot guarantee the requested 10/16-bit scanout.
5. Compare the same pipeline over amdgpu, i915 where available, nouveau, and
   the proprietary NVIDIA control, recording achieved scanout and link depth.
6. Test gradients and difficult highlights at 8-, 10-, 12-, and 16-bit stages
   wherever each stage is genuinely supported, while keeping tone mapping and
   display settings fixed.
7. Preserve a 16-bit offline reference so every real-time backend can be
   compared numerically rather than only by photographing a panel.

### Acceptance

- playback remains real-time at the target 1080p frame rates;
- input PQ/HLG and mastering metadata are logged, not inferred from filename;
- the output is explicitly BT.709 SDR and does not send an HDR EOTF;
- the scanout buffer format is verified rather than assumed from renderer
  precision;
- the sink or immutable `link bpc` property reports the achieved 10/12/16-bpc
  transport depth;
- every vendor either reaches the requested tier or returns an explicit,
  recorded fallback;
- objective pixel comparison passes before subjective banding observations
  are recorded.

## Phase C — native HDR later

Only after the deep-colour path is understood should nouveau expose
`HDR_OUTPUT_METADATA`, Colorspace, construct the 30-byte DRM InfoFrame, and
emit it through the generation-specific generic packet route. Final acceptance
then requires an HDR sink or protocol analyzer.

Phase B is therefore not a consolation prize. It produces a useful open
high-quality path for excellent SDR displays while Phase C acquires the
hardware needed for native-HDR validation.
