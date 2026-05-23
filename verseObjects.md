# Verse Objects

This document describes the common fields used by verse objects produced while parsing USFM.

## Core fields

These fields can be present on most verse objects.

| Field | Description |
|-------|-------------|
| `type` | Object type. Common values include `"text"`, `"word"`, `"milestone"`, or a paragraph/note type from `USFM_PROPERTIES`. |
| `tag` | USFM marker name, such as `"w"`, `"zaln"`, `"p"`, or `"f"`. |

## Content fields

| Field | Description |
|-------|-------------|
| `text` | Display text. Set on displayable markers   |
| `content` | Raw content. Set on normally not ddisplayed markers such as footnotes, cross-references, and milestone source content. |
| `children` | Array of child verseObjects that are contained within this object |


## Whitespace fields

| Field | Description |
|-------|-------------|
| `nextChar` | Character immediately following the marker, usually `" "` or `"\n"`. |
| `endMarkerChar` | Character captured after a milestone end marker, usually whitespace. |

## Word objects

**Word-object fields (type: `"word"`, tag: `"w"`)**

- **USFM source example:** \w और|x-occurrence="1" x-occurrences="1"\w*

- Text of the word is between the `\w` and the `|` (`और`) in the example.
- Attributes are fields between the `|` and the `\w*` (occurrence, occurrences) in the example.

| Field | USFM attribute(s)                    | Notes                                                             |
|-------|--------------------------------------|-------------------------------------------------------------------|
| `strong` | `strong`, `strongs`, `x-strong`      | Strongs number of original language word (e.g. "G25320")          |
| `lemma` | `x-lemma`                            | Lemma for original language word (e.g. "καί")                     |
| `morph` | `x-morph`                            | morphology for original language word (e.g. "Gr,CC,,,,,,,,")      |
| `occurrence` | `x-occurrence`                       | occurrence number for word in verse                               |
| `occurrences` | `x-occurrences`                      | total number of exact occurrences for word in verse               |
| `tw` | `x-tw`                               | tWords key - originally used to identify related translation word |
| `content-source` | `content-source`, `x-content-source` | content source attribute (e.g. `bhp`)                             |
| `x-srcloc` | `x-srcloc`                           | Source of word (e.g. `"gnt5:51.1.2.1"`)                           |
| *(any other attr)* | `<key>`                            | any other added attrib                                            |

## Alignment objects (`zaln`)

**`\zaln` / `\k` milestone attribute fields (type: `"milestone"`)**

- **example:** `\zaln-s |x-content="καὶ" x-lemma="καί" x-morph="Gr,CC,,,,,,,," x-occurrence="1" x-occurrences="1" x-strong="G25320"\*`
- Attributes are fields between the `|` and the `\\*` in the example (x-contents, x-lemma,...).

| Field        | USFM attribute | Notes |
|--------------|----------------|-------|
| `strong`     | `strong`, `strongs`, `x-strong`      | Strongs number of original language word (e.g. "G25320")          |
| `lemma`      | `x-lemma`                            | Lemma for original language word (e.g. "καί")                     |
| `morph`      | `x-morph`                            | morphology for original language word (e.g. "Gr,CC,,,,,,,,")      |
| `occurrence` | `x-occurrence`                       | occurrence number for word in verse                               |
| `occurrences` | `x-occurrences`                      | total number of exact occurrences for word in verse               |
| `content`    | `x-content` | Source-language word; `x-` stripped |

