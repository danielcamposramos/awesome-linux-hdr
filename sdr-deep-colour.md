# SDR deep colour: history, specifications and rules

Deep colour is older than HDR by more than a decade. More than eight bits per
component on an SDR signal removes banding from gradients and gives
calibration and tone-mapping stages room to work; it changes neither the
transfer function nor the gamut. This page follows it from its first
products to the rules a Linux driver must obey to deliver it, and is the
foundation of this list's [high-SDR](readme.md#how-to-read-this-list) goal.
For the basics (colour counts, caveats, and how HDR differs), start with
[From one bit to HDR](colour-depth-explained.md).

## History

- [Matrox Parhelia-512 (June 2002)](https://en.wikipedia.org/wiki/Matrox_Parhelia) -
  "GigaColor": 10 bits per channel through the pixel pipeline, the
  framebuffer and the RAMDACs, described at launch as the first graphics
  technology to carry true 10-bit channels end to end.
- [xvYCC, IEC 61966-2-4 (January 2006)](https://en.wikipedia.org/wiki/XvYCC) -
  Sony-proposed extended-gamut YCC that uses code values outside the nominal
  range; the first wide-gamut signalling HDMI carried.
- [HDMI 1.3 announcement (22 June 2006)](https://www.hdmi.org/press/bodydetails/68) -
  Deep Color: 30, 36 and 48-bit RGB or YCbCr, up from 24-bit, with the
  single-link TMDS clock raised from 165 to 340 MHz to carry it.
- [NVIDIA 30-bit colour technical brief (June 2009)](https://www.nvidia.com/docs/IO/40049/TB-04701-001_v02_new.pdf) -
  10 bits per component over DisplayPort for professional applications.
- [NVIDIA Linux driver: depth-30 X screens (README, driver 396.51)](https://download.nvidia.com/XFree86/Linux-x86_64/396.51/README/depth30.html) -
  How the proprietary Linux stack exposed 30-bit desktops to X11.
- [Windows Advanced Color (Windows 11 22H2 for SDR)](https://learn.microsoft.com/en-us/windows/win32/direct3darticles/high-dynamic-range) -
  Microsoft's composition path at FP16, extended from HDR displays (2017) to
  specially provisioned SDR displays at 10 bits and above.
- [nouveau HDMI Deep Color (September 2026)](https://lore.kernel.org/dri-devel/20260922215317.611388-1-Capitain_Jack@yahoo.com/) -
  The open NVIDIA driver learns 30/36/48-bpp HDMI, measured at 12 and 10
  bits on an SDR television; see the [colour-format follow-on](https://lore.kernel.org/dri-devel/20260922215336.612239-1-Capitain_Jack@yahoo.com/).

## Specifications

- [ITU-R BT.709](https://www.itu.int/rec/R-REC-BT.709/en) - HDTV parameters:
  the SDR primaries, matrix and the 8- and 10-bit coding ranges.
- [ITU-R BT.1886 (March 2011)](https://www.itu.int/rec/R-REC-BT.1886-0-201103-I/en) -
  Reference EOTF for flat-panel SDR displays in HDTV production.
- [IEC 61966-2-1 (sRGB)](https://webstore.iec.ch/en/publication/6169) - The
  default desktop colour space and its transfer function.
- [IEC 61966-2-4 (xvYCC), Amendment 2](https://webstore.iec.ch/en/publication/65489) -
  Current edition of the extended-gamut YCC standard.
- [DRM fourcc definitions](https://github.com/torvalds/linux/blob/master/include/uapi/drm/drm_fourcc.h) -
  `XRGB2101010` and relatives: the 10-bit scanout formats a compositor can
  hand to the kernel.
- [DRM KMS properties](https://docs.kernel.org/gpu/drm-kms.html) - `max bpc`
  and `Broadcast RGB`, the connector controls for link depth and RGB
  quantization range.

HDMI, CTA-861 and DisplayPort texts are licensed or member-only; see
[standards.md](standards.md) for their public pages. The HDMI Deep Color
capability bits (`DC_30`, `DC_36`, `DC_48`, `DC_Y444`) live in the HDMI
vendor-specific data block of the sink's EDID.

## Rules a source must follow

Each rule is linked to readable code that implements it.

- **Both ends must agree.** The sink declares the depths it accepts in its
  EDID; the source may pick one, and the TMDS character rate rises with it
  (×1.25 at 10 bits, ×1.5 at 12, ×2 at 16) and must fit the sink's declared
  maximum. See [drm_hdmi_helper.c](https://github.com/torvalds/linux/blob/master/drivers/gpu/drm/display/drm_hdmi_helper.c).
- **The source must say so.** The General Control Packet carries the colour
  depth and pixel-packing phase (HDMI 1.4b section 6.5.3); a deep-colour link
  with a default GCP is refused or misread. See
  [dw-hdmi.c](https://github.com/torvalds/linux/blob/master/drivers/gpu/drm/bridge/synopsys/dw-hdmi.c).
- **YCbCr 4:2:2 is the exception.** It carries up to 12 bits in the 24-bit
  container at the pixel clock and declares no deep colour; 4:2:0 halves the
  rate. Same helper as above.
- **Range must match its declaration.** CTA-861 lets a source send a
  non-default RGB quantization range only to sinks that declare the QS bit;
  otherwise the pixels must match the video format's default range. See the
  rule quoted in [drm_edid.c](https://github.com/torvalds/linux/blob/master/drivers/gpu/drm/drm_edid.c).
- **Requested is not achieved.** `max bpc` is a ceiling. The achieved link
  depth comes from the driver's selection and the sink's report; see
  [testing.md](testing.md).
- **Scanout and link are independent.** A 10-bit framebuffer may ride a
  12-bit link and a 16-bit framebuffer may be quantized to 10; each stage is
  reported separately ([roadmap.md](roadmap.md)).
