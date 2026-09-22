# From one bit to HDR: colour depth explained

How many colours a system can show, why each step was taken, what more bits
do and do not buy, and where HDR departs from "just more bits". Figures are
exact powers of two; sources are linked where a claim is not arithmetic.

## The ladder

| bits per pixel | levels per channel | colours | typical use |
|---|---|---|---|
| 1 | — | 2 | monochrome displays and printers |
| 2 | — | 4 | [CGA](https://en.wikipedia.org/wiki/Color_Graphics_Adapter) 320×200 graphics (1981) |
| 4 | — | 16 | CGA's full palette, [EGA](https://en.wikipedia.org/wiki/Enhanced_Graphics_Adapter) on-screen colours (1984) |
| 8 | — | 256 | [VGA](https://en.wikipedia.org/wiki/Video_Graphics_Array) palette modes (1987) |
| 15 | 32 | 32,768 | [high colour](https://en.wikipedia.org/wiki/High_color) 5-5-5 |
| 16 | 32 / 64 / 32 | 65,536 | high colour 5-6-5 |
| 18 | 64 | 262,144 | VGA's palette range; 6-bit LCD panels |
| 24 | 256 | 16,777,216 | "true colour", the SDR desktop |
| 30 | 1,024 | 1,073,741,824 | 10-bit deep colour, HDR10 |
| 36 | 4,096 | 68,719,476,736 | 12-bit deep colour |
| 48 | 65,536 | 281,474,976,710,656 | 16-bit deep colour |

Up to 8 bits, the number is usually an **index** into a palette: 256 entries
chosen from a larger range (VGA picked its 256 from 262,144). From 15 bits
up, the number **is** the colour, split into red, green and blue. In 5-6-5,
green gets the extra bit because the eye is most sensitive to it.

"32-bit colour" is 24 bits of colour plus 8 bits of alpha or padding; it
shows no more colours than 24-bit. `XRGB2101010` spends the same 32 bits as
10-10-10 colour with 2 bits spare.

## Why each step happened

- **Memory.** A 640×480 frame at 8 bits is 300 KiB; at 1024×768, 24 bits is
  2.25 MiB. Palettes existed because frame memory was the scarce resource.
  Today a 1080p frame is 7.91 MiB at 32 bits and 15.82 MiB in 16-bit float.
- **The converter.** Each output step needs a digital-to-analogue converter
  (or a digital link) with that many levels. VGA's DAC had 6 bits per
  channel, which is why its palette range was 262,144.
- **Smooth gradients.** The visible defect of too few levels is
  [banding](https://en.wikipedia.org/wiki/Colour_banding): a smooth ramp
  broken into steps. 256 levels per channel are usually enough for SDR, but
  dark gradients, calibration LUTs and repeated processing expose the steps.
  That is the case for 10 and 12 bits on SDR ([SDR deep colour](sdr-deep-colour.md)).
- **HDR.** A wider luminance range spread over the same number of codes
  makes each step larger. HDR therefore needs more bits to stay step-free
  (below).

## Caveats that trip people up

- **More bits is not more gamut.** Bit depth decides how finely the range is
  divided. Which colours exist at all (the gamut) is set by the primaries:
  BT.709 for SDR, BT.2020 as the usual HDR container. A 12-bit BT.709 signal
  has no colour an 8-bit BT.709 signal lacks; it has finer steps between them.
- **More colours than you can see is the point, not a boast.** The "billions"
  figure counts combinations; what matters is whether the step between two
  neighbouring levels is visible in a smooth gradient.
- **The transfer function decides where codes go.** SDR gamma spends more
  codes on dark tones, where the eye notices steps, which is why 8 bits
  suffice at SDR brightness. HDR uses a curve designed for its range (see PQ
  below).
- **Video usually uses less than the full range.** Limited-range video puts
  8-bit luma at 16–235: 220 levels of 256 (10-bit: 64–940, 877 of 1,024;
  12-bit: 256–3,760, 3,505 of 4,096). Chroma runs 16–240. Desktops send full
  range 0–255. If sender and display disagree about which is in use, blacks
  crush or wash out. `Broadcast RGB` and the CTA-861 QS rule exist for this
  ([rules](sdr-deep-colour.md#rules-a-source-must-follow)).
- **Chroma subsampling is not depth.** YCbCr 4:2:2 and 4:2:0 halve colour
  resolution, not colour precision. HDMI carries 4:2:2 at up to 12 bits in
  the same space as 8-bit RGB.
- **Panel, link and framebuffer are three different depths.** Many "8-bit"
  panels are 6-bit with [frame rate control](https://en.wikipedia.org/wiki/Frame_rate_control),
  alternating neighbouring levels over time, and many "10-bit" panels are
  8-bit with FRC. A 10-bit framebuffer can ride a 12-bit link; a 16-bit one
  can be cut to 10. A television's information screen reports the depth it
  received on the link, which says nothing about the panel.
- **Dithering trades steps for grain.** When depth is reduced, adding fine
  noise hides banding. It is a deliberate choice and should be reported as
  one, not mistaken for true depth.

## Where HDR is different

HDR is not a deeper version of SDR. It changes four things at once:

1. **Luminance range.** The PQ curve ([ST 2084](https://en.wikipedia.org/wiki/Perceptual_quantizer))
   encodes 0 to 10,000 cd/m², against the roughly 100 cd/m² that SDR
   television was designed around.
2. **Transfer function.** PQ is built on human perception of banding and is
   designed to show no visible banding at 12 bits; HDR10 uses 10. HLG takes a
   different, backward-compatible approach. Both are defined in
   [BT.2100](https://www.itu.int/rec/R-REC-BT.2100/en), explained in ITU's
   [Report BT.2390](https://www.itu.int/pub/R-REP-BT.2390).
3. **Gamut.** BT.2020 primaries as the container, wider than BT.709.
4. **Metadata.** Mastering-display and content-light information travels to
   the display, which maps the content to what it can actually show.

So a 12-bit SDR link is deep colour, not HDR: same curve, same gamut, finer
steps. HDR content can still be tone-mapped to SDR and carried on that link,
which is this list's [high-SDR](readme.md#how-to-read-this-list) goal.

## Floating point

Compositors and renderers increasingly work in 16-bit floating point, for
example [scRGB](https://en.wikipedia.org/wiki/ScRGB): linear light, with
values above 1.0 for HDR highlights and below 0 for colours outside the
sRGB gamut. The colour count stops being a useful number; precision is
relative to the value, like a camera's exposure. The final output is still
quantized to the link's 8, 10, 12 or 16 bits, and that last step is where
depth is won or lost.
