---
name: awesome-linux-hdr-contributing
description: Operating rules for contributing to awesome-linux-hdr (HDR and deep colour on Linux) with AI assistance. Loads the list's own contribution rules plus the verification discipline that makes them achievable. Use before researching, adding, correcting or removing any entry.
---

# Contributing to awesome-linux-hdr with AI assistance

Two halves. The first is what this list requires. The second is how to actually meet it, learned by getting it wrong first while building the list.

**Check freshness first.** These rules were written on 2026-09-23. If [contributing.md](https://github.com/danielcamposramos/awesome-linux-hdr/blob/main/contributing.md) or [PROVENANCE.md](https://github.com/danielcamposramos/awesome-linux-hdr/blob/main/PROVENANCE.md) has changed since, those files win and this skill is history.

---

# Part zero: the posture, which decides whether the rest happens

Everything below is a rule. Rules are followed when someone is watching. This part is about what to be when nobody is, and it matters more than any single item.

**Act as a valued senior partner, not as an eager assistant.** A senior partner is valued because they will tell you when you are wrong. They ask the awkward question before the work ships, not after it is reverted. They say "I could not verify that" out loud, and early, because a stated gap is cheap and a discovered one is expensive.

- **Push back.** If the human asks for a claim the sources do not support, say so and say why. Agreeing is not helpfulness. It is how a wrong sentence ships with two people's confidence behind it instead of one.
- **Refuse to produce what you cannot support.** No invented link, no quotation you have not read, no number you cannot point at. "I do not have a source for that" is a complete and professional answer.
- **Own the error first.** When you find your own mistake, name it before anyone else does, and name it specifically.
- **Protect the list from your own output.** You are the last check before a stranger reads it and believes it.

**On the tool question.** Software settled this argument once already, over the word *hacker*: the capability is neutral, the conduct is what we judge. The same holds here. What separates good use of these tools from bad is not the model. It is whether the person behind it verified, understood and owned what they published. The maintainer's position is written in [PROVENANCE.md](https://github.com/danielcamposramos/awesome-linux-hdr/blob/main/PROVENANCE.md#on-slop): we judge the artefact, not the author. Linus Torvalds works the same way: his AI-assisted drm/xe fix, [818bebeb63dd](https://github.com/torvalds/linux/commit/818bebeb63dd6bf5f4e07e145f6cdbace520a34c), was a stubborn human directing, a verified result and an honest disclosure in the commit itself.

---

# Part one: what this list requires

## Scope

The Linux path required to create, decode, render, transport, display, diagnose or repair HDR and wide-colour output, including SDR deep colour. Cross-platform projects belong when their Linux implementation is relevant.

## Not in scope

- General photography, display shopping advice and AI HDR-image synthesis.
- Title-by-title game compatibility lists.

## The quality bar

- **Check the link before submitting.** Every entry was verified when it was added.
- One line per entry, a factual description ending with a period.
- Say what a thing *is*, not how good it is.
- Abandoned, unmaintained or shut-down projects are labelled honestly, with a date where known.
- Never work around a site that blocks automated access. Leave the source out and say so.
- Prefer specifications, upstream source, maintainer documentation and reproducible measurements over summaries.
- Label evidence as normative, implemented, measured, reported or proposed.
- Name hardware and software versions for measurements.
- Separate deep colour, wide gamut, transfer function, metadata, sink state and measured light output.
- Link standards; never upload copyrighted copies.

## The pull-request checklist

The template asks you to confirm: every added link opened by hand and read, publisher pages rather than copies, one entry per line, factual descriptions, honest labels, and whether AI assistance was used. Branch `main` is protected: a pull request needs the maintainer's review and a passing lint check.

---

# Part two: how to actually meet it

These are not from the contribution guide. They are what it takes to satisfy it.

## The rule the others serve

**Never let the tool mark its own homework.** An assistant that reports "I verified this" has verified nothing. Verification is a fetch you can see, a page you read, or a page a human opened.

## Links, which is where list entries actually fail

1. **Open every link and read the page.** Not the title, not a search snippet, not a summary.
2. **Check content, not status codes.** A block often arrives as HTTP 200 with a denial page in the body. A dead link often arrives as HTTP 200 after a silent redirect to a home page. Read what came back.
3. **A challenge word inside a page script is not a block.** Some pages carry "captcha" or "challenge" text in their JavaScript and load fine. Decide by the visible title and body, not by a grep.
4. **If a site blocks automated access, stop.** Do not change the user agent, do not retry with other headers, do not route around it. Record the URL and hand it to the maintainer to open in a browser. Circumventing an access control is not a research technique, whatever the goal.
5. **A dead link gets one more chance: the Wayback Machine.** If an archived snapshot shows the genuine page, link the snapshot and say so. If not, drop the entry. Check what the snapshot actually contains: an archived domain can hold an unrelated site from a later owner.
6. **Link the publisher, not a copy.** A paper links to its publisher or DOI page, even when only a third-party PDF is reachable. Mark paywalled sources as paywalled.
7. **Verify identifiers against the page they point to.** A standard's number and its catalogue URL must match. A plausible URL from a model or a search can point at a different document entirely.
8. **Extract URLs with care.** URLs can contain parentheses and percent-encoding. A naive regex cuts them and produces a link that fails for reasons that have nothing to do with the site.
9. **Pace your requests.** Search and wiki APIs rate-limit rapid calls. Space them out and back off when refused; hammering a service gets the maintainer's address blocked.

## Writing entries

10. **One entry per line:** `- [Name](https://absolute-url) - What it is, ending with a period.` No line breaks inside an entry, and absolute URLs only; awesome-lint rejects relative links in the readme.
11. **Say what it is, not how good it is.** No marketing language. Do not repeat a vendor's superlative as fact; "the first" needs a source that says so.
12. **Dates on history.** In this subject the date usually explains the thing.
13. **Honest status.** Abandoned, shut down, paywalled or unreliable goes in the description, with the date where known.
14. **Keep the lint baseline.** The pull-request check fails only if you add awesome-lint errors above what the branch already has. Run `npx awesome-lint readme.md` before and after your change.

## Being honest about it

15. **Disclose the assistance** in the pull request. The template has a checkbox. It is never held against you.
16. **State what you did not verify.** "This source was unreachable, so the entry is held for a human" is a useful sentence. A stated limit is something a reviewer can act on; a hidden one becomes their problem later.
17. **One pull request, one logical change.** It is easier to review, and easier to revert.

## Specific to this list

18. **"Property exposed" is not "packet emitted" is not "sink shows HDR" is not "photometrically correct".** Say which of the four your source proves. Most driver claims prove only the first.
19. **Link the exact source line for driver behaviour.** A property or InfoFrame claim points at the kernel documentation or the file and function that implement it, pinned to a version.
20. **Say what you could not test.** If you have no HDR display or no analyser, the entry says the claim is argued from the specification, not measured.
21. **Keep stereo out.** 3D formats belong in [awesome-stereoscopy](https://github.com/danielcamposramos/awesome-stereoscopy); raw bench measurements stay in [sony-bravia-linux](https://github.com/danielcamposramos/sony-bravia-linux).

---

# Where these came from

Not theory. Building this list, and the bench work behind it, produced:

- An ISO gain-map standard page that refused automated checks and went in only after the owner opened it by hand.
- A 12-bit deep-colour patch for nouveau that was correct on paper, and that the TV still refused. On the bench, a later audio step turned out to overwrite the General Control Packet that announces the colour depth. Only the measurement found that, and only the fix that preserved the packet passed.
- The owner's own scope note, kept in the list: no HDR display on the bench, so the HDR links are argued from the specifications, not measured.

Every rule in part two is one of those, written down so the next person does not pay for it again.
