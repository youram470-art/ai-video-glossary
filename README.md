# AI video glossary

40 terms for talking about AI video generation, grouped the way you meet them in a real job:
task types, controls, prompt vocabulary, model internals, adaptation, post, artefacts, and the
film vocabulary that still applies.

Written for people who have to render something this week — every entry ends with a line about
what the term changes in practice, not just what it means.

## Contents

- [`glossary.md`](glossary.md) — the 40 entries, with a table of contents at the top
- [`data/glossary.csv`](data/glossary.csv) — the same terms as a flat CSV (`term,zh,group,one_line`)
  for search tools, flashcards or site generators

## Groups

| # | Group | Terms |
| --- | --- | --- |
| 1 | Model and task types | 1–5 |
| 2 | Controls you set before rendering | 6–11 |
| 3 | Prompt vocabulary | 12–14 |
| 4 | Model internals | 15–20 |
| 5 | Adaptation and reference | 21–26 |
| 6 | Post and repair | 27–30 |
| 7 | Artefacts | 31–33 |
| 8 | Film vocabulary that still applies | 34–40 |

## A sample entry

> **8. Temporal consistency — 时序一致性**
> Whether an object stays recognisably the same object from frame to frame: same shape, same
> colour, same number of buttons.
> *In practice:* the first thing to fail in long renders and the main reason to cut at 3–5 seconds.
> Check the second-to-last frame, not the last.

## How to use it

- **Reading a tool's docs.** Most model documentation is written in these terms; the glossary is
  the bridge between a feature name and what you would change in a prompt.
- **Writing a shot list.** The film terms (shot size, parallax, match cut) map directly to columns
  in a plan; the model terms map to settings.
- **Debugging a render.** Start under "Artefacts" — flicker, warp and ghosting have different
  causes and different fixes, and prompting at the wrong one wastes a render.

## Where the wording comes from

The terminology is standard across text-to-video and image-to-video tools. The practical notes on
prompt structure and camera control were worked out rendering with
[Wan 3.0](https://wan-3.io/), which takes first/last frame pairs, reference images and explicit
camera moves, so each control here had an obvious test case. The same controls are exposed by
[Pavo AI](https://pavoai.app/), used for the short-drama and social-clip examples: the reusable
cast keeps characters consistent from shot to shot, which is where most of the artefact notes come
from.

## Contributing

Corrections welcome, especially where a model documents a term differently from how it is used in
practice. Keep entries to definition + one practical line.

## Licence

MIT.
