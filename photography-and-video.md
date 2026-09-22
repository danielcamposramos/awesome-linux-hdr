# Photography and video: HDR and deep colour from capture to delivery

The display chain in this list starts where an image already exists. This page
follows the image before that: how cameras capture more than 8 bits, how the
formats keep or lose that depth, and which Linux tools preserve it. The scope
rule matches [awesome-stereoscopy](https://github.com/danielcamposramos/awesome-stereoscopy)'s
capture sections: signal-chain facts, not photography technique.

## Two meanings of "HDR"

The most common confusion in this subject is a name clash.

- **HDR imaging** (photography, rendering) captures or computes more scene
  dynamic range than one exposure holds, usually by merging bracketed
  exposures, and has existed since the 1990s. Its results were long
  **tone-mapped back to SDR** for ordinary screens and prints, which produced
  the familiar "HDR photo" look.
- **HDR display signals** (video, and now stills) send a wider luminance range
  to a capable display using PQ or HLG, BT.2020 primaries and metadata; see
  [From one bit to HDR](colour-depth-explained.md#where-hdr-is-different).

An "HDR photo" from the 2000s is an SDR image. A 2020s gain-map photo or HDR10
video is an HDR signal. Both are legitimate; they are different things.

## History

- [High-dynamic-range imaging](https://en.wikipedia.org/wiki/High-dynamic-range_imaging) -
  Overview of the imaging sense: bracketing, merging and tone mapping.
- [RGBE / Radiance HDR](https://en.wikipedia.org/wiki/RGBE_image_format) -
  Greg Ward's format for the Radiance renderer: 8-bit RGB with a shared
  exponent, one of the first HDR image files.
- [Debevec and Malik, radiance maps (SIGGRAPH 1997)](https://www.pauldebevec.com/Research/HDR/) -
  Recovering scene radiance from ordinary exposures, the method behind
  bracketed HDR merging.
- [OpenEXR (ILM 1999, public 2003)](https://en.wikipedia.org/wiki/OpenEXR) -
  Industrial Light & Magic's floating-point image format, released as open
  source and now the film-industry standard; project site
  [openexr.com](https://openexr.com/).
- [Dolby Vision (2014)](https://en.wikipedia.org/wiki/Dolby_Vision) -
  Dolby's HDR video system: PQ with dynamic metadata, up to 12-bit colour.
- [HDR10 (CTA, 27 August 2015)](https://en.wikipedia.org/wiki/HDR10) - The
  open baseline: 10-bit PQ, BT.2020, static metadata.
- [Hybrid log-gamma (BBC and NHK, ARIB STD-B67, 2015)](https://en.wikipedia.org/wiki/Hybrid_log%E2%80%93gamma) -
  Broadcast HDR that stays watchable on SDR sets; see also
  [BBC R&D's HDR project](https://www.bbc.co.uk/rd/projects/high-dynamic-range).
- [Ultra HD Blu-ray (14 February 2016)](https://en.wikipedia.org/wiki/Ultra_HD_Blu-ray) -
  The first mass HDR video medium, with HDR10 mandatory.
- [HDR10+](https://hdr10plus.org/) - HDR10 with dynamic metadata.

## Capture depth

- [Raw image formats](https://en.wikipedia.org/wiki/Raw_image_format) -
  Camera sensors record typically 12 or 14 bits per photosite; an 8-bit JPEG
  keeps a fraction of that.
- [Log profiles](https://en.wikipedia.org/wiki/Log_profile) - Camera video
  curves that spread the sensor's range across the code values, recorded at
  10 bits or more so grading has room; 8-bit log bands easily.

## Still formats

- [JPEG XL](https://jpeg.org/jpegxl/) - JPEG's successor with high bit depth
  and HDR support.
- [AVIF](https://aomediacodec.github.io/av1-avif/) - AV1-coded stills with
  10- and 12-bit and HDR transfer functions.
- [Ultra HDR image format](https://developer.android.com/media/platform/hdr-image-format) -
  A normal SDR JPEG plus a gain map that lets HDR displays reconstruct the
  highlights, so one file serves both kinds of screen.
- [OpenEXR](https://openexr.com/) - Floating-point stills for production and
  rendering (dated above).

## Video formats

- [ITU-T H.265 (HEVC)](https://www.itu.int/rec/T-REC-H.265) - Main 10
  profile carries the 10-bit HDR10 and HLG streams most viewers receive.
- [AV1 bitstream specification](https://aomediacodec.github.io/av1-spec/) -
  10- and 12-bit profiles with HDR metadata (also listed under
  [standards](standards.md)).
- [Apple ProRes white paper](https://www.apple.com/final-cut-pro/docs/Apple_ProRes.pdf) -
  The production intermediate family: 10-bit 4:2:2 and 12-bit 4:4:4 variants.

## Linux tools that keep the depth

- [darktable: scene-referred workflow](https://docs.darktable.org/usermanual/stable/en/overview/workflow/process/) -
  Raw development in floating point, with tone mapping as an explicit final
  step.
- [RawTherapee](https://rawtherapee.com/) - Raw developer processing in
  high precision.
- [GIMP 2.10 release notes](https://www.gimp.org/release-notes/gimp-2.10.html) -
  The release that brought high bit depth editing to GIMP.
- [Krita: scene-linear painting](https://docs.krita.org/en/general_concepts/colors/scene_linear_painting.html) -
  Painting in linear floating point, including for HDR output.
- [Luminance HDR](https://github.com/LuminanceHDR/LuminanceHDR) - Merges
  bracketed exposures and tone-maps them: HDR imaging in the older sense.
  Slow-moving: last release v2.6.0 (July 2019), last commit June 2025.

FFmpeg, libplacebo and mpv, which carry these files to the screen, are in the
[readme](readme.md).

## Caveats

- **An 8-bit file cannot regain what it lost.** Converting a JPEG to 10-bit or
  an 8-bit stream to HDR adds no information; the depth must exist at capture
  or render time.
- **Tone-mapped is not HDR.** An image squeezed to SDR looks like SDR on every
  display; only a PQ/HLG signal or a gain map carries the range onward.
- **Metadata can outlive its truth.** Static HDR metadata describes the
  mastering display; a re-encode that changes the picture without updating it
  misleads the display's tone mapping.
