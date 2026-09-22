# Testing HDR honestly

HDR validation is a ladder. Passing one rung does not imply the next.

## Evidence ladder

1. **Source:** the driver attaches the required properties and has a complete
   metadata-to-packet path.
2. **Enumeration:** `drm_info` or `modetest -c` shows the expected connector
   properties and sink capabilities.
3. **Atomic state:** valid property combinations are accepted; invalid blobs,
   depths, formats, and modes are rejected deliberately.
4. **Driver delivery:** tracepoints or instrumented code show the complete,
   checksummed packet reaching the generation-specific output function.
5. **Wire:** an HDMI/DP analyzer captures the metadata packet and link format.
6. **Sink:** the display reports the received mode, bit depth, colour format,
   and HDR state.
7. **Light:** a meter verifies luminance, EOTF tracking, black level, and
   chromaticity.

## Non-invasive inventory

These commands only read state:

```sh
edid-decode /sys/class/drm/cardN-CONNECTOR/edid
modetest -M DRIVER -c
drm_info
```

Record the kernel, driver version, GPU PCI ID, connector, cable path, display
model, EDID hash, desktop session, and every property value. “Same display” is
not enough provenance.

## Deep-colour test

Deep colour is a useful prerequisite test when no HDR sink is available:

1. confirm the EDID declares the target depth for the selected HDMI colour
   format;
2. confirm the driver exposes a depth control such as `max bpc`, or document
   the internal selection policy if it does not;
3. choose a mode whose pixel clock fits the link at that depth;
4. perform a reversible atomic modeset with a deterministic test image;
5. capture the requested connector state and driver log;
6. read the sink's own signal-information OSD;
7. return to the known-good mode and verify the desktop.

An OSD report of 12 bits is measured link evidence. It does not establish PQ,
HLG, HDR metadata, or photometric correctness.

## Safety

- Keep a second control display or remote shell available before a modeset.
- Test one connector and one variable at a time.
- Save the known-good mode and provide an automatic rollback timer.
- Never infer packet contents from an OSD label.
- Do not publish copyrighted test clips or standards; link to their official
  source and record hashes for locally owned fixtures.

## Minimum report table

| layer | result | evidence |
|---|---|---|
| EDID capability | pass/fail | decoded block + EDID SHA-256 |
| property exposure | pass/fail | `drm_info`/`modetest` capture |
| atomic request | pass/fail | exact mode, format, bpc, return code |
| driver packet path | pass/fail/not observed | trace or source boundary |
| on-wire packet | pass/fail/not measured | analyzer capture |
| sink state | pass/fail/not available | OSD or service readout |
| photometric output | pass/fail/not measured | instrument and method |

Use “not measured” freely. It is more useful than collapsing the stack into a
single “HDR works” checkbox.
