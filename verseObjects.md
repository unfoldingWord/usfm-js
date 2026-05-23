
**Core (present on most objects)**

| Field | Description |
|-------|-------------|
| `type` | `"text"` \| `"word"` \| `"milestone"` \| paragraph/note type from `USFM_PROPERTIES` |
| `tag` | USFM marker name (e.g. `"w"`, `"zaln"`, `"p"`, `"f"`) |

**Content**

| Field | Description |
|-------|-------------|
| `text` | Display text — set on displayable markers (`isText = true`) |
| `content` | Raw content — set on non-displayable markers (footnotes, cross-refs, etc.) |
| `number` | Marker number (e.g. verse `"1"`, chapter `"3"`) — deleted if marker doesn't support numbers |

**Whitespace**

| Field | Description |
|-------|-------------|
| `nextChar` | The character immediately following the marker — `" "` or `"\n"` |

**Word-object fields (type: `"word"`, tag: `"w"`)**

- **example:** \w और|x-occurrence="1" x-occurrences="1"\w*

- Text of the word is between the `\w` and the `|` (`और`) in the example.
- Attributes are fields between the `|` and the `\w*` (occurrence, occurrences) in the example.


        {
            "content": "καὶ",
            "endTag": "zaln-e\\*",
            "lemma": "καί",
            "morph": "Gr,CC,,,,,,,,",
            "occurrence": "1",
            "occurrences": "1",
            "strong": "G25320",
            "tag": "zaln",
            "type": "milestone"
          },

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

**`\zaln` / `\k` milestone attribute fields (type: `"milestone"`)**

- **example:** `\zaln-s |x-content="καὶ" x-lemma="καί" x-morph="Gr,CC,,,,,,,," x-occurrence="1" x-occurrences="1" x-strong="G25320"\*`
- Attributes are fields between the `|` and the `\\*` in the example.

| Field | USFM attribute | Notes |
|-------|----------------|-------|
| `strong` | `strong`, `strongs`, `x-strong`      | Strongs number of original language word (e.g. "G25320")          |
| `lemma` | `x-lemma`                            | Lemma for original language word (e.g. "καί")                     |
| `morph` | `x-morph`                            | morphology for original language word (e.g. "Gr,CC,,,,,,,,")      |
| `occurrence` | `x-occurrence`                       | occurrence number for word in verse                               |
| `occurrences` | `x-occurrences`                      | total number of exact occurrences for word in verse               |
 `content` | `x-content` | Source-language word; `x-` stripped |
| `children` | — | Array of child verseObjects (span content) |
| `endTag` | — | Set on close: `"zaln-e\\*"` or `"k-e\\*"` |

Milestone / Span objects (other tags: `\qt-s`, `\xt`, etc.)

The `attrib` field applies to non-word, non-zaln/k span markers that go through
`startSpan()`. The `|`-delimited attribute string is stored raw rather than expanded.

| Field | Description |
|-------|-------------|
| `children` | Array of child verseObjects (displayable spans only) |
| `endTag` | End marker string — set when span closes |
| `attrib` | Raw `\|attr="val"...` string from `startSpan` attribute parsing |

Internal fields (set during parsing, deleted before final output in normal flow)

| Field | Lifecycle |
|-------|-----------|
| `usfm3Milestone` | Set to true on USFM3 milestone open; deleted by `decrementPhraseNesting` / `terminatePhrases` |
| `nesting` | Incremented/decremented for nested same-type spans; deleted when it reaches `0` |
| `endMarkerChar` | Captures space after milestone end marker; consumed by `endSpan`, not cleaned up — can leak into saved objects in edge cases |
| `open` | Set by `parseLine`; always deleted by `createUsfmObject` |
| `close` | Set by `parseLine`; always deleted by `createUsfmObject` |