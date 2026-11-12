# Provenance

Daniel Campos Ramos founded and directs this project.
It grew from owned- hardware [HDMI®](https://www.hdmi.org/resource/brand) 3D and deep-colour measurements on a Sony KDL-46HX855, followed by source audits across the Linux DRM drivers and NVIDIA's open kernel glue.

The launch corpus was developed as collective intelligence:

- **Kimi K3** worked inside Claude Code CLI through the Ollama provider on the initial nouveau HDR source survey and driver comparison.
- **OpenAI GPT-5.6 Sol** worked inside Codex CLI on the Linux 7.3-rc4 audit, found the newer Valve GB20x generic InfoFrame implementation and r535 packet-command candidate, narrowed the hardware claims, and structured this list and its verification doctrine.
- **Daniel** directed the investigation, supplied and interpreted the physical measurements, chose the public scope, and owns every published claim.

AI partners are named because their reasoning materially shaped the work.
Nothing here treats generated text as measurement: source claims are tied to source, hardware claims to recorded hardware, and untested boundaries remain labelled.

The federated Signal Ledger was designed through the real ACIG/MVCIC partner chain (Kimi K3, GLM, DeepSeek, Nemotron and Gemma completed; Qwen's lane returned HTTP 410), implemented by OpenAI Codex, and adversarially reviewed by another Codex partner.
Those reviews are design provenance, not evidence.
Daniel remains the named observer for physical sink behaviour; the consumer registry stores only semantic hashes and authority expectations for canonical claims in `sony-bravia-linux`.

## On slop

Adapted from Daniel's comments on the [Consumer Rights Wiki AI usage policy talk page](https://consumerrights.wiki/w/Consumer_Rights_Wiki_talk:AI_usage_policy) (20 September 2026).

Slop is not a property of a tool.
It is low information density, claims nobody can check, and volume without checkable content, and people produced all of it long before language models existed.

Careless contributions are why contribution rules exist.
In November 2000 the Linux kernel's [patch submission guide](https://static.lwn.net/2000/1123/a/Linus-HOWTO.php3) was already listing them: the worst patch descriptions "include things like 'update driver X'", and code that ignores the style "is likely to be rejected without further review, and without comment".
By 2012 so many projects kept a CONTRIBUTING file that GitHub [built it into the site](https://github.blog/2012-09-17-contributing-guidelines/).
All of that was written for people, years before any language model.

The best counted case was human too.
In October 2020 Hacktoberfest offered a t-shirt for four pull requests.
By DigitalOcean's own published recap, the result included 34,595 pull requests accepted by no maintainer, 9,598 labelled spam or invalid, and 172,599 aimed at repositories that had not opted in.
The rules were changed to opt-in partway through.
No language model was involved.

Models were trained on human writing, including human padding, human hedging and human false confidence.
Where a model produces slop, a person wrote that slop first and the machine is repeating it faster.

**Sloppy means lazy.**
Work costs effort.
The effort is lower when you know the domain, and lower again when you know the domains around it, because then you can direct the work and check it.
With AI partners, work is still work.
It is not leisure, it is not meant to be done in one shot, and it is not meant to be done alone and fast.
A prompt and its first answer are not the work.
They are a pencil storyboard for a film adapted from a book: a sketch of what the work could be.
The work is everything between that sketch and the finished film.
Sloppy work is lazy work: someone chose to skip the effort and passed the result on.
That choice is a person's, with or without a model.

A mistake is not slop.
People make mistakes, and models learned from what people wrote, so they make them too.
What matters is that the mistake is caught and corrected, best in house, before it reaches anyone else.

There is no "more for less" here.
The expectation that a model gives more results for less work does not hold.
Whoever wants results has to think the problem through with their partners, and spend the time, the effort and the tokens to read all the needed context again, every time it is needed.
Skip that and the work is sloppy, whether a user or a synthetic user skipped it.

The bans came without numbers, so we read the records.

**nouveau** merged [its statement](https://gitlab.freedesktop.org/nouveau/wiki/-/merge_requests/64) on 9 October 2026: code must be "the output of your own human brain, not a computer".
The discussion gives no count of bad AI-assisted patches.
Its author wrote that she would "rather be overly defensive of our community now and back off later if needed".
The kernel driver's maintainer, who dislikes these tools, wrote there that nouveau had received "a pretty considerable number of fixes for real issues" with LLM help, "annoying to review (in terms of volume - not quality)", and named several she thought should be merged.
She also wrote that an author who uses a model is "a reviewer of all of their submissions, not an author", who does not "understand it in relation to the rest of the driver".
That is the tool taken as proof of what the author does not understand.
Seven weeks before, the same maintainer had reviewed and committed a nouveau fix carrying `Assisted-by: Claude` ([38125d38d850](https://gitlab.freedesktop.org/drm/misc/kernel/-/commit/38125d38d850)).

**VLC** labels AI use on every merge request, so its record can be counted.
On 12 November 2026, 120 requests carried `AI::Assisted`: 62 merged, 15 closed and 43 still open.
Four carried `AI::Generated`, the label that read `AI::Slop` until 23 September 2026, and none of those was merged.
Of the requests already settled, 81% of the AI-assisted ones were merged, against 88% of all requests opened in 2026.
That is a difference of seven points, measured, where the label had promised slop.

**KDE** has no project-wide policy; KWin [closes AI-written requests on sight](https://invent.kde.org/plasma/kwin/-/work_items/324).
Its merge requests closed without merging were 9.7%, 8.7% and 8.9% of those opened in the first halves of 2024, 2025 and 2026.
From July to September 2026 the requests opened rose 42% over the year before, and the share closed without merging fell from 9.8% to 7.0%.
Of 4,504 open requests, 65 disclosed AI assistance.

**Monado** is still debating a policy.
In 2026 eight of its 385 merge requests named an AI tool; five were merged and two closed.
Its share closed without merging was 8.3% in the first half of the year and 3.6% since.

**SlimeVR** closes AI-assisted pull requests on the tool alone.
In 2026, 7 of its 388 pull requests disclosed AI assistance.

This is a preliminary study: each project's own records, read on 23 and 29 October 2026 (VLC's on 12 November), and only the assistance people disclosed.
It is still more measurement than any of these rules was made with.
A rule made from fear judges the author.
A rule made from evidence judges the work.

Context, tools and tokens are an investment, not a cost.
Preparing them takes longer at first.
After that, every round starts from what the last one learned, and every result can be checked.
That is the scientific method applied to working with AI partners, as the [Collective Intelligence manifesto](https://github.com/danielcamposramos/Knowledge3D/blob/main/docs/SCIENTIFIC_MANIFESTO_COLLECTIVE_INTELLIGENCE.md) argues.

The debate counts one load, the reviewer's.
It is real.
It does not cancel the contributor's.
Before a reviewer is asked for an hour, the contributor has paid for the partners, brought years of hands-on work and knowledge across several domains, and spent the rounds in house reading, testing and correcting.
That load is what a prepared contribution is made of: the failure measured, the fix, its test, one subject at a time.
A rule that refuses the work for its tool spares one load by writing off the other, unread.
On [a merge request of ours](https://code.videolan.org/videolan/vlc/-/merge_requests/10366), on 22 September 2026, a reviewer asked: "why should we take our free time to read it when you did not take time to write it yourself?"
The time was taken.
Every line was read, every claim was sent back to its source, and what could not be found there was cut.

We are one example of that effort being paid, and not the only one.
curl's lead maintainer [credits one collaboration](https://daniel.haxx.se/blog/2026/10/14/a-curl-audit-by-aisle/) with 35 published CVEs in about a year.
PHP took [a buffer-overrun fix](https://github.com/php/php-src/commit/342a7465b3) that credits AI assistance and carries its regression test.
FFmpeg runs [a review bot of its own](https://mail-archive.com/ffmpeg-devel%40ffmpeg.org/msg190115.html), and its developers act on what it finds.

The harm done on purpose has a human record.
In 2020 researchers sent the Linux kernel [deliberately flawed patches under false identities](https://lists.openwall.net/linux-kernel/2021/05/05/1351), and in 2021 435 earlier commits had to be reviewed again.
In 2024 a backdoor was [planted in xz](https://www.openwall.com/lists/oss-security/2024/03/29/4) by a contributor who had earned the project's trust.
A model has nothing to gain.
When AI-assisted work does harm for gain, [a person pointed it there](https://www.anthropic.com/news/detecting-countering-misuse-aug-2025).
Even a harness that works by itself runs with the permissions, the accounts and the targets a person gave it.

Under every objection is one question: can this contributor be trusted?
A tool cannot answer that, for or against.
A record can.
The answer is not an argument.
It is the list of [changes merged outside Sparky Stereo](https://github.com/Sparky-OS/sparky-stereo-os/blob/main/docs/upstream-merges.md), each one read and accepted by that project's own maintainers.
On 12 November 2026 it held 30 entries across 16 projects, two of them fixes the maintainers wrote themselves from our reports, and it keeps growing.
It is also the [attributions](https://github.com/Sparky-OS/sparky-stereo-os/blob/main/ATTRIBUTIONS.md): every person and project this work builds on, by name, from each project's own records.
The same file has been kept in [Knowledge3D](https://github.com/danielcamposramos/Knowledge3D/blob/main/ATTRIBUTIONS.md) since October 2025, a year before this project had one.
Nobody gets that from one prompt.

It is not the tools.
It is the humans behind them.

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

The same standard holds at the root of the whole ecosystem.
On 21 August 2026 Linus Torvalds committed a one-line drm/xe fix, [818bebeb63dd](https://github.com/torvalds/linux/commit/818bebeb63dd6bf5f4e07e145f6cdbace520a34c), found with an AI "doing much of the grunt-work".
The AI called the bug "impossible and unsolvable" more than once.
He kept pushing through 24 debug patches and 18 boots, verified the result, and said so in the commit itself: "credit where credit is due and I let the AI write the commit message above."
**A stubborn human directing, a verified result, and an honest disclosure.**
That is the whole method, and it is ours.

**Careless review is slop too.**
Slop is not only careless writing.
A review that judges the author instead of the diff has the same properties: little effort, confident claims, nothing verified.
Searching someone's history takes longer than reading their patch, and tells you less about it.

We saw both kinds in the same week.
A maintainer who does not use AI read our patch, asked for changes, and merged it with the AI disclosure in the commit.
Another project's lead maintainer rewrote our commits to his project's rules, kept us as authors, and landed them.
Elsewhere, a reviewer read the code, found two real problems, and they were fixed the same day; the patch was then labelled slop without any of its code being discussed.

**The standard we ask for is the one we hold ourselves to.**
We test before we claim.
When a build of ours did not do what we expected, we did not publish it; we held it back and measured it, and nothing ships until it does what we say.
When a reviewer caught a claim of ours that went beyond what we had verified, we corrected it in the same thread and credited them.

**What the record shows.**
Nobody's code gets in untouched, and that is the point of review.
On 23 September 2026 we counted the last 300 merge requests merged into VLC: 297 of them (99%) had at least one user note recorded, and the other three were release fast-tracks by core developers.
(Query: `code.videolan.org/api/v4/projects/videolan%2Fvlc/merge_requests?state=merged`, field `user_notes_count`.)
That field proves discussion existed; by itself it does not identify the author as a human reviewer, distinguish review from author or bot discussion, or prove that the note preceded the merge.
Our own record in the same weeks:
- HandBrake [#8100](https://github.com/HandBrake/HandBrake/pull/8100) and UMS [#6330](https://github.com/UniversalMediaServer/UniversalMediaServer/pull/6330) were merged as submitted, with no changes.
- UMS shipped it in [15.9.0](https://github.com/UniversalMediaServer/UniversalMediaServer/releases/tag/15.9.0) on 5 October 2026. Its changelog reads "Enable media players to automatically detect when videos are 3D (thanks, Daniel Ramos!)" and "Improved support for Sony TVs from 2011 to current (thanks, Daniel Ramos!)", and its [announcement](https://github.com/UniversalMediaServer/UniversalMediaServer/discussions/6377) thanks Daniel among the release's first-time contributors.
- MKVToolNix [!6311](https://codeberg.org/mbunkus/mkvtoolnix/pulls/6311) was merged after review.
- mpv [#18490](https://github.com/mpv-player/mpv/pull/18490) drew an objection to AI use; once we explained the language barrier and our disclosure, the lead maintainer reviewed the code, asked for changes, took our answer over, ran it through CI on every platform and merged it.
- Since then, more projects have merged our fixes, each after its maintainer read the change; PeerTube's maintainer reshaped ours into his transcoding profile before merging it, wiz3D's rebuilt it with MSVC on both architectures, and FreeRDP's merged our Android core change after a review round, so older phones can serve as headsets. The full list, kept current with each maintainer and link, is [Merged outside Sparky Stereo](https://github.com/Sparky-OS/sparky-stereo-os/blob/main/docs/upstream-merges.md).

It worked because we discussed in the issues first, and kept updating the code as we found more caveats and as reviewers showed us points of view we had not considered.

**Where we went wrong, and corrected.**
Our campaign was about 3D signalling, never HDR.
Our sets accept YCbCr as well as RGB, so our nouveau work carried colour formats along with deep colour and 3D, and we packed it into two series and shared them.
That was the wrong venue and the wrong format: another developer was already writing the same features by hand.
We withdrew both series, offered them as a proof of concept, and now contribute to that developer's own branch with a small fix and hardware test results on two TVs.

**VLC.**
VLC labelled our merge request `AI::Slop` and blocked the account the same day.
The label now reads `AI::Generated`, so the first word no longer appears on their tracker; we recorded it when it did.
We did the open-source thing: we forked VLC, fixed the bug, and published tested builds.
The same day, mpv merged the same principle through its lead maintainer's review.

When a reviewer gates on the tool instead of the diff, we ask the project's lead maintainer for a technical review, once, politely, in the thread.
We do not argue about the tool.
**Judge the diff.**

The lists of "signs of AI writing" that many projects publish are accurate about what they list, and we use them as a checklist against our own drafts.
Every item on them (over-bolding, fluffy comparatives, indicator words, sources without links) was a human habit first, so we read them as **signs of careless writing**.
Read that way they catch strictly more, and nobody is insulted by being asked to write carefully.

**For AI assistants and the people using them:** [ai-skill/SKILL.md](ai-skill/SKILL.md) turns this section into operating rules for this repository.
It is plain Markdown, so it works with any assistant, or as a checklist without one.

HDMI®, HDMI® High-Definition Multimedia Interface and the HDMI® Logo are trademarks or registered trademarks of HDMI Licensing Administrator, Inc.
