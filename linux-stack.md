# Linux HDR stack map

The signal path is ordered from the display inward. A link in source proves
implementation, not successful hardware output.

## Kernel common layer

- [`HDR_OUTPUT_METADATA` UAPI](https://github.com/torvalds/linux/blob/master/include/uapi/drm/drm_mode.h) -
  Userspace supplies static metadata as a connector blob.
- [Connector HDR property helpers](https://github.com/torvalds/linux/tree/master/drivers/gpu/drm) -
  Create, attach, duplicate, validate, and destroy connector state.
- [`drm_hdmi_infoframe_set_hdr_metadata()`](https://github.com/torvalds/linux/blob/master/drivers/gpu/drm/display/drm_hdmi_helper.c) -
  Converts connector metadata into the HDMI DRM InfoFrame structure.
- [EDID display information](https://github.com/torvalds/linux/blob/master/drivers/gpu/drm/drm_edid.c) -
  Parses CTA blocks and display colour/HDR declarations.

## Driver map

| driver | readable HDR surface | current evidence boundary |
|---|---|---|
| i915/xe display | Connector properties and HDMI/DP metadata paths exist in-tree | Reference implementation; hardware coverage varies by generation. |
| amdgpu DC | Connector properties, colour state, and HDMI/DP metadata paths exist in-tree | Community-repairable; individual colour/link defects still require hardware testing. |
| nouveau | No HDR metadata property or DRM InfoFrame path in Linux 7.3-rc4 | [Source audit](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/docs/research/nouveau-hdr-gap-2026-09-22.md); implementation project, not output-tested HDR. |
| proprietary NVIDIA | Open `nvidia-drm` glue exposes metadata and hands it to NVKMS | Output policy is closed; [#1384](https://github.com/NVIDIA/open-gpu-kernel-modules/issues/1384) asks NVIDIA to identify the ownership boundary. |

## Compositor boundary

- [color-management-v1](https://wayland.app/protocols/color-management-v1) -
  Standardized Wayland protocol contract.
- [KWin](https://invent.kde.org/plasma/kwin) - Open compositor implementation
  with DRM, colour pipeline, and HDR policy in one inspectable tree.
- [Mutter](https://gitlab.gnome.org/GNOME/mutter) - GNOME compositor and KMS
  backend.
- [wlroots](https://gitlab.freedesktop.org/wlroots/wlroots) - Library used by
  multiple compositors; support must be checked by version and backend.
- [gamescope](https://github.com/ValveSoftware/gamescope) - Gaming compositor
  joining Vulkan presentation, HDR metadata, tone mapping, and KMS.

## Application and renderer boundary

- [libplacebo](https://code.videolan.org/videolan/libplacebo) - Colour-space
  conversion, gamut mapping, tone mapping, and HDR metadata logic.
- [mpv](https://github.com/mpv-player/mpv) - Uses libplacebo in its modern GPU
  renderer and is a useful end-to-end playback client.
- [Mesa Vulkan WSI](https://gitlab.freedesktop.org/mesa/mesa) - Presentation
  formats, colour spaces, and direct-display paths beneath Vulkan clients.
- [FFmpeg](https://github.com/FFmpeg/FFmpeg) and
  [GStreamer](https://gitlab.freedesktop.org/gstreamer/gstreamer) - Decode and
  transport content metadata; neither alone proves compositor or KMS output.

## NVIDIA's two open repair lanes

The proprietary and nouveau findings share a standards contract but not a
code path:

1. `nvidia-drm` already translates HDR connector state into a closed NVKMS
   request. Link policy changes there require NVIDIA.
2. nouveau lacks the KMS surface and 30-byte packet route in readable code.
   Its upper plumbing is straightforward; generation-specific packet emission
   remains the hardware-facing research boundary.

Calling the second a “port” is architectural shorthand, not literal code
reuse from NVKMS.
