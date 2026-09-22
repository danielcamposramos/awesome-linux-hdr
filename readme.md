# Awesome Linux HDR [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Linux HDR from specification to photons: formats, metadata, DRM/KMS,
> drivers, compositors, applications, diagnostics, and measurement.

HDR is not one switch. A working path must preserve transfer function,
colour primaries, mastering metadata, pixel depth, link format, and display
state across every layer. This list follows that chain and records where it
can be inspected and repaired.

*AI worked as an intelligent research partner in the development of this
list—see [provenance](PROVENANCE.md).*

## Contents

- [How to read this list](#how-to-read-this-list)
- [Start here](#start-here)
- [Standards and formats](#standards-and-formats)
- [Linux display stack](#linux-display-stack)
- [Project roadmap](#project-roadmap)
- [Rendering and playback](#rendering-and-playback)
- [Tools](#tools)
- [Measurement and test material](#measurement-and-test-material)
- [Repair maps and case studies](#repair-maps-and-case-studies)
- [Related lists](#related-lists)
- [Known gaps](#known-gaps)

## How to read this list

Entries and project claims should keep these evidence classes separate:

- **Normative** — a specification defines the required syntax or behaviour.
- **Implemented** — readable source contains the path; this does not prove it
  runs on every device.
- **Measured** — named hardware and software produced a recorded result.
- **Reported** — a third party describes a result that has not been locally
  reproduced.
- **Proposed** — a design or patch exists but has not completed validation.

Deep colour, wide colour gamut, and HDR are related but not interchangeable.
A 12-bpc link is useful transport evidence; it is not by itself proof of an
HDR transfer function or metadata reaching the display.

**High-SDR** in this project means HDR decoded and tone/gamut-mapped in real
time through a 16-bit or floating-point working pipeline, then delivered as
ordinary SDR using the highest verified scanout and link precision the output
supports (10, 12, or 16 bpc). It does not mean tagging an SDR display as HDR.

## Start here

- [Standards and formats](standards.md) - The public normative chain and the
  boundary around licensed specifications.
- [SDR deep colour](https://github.com/danielcamposramos/awesome-linux-hdr/blob/main/sdr-deep-colour.md) - History, specifications and the signalling rules for 10-, 12- and 16-bit SDR output.
- [Linux stack map](linux-stack.md) - Kernel, drivers, compositors, APIs, and
  applications in signal order.
- [Testing HDR honestly](testing.md) - A layered verification ladder that
  prevents property exposure from being mistaken for light on the wire.
- [Open implementation roadmap](roadmap.md) - nouveau deep colour first,
  followed by real-time HDR-to-SDR rendering over a high-bit-depth link.
- [HDR-to-high-SDR prior art](prior-art.md) - Existing software and hardware
  processors, the owner-provided Gemini assessment, and the remaining open
  Linux integration gap.
- [Linux DRM KMS documentation](https://docs.kernel.org/gpu/drm-kms.html) -
  The kernel's display-mode-setting architecture.
- [Wayland color-management-v1](https://wayland.app/protocols/color-management-v1) -
  The protocol for communicating image descriptions and output colour state.

## Standards and formats

- [ITU-R BT.2100](https://www.itu.int/rec/R-REC-BT.2100/en) - HDR television
  image parameters, including PQ and HLG systems.
- [ITU-R BT.2020](https://www.itu.int/rec/R-REC-BT.2020/en) - UHD television
  primaries, signal formats, and bit-depth vocabulary used by HDR systems.
- [CTA-861.3-A](https://shop.cta.tech/products/cta-861-3) - Static HDR
  capability metadata and the Dynamic Range and Mastering InfoFrame.
- [HDMI 2.0a announcement](https://hdmiforum.org/hdmi-forum-inc-release-2-0a-specification/) -
  HDMI Forum's public record of HDR transport being added through CEA-861.3.
- [Vulkan `VK_EXT_hdr_metadata`](https://registry.khronos.org/vulkan/specs/latest/man/html/VK_EXT_hdr_metadata.html) -
  Application API for supplying static HDR metadata to a swapchain.
- [Matroska colour elements](https://www.matroska.org/technical/elements.html) -
  Container fields for matrix coefficients, transfer characteristics,
  primaries, mastering metadata, and content light levels.

See [standards.md](standards.md) for the connected map and citation notes.

## Linux display stack

- [Linux DRM](https://github.com/torvalds/linux/tree/master/drivers/gpu/drm) -
  Connector capabilities, atomic state, EDID parsing, colour management, and
  driver-specific output programming.
- [DRM HDR metadata UAPI](https://github.com/torvalds/linux/blob/master/include/uapi/drm/drm_mode.h) -
  `struct hdr_output_metadata`, the cross-driver userspace contract.
- [DRM HDMI helper](https://github.com/torvalds/linux/blob/master/drivers/gpu/drm/display/drm_hdmi_helper.c) -
  Builds the HDMI Dynamic Range and Mastering InfoFrame from connector state.
- [Mesa](https://gitlab.freedesktop.org/mesa/mesa) - OpenGL/Vulkan drivers and
  WSI paths between applications, compositors, and KMS.
- [Wayland protocols](https://gitlab.freedesktop.org/wayland/wayland-protocols) -
  Home of the standardized colour-management protocol.
- [KWin](https://invent.kde.org/plasma/kwin) - KDE's Wayland compositor and
  one open implementation of HDR and colour-management policy.
- [Mutter](https://gitlab.gnome.org/GNOME/mutter) - GNOME's compositor and
  display server.
- [wlroots](https://gitlab.freedesktop.org/wlroots/wlroots) - Reusable Wayland
  compositor library with DRM backends.
- [gamescope](https://github.com/ValveSoftware/gamescope) - Valve's gaming
  compositor; useful for following HDR, Vulkan, and direct-display work.

The per-driver implementation map lives in [linux-stack.md](linux-stack.md).

## Project roadmap

The first owned-hardware programme deliberately does not require an HDR
display: enable a standards-bounded 12-bpc nouveau HDMI link to an SDR Sony
KDL-46HX855, then render HDR10/HLG sources through a high-precision real-time
tone mapper into a 10-bit scanout buffer carried by that 12-bpc link. amdgpu
is the open implementation reference; the proprietary NVIDIA stack is a
measured behavioural control and an API reference where its glue is open.

The wider target is cross-vendor **HDR-to-high-SDR on the fly**. Linux already
has tone-mapping algorithms, 16-bit integer/float DRM formats, and individual
high-bpc driver paths, but not one automatic, measured contract that preserves
that precision from HDR metadata to an SDR sink across AMD, Intel, NVIDIA, and
nouveau.

See [roadmap.md](roadmap.md) for the separable acceptance gates. A 12-bpc OSD
result proves link training. Correct HDR-to-SDR pixels prove the render path.
Neither is reported as native HDR output.

## Rendering and playback

- [libplacebo](https://code.videolan.org/videolan/libplacebo) - GPU rendering,
  colour management, tone mapping, gamut mapping, and HDR metadata handling.
- [mpv](https://github.com/mpv-player/mpv) - Player whose GPU-next rendering
  path is built on libplacebo.
- [FFmpeg](https://github.com/FFmpeg/FFmpeg) - Codec, container, metadata,
  filtering, and conversion foundation used across the media stack.
- [VLC](https://code.videolan.org/videolan/vlc) - Cross-platform player with
  libplacebo and platform-specific video-output paths.
- [Kodi](https://github.com/xbmc/xbmc) - Media centre with platform-specific
  HDR passthrough and display-mode code worth comparing across operating
  systems.
- [GStreamer](https://gitlab.freedesktop.org/gstreamer/gstreamer) - Media
  framework carrying colourimetry and HDR metadata through caps and elements.
- [OpenColorIO](https://github.com/AcademySoftwareFoundation/OpenColorIO) -
  Production colour-management framework; adjacent to display HDR rather than
  a KMS implementation.

## Tools

- [edid-decode](https://git.linuxtv.org/edid-decode.git/) - Decodes EDID,
  CTA extension blocks, deep-colour declarations, and HDR static metadata.
- [libdrm tests](https://gitlab.freedesktop.org/mesa/drm/-/tree/main/tests) -
  Includes `modetest` for enumerating connector properties and exercising
  atomic modesets.
- [drm_info](https://gitlab.freedesktop.org/emersion/drm_info) - Dumps DRM
  devices, connector properties, formats, and capabilities as structured data.
- [IGT GPU Tools](https://gitlab.freedesktop.org/drm/igt-gpu-tools) - Kernel
  graphics test suite, including KMS validation infrastructure.
- [dovi_tool](https://github.com/quietvoid/dovi_tool) - Parses and manipulates
  Dolby Vision RPU metadata in owned media.
- [hdr10plus_tool](https://github.com/quietvoid/hdr10plus_tool) - Extracts,
  edits, and verifies HDR10+ dynamic metadata.
- [MediaInfo](https://github.com/MediaArea/MediaInfo) - Reports container and
  elementary-stream colour/HDR metadata.

## Measurement and test material

- [EBU test sequences](https://tech.ebu.ch/testsequences) - Broadcaster test
  material, with licensing stated per sequence.
- [AV1 test vectors](https://aomedia.googlesource.com/av1-test-vectors/) -
  Alliance for Open Media decoder conformance material.
- [Testing guide](testing.md) - Separates source inspection, property checks,
  atomic acceptance, packet observation, sink indication, and photometric
  measurement.

An HDMI protocol analyzer can prove InfoFrame bytes. A display OSD can prove
the mode the sink says it received. A colorimeter or spectroradiometer can
measure luminance and chromaticity. None substitutes for all the others.

## Repair maps and case studies

- [nouveau HDR gap audit](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/docs/research/nouveau-hdr-gap-2026-09-22.md) -
  Source-confirmed missing KMS/InfoFrame path in Linux 7.3-rc4, including the
  17-byte-versus-30-byte boundary and newer generic packet machinery.
- [nouveau HDMI Deep Color v2](https://lore.kernel.org/dri-devel/20260922215317.611388-1-Capitain_Jack@yahoo.com/) - Link-depth selection and GCP programming for 30/36/48-bpp RGB; 12- and 10-bpc links measured on GA106 to a Sony KDL-46HX855, including 3D at 12 bpc. 48 bpp is implemented, not measured.
- [nouveau HDMI colour format series](https://lore.kernel.org/dri-devel/20260922215336.612239-1-Capitain_Jack@yahoo.com/) - Broadcast RGB and YCbCr 4:4:4/4:2:2/4:2:0 through the head's output CSC, 25 configurations measured on the same bench. 4:2:0 and DVI are implemented, not measured.
- [NVIDIA issue #1384](https://github.com/NVIDIA/open-gpu-kernel-modules/issues/1384) -
  Measured EDID-surface narrowing on GA106 plus the explicitly qualified HDR
  connection; the reporting bench has no HDR sink.
- [DRM HDR metadata introduction](https://lists.freedesktop.org/archives/dri-devel/2019-March/211334.html) -
  Intel-originated kernel series showing how the common UAPI entered DRM.

## Related lists

- [awesome-stereoscopy](https://github.com/danielcamposramos/awesome-stereoscopy) -
  Depth formats and HDMI 3D signalling; overlaps where EDID and InfoFrame
  machinery carries both depth and colour capabilities.
- [awesome-colour](https://github.com/colour-science/awesome-colour) - Colour
  science, libraries, datasets, and production tools.
- [awesome-browser-hdr](https://github.com/cmahnke/awesome-browser-hdr) - HDR
  behaviour and tooling in browsers.
- [Awesome Gain Maps](https://github.com/NMoroney/Awesome-Gain-Maps) - HDR
  still images represented with gain maps.

## Known gaps

- A vendor-neutral real-time HDR-to-high-SDR contract: metadata-driven tone
  mapping, 16-bit/float working surfaces, verified 10/12/16-bpc scanout/link
  selection, calibration, and an observable fallback when a layer narrows it.
- A generation-by-generation table of HDR property and InfoFrame support for
  amdgpu, i915, nouveau, and the proprietary NVIDIA stack.
- Reproducible compositor test recipes under KWin, Mutter, wlroots, and
  gamescope.
- Open HDR-capable capture hardware and protocol-analyzer workflows.
- Sink-independent KMS tests for metadata replacement, removal, and SDR
  fallback.
- A public matrix separating “property exposed,” “packet emitted,” “sink
  reports HDR,” and “photometrically correct.”

Contributions that close these gaps are welcome; see [contributing.md](contributing.md).
