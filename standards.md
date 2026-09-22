# Standards and formats

This document follows the HDR signal in order. It links public first-party
material and does not mirror copyrighted standards.

## Vocabulary

- **Deep colour** is link or sample precision above 8 bits per component.
- **Wide colour gamut** describes chromaticities beyond the older television
  gamut; BT.2020 primaries are commonly used as the HDR container.
- **HDR** combines an HDR transfer system (normally PQ or HLG) with suitable
  signal precision and display processing.
- **Static metadata** describes the programme or mastering display as a
  whole. **Dynamic metadata** may vary by scene or frame.

A deep-colour link can carry SDR. HDR content can be tone-mapped to SDR.
Neither observation proves the other.

This project uses **high-SDR** for SDR output produced from HDR in real time
with 16-bit/float working precision and the highest verified 10/12/16-bpc
scanout and link available. Working precision, framebuffer format, link depth,
panel depth, and calibration-LUT precision are recorded separately.

## Image system

- [ITU-R BT.2020](https://www.itu.int/rec/R-REC-BT.2020/en) - UHD resolution,
  primaries, matrices, and 10/12-bit signal representations.
- [ITU-R BT.2100 family](https://www.itu.int/rec/R-REC-BT.2100/en) - HDR-TV
  system definition using PQ or HLG.
- [Pinned BT.2100-3 edition](https://www.itu.int/rec/R-REC-BT.2100-3-202502-I/en) -
  In-force February 2025 edition used by this list's dated research.

## Sink declaration and HDMI transport

- [HDMI 2.0a public announcement](https://hdmiforum.org/hdmi-forum-inc-release-2-0a-specification/) -
  Records the addition of HDR formats through the CEA-861.3 extension.
- [CTA-861.3-A official page](https://shop.cta.tech/products/cta-861-3) - HDR
  Static Metadata Data Block and Dynamic Range and Mastering InfoFrame.
- [CTA-hosted CEA-861.3 preview](https://standards.cta.tech/kwspub/published_docs/CEA-861.3-Preview.pdf) -
  Public preview suitable for checking the document's scope and field names.
- [EDID 1.4](https://vesa.org/vesa-standards/) - Base display-identification
  framework; CTA extension blocks carry HDMI and HDR capabilities.

CTA and HDMI normative texts are licensed publications. Their public pages
and previews are citations, not permission to redistribute the documents.

## DVI and DisplayPort

- [DVI 1.0 (DDWG, April 1999, archived copy)](https://glenwing.github.io/docs/DVI-1.0.pdf) -
  The original TMDS link that HDMI extends. A DVI connector drives an HDMI
  sink through a passive adapter, and the source then speaks HDMI
  (InfoFrames, Deep Color, YCbCr) because the sink's EDID declares it.
- [About DisplayPort, VESA](https://vesa.org/displayport-developer/about-displayport/) -
  Public overview of the VESA link; the specification itself is
  member-only.
- [VESA DisplayHDR](https://displayhdr.org/) - VESA's HDR performance
  certification programme for displays.
- [DRM DP dual-mode adaptor helpers](https://docs.kernel.org/gpu/drm-kms-helpers.html#display-port-dual-mode-adaptor-helper-functions-reference) -
  Linux implementation of DP++, which lets a DisplayPort output emit
  TMDS/HDMI through a passive adapter.

DisplayPort carries pixel encoding, colorimetry and HDR metadata in its own
stream attributes and secondary data packets; the HDMI InfoFrame model
applies only where a DVI or DP++ output presents a TMDS link to an HDMI
sink. For an HDR-to-SDR adaptor, the output connector therefore says less
than the sink's EDID about which rules apply.

## Linux and graphics APIs

- [DRM mode UAPI](https://github.com/torvalds/linux/blob/master/include/uapi/drm/drm_mode.h) -
  `drm_hdr_metadata_infoframe` and `hdr_output_metadata` structures visible to
  userspace.
- [DRM connector documentation](https://docs.kernel.org/gpu/drm-kms.html#display-output-abstraction) -
  Connector state and property model.
- [Wayland color-management-v1](https://wayland.app/protocols/color-management-v1) -
  Image descriptions, output descriptions, rendering intents, and protocol
  feature discovery.
- [Vulkan `VK_EXT_hdr_metadata`](https://registry.khronos.org/vulkan/specs/latest/man/html/VK_EXT_hdr_metadata.html) -
  Static HDR metadata supplied to presentation swapchains.

## Containers and codecs

- [Matroska element specification](https://www.matroska.org/technical/elements.html) -
  Colour matrix, range, transfer, primaries, mastering metadata, and content
  light level elements.
- [AV1 bitstream specification](https://aomediacodec.github.io/av1-spec/) -
  Colour configuration and metadata OBUs.
- [FFmpeg colour and mastering metadata types](https://github.com/FFmpeg/FFmpeg/tree/master/libavutil) -
  Widely consumed implementation vocabulary for decoded frames.

Container tags, coded-stream metadata, compositor state, and HDMI packets are
separate copies of related information. Testing must observe each boundary.
