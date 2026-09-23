# Provenance

Daniel Campos Ramos founded and directs this project. It grew from owned-
hardware HDMI 3D and deep-colour measurements on a Sony KDL-46HX855, followed
by source audits across the Linux DRM drivers and NVIDIA's open kernel glue.

The launch corpus was developed as collective intelligence:

- **Kimi K3** worked inside Claude Code CLI through the Ollama provider on the
  initial nouveau HDR source survey and driver comparison.
- **OpenAI GPT-5.6 Sol** worked inside Codex CLI on the Linux 7.3-rc4 audit,
  found the newer Valve GB20x generic InfoFrame implementation and r535
  packet-command candidate, narrowed the hardware claims, and structured this
  list and its verification doctrine.
- **Daniel** directed the investigation, supplied and interpreted the physical
  measurements, chose the public scope, and owns every published claim.

AI partners are named because their reasoning materially shaped the work.
Nothing here treats generated text as measurement: source claims are tied to
source, hardware claims to recorded hardware, and untested boundaries remain
labelled.

## On slop

Slop is not a property of a tool.
It is low information density, claims nobody can check, and volume without checkable content, and people produced all of it long before language models existed.

The largest slop event open source has suffered was human.
In October 2020 Hacktoberfest offered a t-shirt for four pull requests.
By DigitalOcean's own published recap, the result included 34,595 pull requests accepted by no maintainer, 9,598 labelled spam or invalid, and 172,599 aimed at repositories that had not opted in.
The rules were changed to opt-in partway through.
No language model was involved.

Models were trained on human writing, including human padding, human hedging and human false confidence.
Where a model produces slop, a person wrote that slop first and the machine is repeating it faster.

So this project **judges the artefact, not the author**.
A claim is sourced or unsourced, a result is reproduced or not, a page is readable or not, and all of that is visible in the diff without anyone guessing how it was made.
The checks that matter catch bad work whoever wrote it:

- **Read the rendered result**, not the draft.
- **Open every link and reference yourself.** A tool does not mark its own homework: a model saying "yes, the quote is there" is worthless until you have searched the source.
- **Check content, not status codes.** A dead or redirected page can still answer HTTP 200.
- **Tie every hardware claim to a recorded run** on named hardware.

Disclosure here is simple and non-punitive.
Saying "AI assisted, sources checked by me" must never cost a contributor more than saying nothing.
Disclosure lands on the people who were already careful, so it lets a reviewer calibrate how hard to look, but it never replaces looking.

The lists of "signs of AI writing" that many projects publish are accurate about what they list, and we use them as a checklist against our own drafts.
Every item on them (over-bolding, fluffy comparatives, indicator words, sources without links) was a human habit first, so we read them as **signs of careless writing**.
Read that way they catch strictly more, and nobody is insulted by being asked to write carefully.
