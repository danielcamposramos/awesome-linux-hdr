# Federated Signal Ledger pointers

Generated from local pointer files and live upstream append-only ledgers. Do not edit this view.
Pointer digest: `abb42fdc687eecb8a1f65cf86bf201b58f384b086dfa2573cb30808ddbda959c`.

| Pointer | Upstream claim | State | Purpose |
|---|---|---|---|
| `ALH-SBL-3D-0001` | [SBL-3D-0001](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-3d-0001.toml) | supported / SUPPORTED | Bind the run-34 22-step DRM/KMS matrix that combines deep colour, RGB/YCbCr formats and stereoscopic modes. |
| `ALH-SBL-3D-0002` | [SBL-3D-0002](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-3d-0002.toml) | supported / SUPPORTED | Bind Daniel's physical observation that every accepted run-34 mode displayed, including 3D at 12 bpc. |
| `ALH-SBL-DC-0001` | [SBL-DC-0001](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0001.toml) | supported / SUPPORTED | Ground the HDMI Deep Color General Control Packet fields cited by the SDR deep-colour guide. |
| `ALH-SBL-DC-0002` | [SBL-DC-0002](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0002.toml) | supported / SUPPORTED | Bind the nouveau 12-bpc driver-state measurement used by the implementation roadmap. |
| `ALH-SBL-DC-0003` | [SBL-DC-0003](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0003.toml) | supported / SUPPORTED | Bind Daniel's physical observation that the HX855 accepted the nouveau 12-bpc test signal. |
| `ALH-SBL-DC-0004` | [SBL-DC-0004](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0004.toml) | supported / SUPPORTED | Bind the NVIDIA 615.71.09 parameter-state measurement used as the proprietary control. |
| `ALH-SBL-DC-0005` | [SBL-DC-0005](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0005.toml) | supported / SUPPORTED | Bind Daniel's physical observation that the HX855 accepted the NVIDIA 12-bpc test signal. |
| `ALH-SBL-DC-0006` | [SBL-DC-0006](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0006.toml) | supported / SUPPORTED | Bind the bounded interpretation of the software-visible nouveau GCP payload without claiming cable emission. |
| `ALH-SBL-DC-0007` | [SBL-DC-0007](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0007.toml) | undetermined / UNTESTED | Preserve the local 16-bpc hardware gap as untested rather than positive or negative evidence. |
| `ALH-SBL-DC-0008` | [SBL-DC-0008](https://github.com/danielcamposramos/sony-bravia-linux/blob/main/data/evidence-ledger/claims/sbl-dc-0008.toml) | undetermined / UNTESTED | Preserve native HDR output as locally untested because the measurement bench has no HDR sink. |

Pointers bind the canonical semantic hash of each upstream claim; they do not copy its body.
A changed hash, missing claim, unexpected authority state, or disallowed correction/supersession fails CI.
