# Burmese stroke order

How each Burmese (Myanmar script) letter and mark is written, stroke by stroke, as open data.

66 shapes: the 33 consonants, 28 vowel signs, medials and marks, 4 short letter forms and ဦ. Every stroke
was placed by hand for the [Myanmar Culture Club](https://uoftmcc.ca) "Learn Burmese" page, where they animate
the alphabet and write visitors' names.

Open `index.html` to watch them. It loads the data with `fetch`, so serve the folder over http
(for example `python3 -m http.server`, then open http://localhost:8000).

## Files

| File | What it is |
|---|---|
| `data/strokes.json` | Every shape: its outline and its strokes in writing order. One shape per line. |
| `data/clusters.json` | Which shapes to draw, and where, to write each of 671 Burmese syllables. |
| `index.html` | A small player for the data, with no dependencies. |

## Format

Coordinates are in font units: 100 units = 1 em, the baseline is at y = 0, and **y grows downward** (as in SVG).

```json
{
  "id": 214,
  "char": "က",
  "kind": "consonant",
  "latin": "ka",
  "name": "ka gyi",
  "needs_review": false,
  "outline": "M14.9 1.7Q10.6 -2.1 …Z",
  "bands": [ { "a": [[x, y], …], "b": [[x, y], …] }, … ]
}
```

- `id`: the glyph id in the Padauk Book font. Marks and short forms are keyed by their own glyph, so a
  character can appear more than once (for example ု short and long, or ြ in six widths).
- `kind`: `consonant`, `mark`, `short-form` (a letter's shorter shape before a sign below it) or `vowel-letter` (ဦ).
- `outline`: the shape as an SVG path.
- `bands`: the strokes, in writing order. A band is the ink between two borders of the shape. `a[k]` and `b[k]`
  are matching points on the two borders, from where the pen starts to where it lifts.
- `needs_review`: the strokes for this shape work but aren't perfect yet (often where the font merges two
  strokes). They may change.

### Drawing a stroke

To show a band written up to point k, fill the polygon `a[0..k]` followed by `b[k..0]`. The ink grows from the
start of the stroke with a straight front edge. Measure progress along the middle of the band, the midpoint of
`a[k]` and `b[k]`. The site plays strokes at 45 units a second with a 0.42 s pause between strokes. These
values are in `playback`.

### Writing a syllable

`clusters.json` maps a syllable to `[advance, glyphId, x, y, glyphId, x, y, …]`. Draw each listed shape moved
by (x, y), in the order given. Then move the pen right by `advance` for the next syllable.

## Licence

The outlines come from the [Padauk](https://software.sil.org/padauk/) font by SIL Global, which is licensed
under the SIL Open Font License 1.1. The stroke data follows the outlines exactly, so it is released under the
same licence. See `LICENSE`. This is data, not a font, and it is not affiliated with SIL.
