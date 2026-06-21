# Form Notation Specification

**Version:** 1.0
**Status:** Draft (for review)
**Created by:** Brent Brinkley
**Owner:** Polyhedra LLC
**Maintainer (BDFL):** Brent Brinkley
**Published:** 2026-06-19

> **Form Notation™** is a trademark of Polyhedra LLC.
> The text of this specification is licensed under **Creative Commons Attribution 4.0 International (CC BY 4.0)**.
> You may implement, adapt, teach, and build on this notation, commercially or otherwise, provided you give attribution as specified in §18. The **name** "Form Notation" and the **reference glyph artwork** are not granted by this license (see §18).

---

## 1. About This Document

This is the implementer's reference for **Form Notation**, a visual notation system that encodes pitch, octave, and rhythm with geometric shapes, colors, and a circular duration indicator. It is written for anyone who wants to **read, write, render, teach, or build tools** for Form.

It deliberately does **not** describe how any particular product parses or processes its input, stores files, or implements its UI. It describes only what an implementation must honor to be **Form-compliant**.

### 1.1 Normative Language

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are used in the sense of RFC 2119. Sections or tables marked *normative* define conformance. Sections marked *non-normative* (including all reference artwork) are guidance and may evolve without changing the standard.

### 1.2 What Is and Isn't Fixed

- The **shape vocabulary** (which shape means which pitch class) and the **color vocabulary** (which color means which octave) are **normative and stable**.
- The **duration set** (the defined note values and their tick counts) and the **duration halo** that renders them are **normative and exact** (see §7 and §8). The note values are a closed set used exactly as shown, and the halo geometry must be reproduced exactly; unlike the pitch-class shapes, the halo is not redrawable.
- The **reference glyph artwork** (the specific drawn appearance of each shape) is **non-normative and versioned** (see §6). Implementations MAY render their own glyphs provided each remains clearly recognizable as the named shape.

---

## 2. Overview

Form represents a musical event with three independent visual channels:

| Channel | Encodes | Vocabulary |
|---------|---------|------------|
| **Shape** | Pitch class (which of the 12 chromatic notes) | 12 shapes |
| **Color** | Octave (register) | 11 colors |
| **Duration halo** | Rhythmic length | proportion of a circle |

Because the three channels are orthogonal, any pitch in the standard MIDI range maps to exactly one (shape, color) pair, and any pair maps back to exactly one pitch. Form is a complete alternative to staff notation for equal-tempered music, and it round-trips with both MIDI and formal composition (traditional, staff-based notation), so a piece can move among all three representations, Form, MIDI, and formal composition, in either direction.

### 2.1 Design Principles

- **Regularity over convention.** Every shape, color, and duration follows one rule with no exceptions, which is why the system is learnable in minutes.
- **Orthogonal channels.** Pitch class, octave, and duration never share a visual cue.
- **Round-trips with MIDI and formal composition.** Form round-trips with MIDI without loss of pitch; this is the baseline a conforming implementation must support (§15). It also interchanges with formal composition (traditional, staff-based notation), so a piece can move among all three representations in either direction, though supporting the formal-composition side is optional.
- **Rhythm as proportion.** Duration is read as a fraction of a circle, not as flags and beams.

---

## 3. Pitch Class: Shapes *(normative)*

There are exactly **12 shapes**, one per chromatic pitch class. Pitch class is independent of octave.

| Pitch Class | Note | Shape    |
|:-----------:|:----:|----------|
| 0  | C   | Circle   |
| 1  | C♯  | Moon     |
| 2  | D   | Star     |
| 3  | D♯  | Plus     |
| 4  | E   | Triangle |
| 5  | F   | Square   |
| 6  | F♯  | Key      |
| 7  | G   | Leaf     |
| 8  | G♯  | Crown    |
| 9  | A   | Cube     |
| 10 | A♯  | Minus    |
| 11 | B   | Heart    |

Enharmonic equivalents share a shape: C♯ and D♭ are both **Moon**. Form encodes pitch *class*, not spelling.

---

## 4. Octave: Colors *(normative)*

There are exactly **11 colors**, one per octave from −1 through 9. Octave numbering follows the scientific/MIDI convention in which **middle C is C4**.

| Octave | Color  | MIDI Range | Notes |
|:------:|--------|:----------:|-------|
| −1 | Black  | 0–11    | Lowest MIDI octave (deepest pitch) |
| 0  | Gold   | 12–23   |  |
| 1  | Red    | 24–35   |  |
| 2  | Orange | 36–47   |  |
| 3  | Yellow | 48–59   |  |
| 4  | Green  | 60–71   | Contains middle C (C4 = 60) |
| 5  | Cyan   | 72–83   |  |
| 6  | Blue   | 84–95   |  |
| 7  | Purple | 96–107  |  |
| 8  | Pink   | 108–119 |  |
| 9  | Silver | 120–127 | Highest MIDI octave (caps at G9 = 127) |

Color names are normative identities. Their exact rendered hues are **non-normative** (see §6.1): an implementation MUST keep the eleven colors mutually distinguishable but MAY tune the palette for its display.

---

## 5. Pitch ↔ MIDI Mapping *(normative)*

Let `pitch_class ∈ 0…11` and `octave ∈ −1…9`.

**MIDI → Form**
```
pitch_class = midi_note_number mod 12
octave      = (midi_note_number div 12) − 1
shape       = SHAPES[pitch_class]
color       = COLORS[octave + 1]
```

**Form → MIDI**
```
midi_note_number = (octave + 1) × 12 + pitch_class
```

where `SHAPES` is the §3 table indexed by pitch class and `COLORS` is the §4 table indexed by `octave + 1`.

### 5.1 Range and Clamping

Valid MIDI note numbers are **0–127**. A conforming converter:

- MUST map every input in 0–127 to a valid (shape, color) pair and back without loss.

---

## 6. Reference Glyph Artwork *(non-normative, versioned)*

The canonical visual rendering of the twelve shapes is published separately as the **Form Reference Glyph Set**, distributed and dated independently of this specification so the artwork can be refined without revising the standard.

- Current reference set: **Reference Glyphs v0.9** (provisional; subject to refinement before a locked 1.0 release).
- The reference artwork is © 2009–2026 Brent Brinkley / Polyhedra LLC. It is **not** part of this document's CC BY 4.0 grant, but it is published in the repository and may be used under a separate **glyph asset license** (free for Form-compliant, non-reserved use, with copyright retained). See the Brand & Trademark policy.
- An implementation is conformant as long as each rendered shape is unambiguously recognizable as the named shape in §3, whether or not it uses the official artwork.

### 6.1 Color Rendering *(non-normative)*

A reference palette is published with the glyph set. Implementations MAY substitute their own hues provided all eleven octave colors remain mutually distinguishable on the target display.

---

## 7. Duration: Ticks *(normative)*

Duration is measured in **ticks** at a resolution of **960 ticks per quarter note** (standard MIDI PPQ). A whole note is therefore **3840 ticks**.

The durations defined in this section (§7.1 through §7.3, together with the resolution floor in §7.6) are the **complete and normative set** of legal note values. A conforming implementation MUST adhere to these durations, their tick values, and their assigned representations **exactly as shown**. The set is closed: there is no divergence from it, and no implementation may add, remove, redefine, or approximate these values.

### 7.1 Standard Durations

| Duration | Ticks | Form Name |
|----------|:-----:|-----------|
| Whole | 3840 | Whole |
| Half | 1920 | Half |
| Quarter | 960 | Quarter |
| Eighth | 480 | 8th |
| Sixteenth | 240 | 16th |
| Thirty-second | 120 | 32nd |
| Sixty-fourth | 60 | 64th |

### 7.2 Dotted Durations (base × 1.5)

| Duration | Ticks | Form Name |
|----------|:-----:|-----------|
| Dotted Half | 2880 | 3/4 |
| Dotted Quarter | 1440 | 3/8 |
| Dotted Eighth | 720 | 3/16 |
| Dotted Sixteenth | 360 | 3/32 |
| Dotted Thirty-second | 180 | 3/64 |
| Dotted Sixty-fourth ‡ | 90 | 3/128 |

### 7.3 Triplet Durations (base × 2/3)

| Duration                | Ticks | Form Name |
| ----------------------- | :---: | --------- |
| Half Triplet            | 1280  | Third     |
| Quarter Triplet         |  640  | 6th       |
| Eighth Triplet          |  320  | 12th      |
| Sixteenth Triplet       |  160  | 16th t    |
| Thirty-second Triplet ‡ |  80   | 32nd t    |

> **‡ Shared glyph.** The dotted sixty-fourth (90 ticks) and the thirty-second triplet (80 ticks) are indistinguishable at this resolution, so Form treats them as one and the same value: both render with the **thirty-second-triplet halo**.

### 7.4 Alignment to Note Values (No Arbitrary Ticks)

The named durations in §7.1–§7.3 are the **complete, closed set of legal note values**, not merely conveniences. Form has **no arbitrary tick durations**, exactly as standard notation has no note value "between" the ones it defines. Every event's duration MUST be one of the defined note values.

Longer or tied durations are not off-grid values; they are expressed as **continuations composed of defined values** (§10.3). For example, a sustain spanning a whole note plus a half note is a whole-note attack followed by a half-note continuation.

Any incoming duration that does not align to a defined value (for example, an unquantized performance captured from a live source) MUST be **forced into alignment** (quantized onto the defined set) before it becomes a Form event. The stored and rendered duration is always a defined note value.

> **Why this constraint exists.** Restricting durations to a closed set of note values keeps the notation's structure coherent and legible: every event reads as a recognizable value, the grid stays consistent, and the system remains as readable as standard notation rather than degrading into off-grid clutter.

> **Tooling (non-normative).** Performing this alignment (quantizing off-grid input onto the defined note-value set, and the related conversion of source material into Form events) is the responsibility of each implementation. Polyhedra LLC develops and makes available proprietary software tools, including algorithmic quantization and conversion utilities, that perform this work. Such tools are commercial products offered separately and subject to a fee and to their own license terms. They do **not** form part of this specification, and **no** license or right to use them is granted by the Creative Commons license covering this document (see §18); that license extends only to the text of this specification. Nothing herein obligates any party to use Polyhedra LLC's tools; implementers remain free to develop their own conforming alignment and conversion logic.

### 7.5 Tick-to-Time

For tempo in BPM:
```
seconds = ticks / ((BPM / 60) × 960)
```

### 7.6 Resolution Floor

The 64th note is the floor of distinguishable resolution, with two consequences:

- **Below the 64th note** (any value shorter than a 64th, under 60 ticks, such as the 96th at 40 ticks or the 128th at 30 ticks): these cannot be told apart at this scale, so they are not assigned individual values. They all render as a single **bare shape glyph with no duration halo** (§10.5), signifying an ultra-short note of indeterminate exact length.
- **Near-identical neighbors merge.** The thirty-second triplet (80 ticks) and the dotted sixty-fourth (90 ticks) are too close in resolution to distinguish. Form treats them as one and the same value, and both render with the **thirty-second-triplet halo**.

---

## 8. Duration Halo: Visual Encoding *(normative)*

Rhythm is shown as a **circular duration indicator** ("halo") displayed beside the shape. The indicator works in **two tiers**: a standard (large) circle for longer values and a smaller circle for finer subdivisions, so that small durations stay legible instead of collapsing into a thin sliver.

### 8.1 The Standard Circle: Whole through Eighth

For values from a **whole note down to an eighth note**, the standard (large) circle is used. Fill scales linearly with duration, a whole note filling the entire circle:

```
standard_circle_span_degrees = (ticks / 3840) × 360      // 3840 ticks = one whole note
```

| Duration | Halo Span (standard circle) |
|----------|:---------:|
| Whole | 360° |
| Half | 180° |
| Quarter | 90° |
| Eighth | 45° |

The standard circle is drawn as a **halo with an open center (a "hole")**. That open center is intentional: it is the seat of the finer-resolution **smaller circle**, which takes over once a value drops below an eighth note.

### 8.2 The Smaller Circle: Sixteenth and Finer

At the **sixteenth note**, the indicator switches to the smaller circle. This is itself the signal that the value is a sixteenth or finer. The smaller circle **re-bases** the scale and **subdivides** to show the finer units: a sixteenth fills the entire smaller circle, and each finer value is a proportion of it:

```
smaller_circle_span_degrees = (ticks / 240) × 360        // 240 ticks = one sixteenth
```

| Duration | Halo Span (smaller circle) |
|----------|:---------:|
| Sixteenth | 360° |
| Thirty-second | 180° |
| Sixty-fourth | 90° |

### 8.3 Conformance and Geometry

The duration halo is a defining, normative characteristic of Form, not a stylistic choice. To be **Form-compliant**, an implementation MUST reproduce the halo geometry exactly as shown, with no divergence. This is normative in full: the **two-tier structure**, the tier transition at the sixteenth, **linear fill within each tier**, the relative sizes of the two circles, the open-center hole and its proportions, segment insets and spacing, corner rounding, and the way **dotted** and **triplet** values are subdivided into composite arcs.

The exact geometry is fixed by the canonical Form reference rendering published with this specification (see the Form Notation documentation and reference set). This is the one place Form locks rendering. It contrasts deliberately with §6: an implementation MAY redraw the pitch-class shapes so long as they remain recognizable, but it MUST NOT diverge from the duration halo. Pitch-shape rendering is flexible; **duration halo rendering is not**.

The halo is drawn only down to the 64th note. Durations shorter than a 64th carry no halo and render as a bare shape (§7.6, §10.5).

See the accompanying Form Notation documentation for worked visual examples of both circles and their subdivisions.

---

## 9. The Form Event Model *(normative)*

A **Form Event** is one musical moment: a note attack, a chord member, a sustained continuation, or a rest. It carries:

| Field | Type | Meaning |
|-------|------|---------|
| `shape` | Shape \| null | Pitch class. `null` only for rests. |
| `color` | Color \| null | Octave. `null` only for rests. |
| `duration` | ticks (integer ≥ 0) | Length of the event. |
| `position` | ticks (integer ≥ 0) | Start, measured from the beginning of the piece. |
| `part` | integer ≥ 0 | Voice / hand (see §13). |
| `isChord` | boolean | Member of a chord group (see §11). |
| `isRest` | boolean | Silence (see §12). |
| `isExtension` | boolean | Sustained continuation of a prior attack (see §10.3). |
| `velocity` | integer 0–127 | Dynamics (see §14). |
| `hasArticulation` | boolean | Reserved for future articulation/expression use. |

Implementations MAY carry additional fields (e.g. display-grouping or source-tracking metadata) but MUST preserve the fields above through any round-trip.

---

## 10. Notes and Their States *(normative)*

A single sounding note is a Form Event with a `shape`, a `color`, a `duration`, and `isRest = isExtension = false`. Four display states share the same data model and MUST be visually distinguishable:

| State | Shape drawn? | Halo | Identifies |
|-------|:------------:|-----------|-----------|
| **Attack** | Yes | Octave color | A new note onset |
| **Extension** (§10.3) | No | Octave color (same as attack) | A note still sounding, not re-struck |
| **Rest** (§12) | No | Neutral / gray | Silence |
| **Sub-64th** (§10.5) | Yes | None | An ultra-short note below readable resolution |

### 10.1 Attack

The normal case: the shape is drawn in (or against) its octave color, with the duration halo beside it.

### 10.2 Chord Members

A chord member is an ordinary note glyph with its own shape, color, and halo. Chords are formed by stacking such notes at the same position (§11).

### 10.3 Extension (Sustain / Tie)

When a note sustains across a boundary (beat, measure, or display cell) it MAY be split into an attack event followed by one or more **extension** events. Each extension:

- MUST repeat the original `shape` and `color` (so register and identity stay visually continuous), and
- MUST set `isExtension = true`, and
- MUST be rendered **without the shape glyph** (only the duration halo, in the note's color), signaling "still sounding, not re-attacked."

### 10.4 Legato *(planned, not yet finalized)*

A **legato** relationship exists when one note's end point coincides exactly with the next note's start point within the same part. That is, the first note's `position + duration` equals the next note's `position`, with no gap and no overlap. Such notes are played seamlessly into one another.

Form will give legato notes a **distinct visual appearance** to mark the seamless connection. The exact treatment is **in the pipeline and not yet specified**; this subsection records that legato is a recognized note state a complete implementation will eventually need to **detect and render**. Until the visual treatment is finalized, implementations MAY render such notes as ordinary attacks.

Legato is distinct from an **extension** (§10.3): an extension is the continuation of a *single* sustained note (no new attack), whereas legato is a connection between *two separate* notes, each with its own attack.

### 10.5 Sub-64th Notes (bare shape)

Durations shorter than a 64th note fall below the resolution at which the duration halo conveys readable information (§7.6). Such a note is rendered as its **shape glyph alone, with no duration halo**. A single bare-shape style covers every sub-64th duration, since they cannot be told apart at this scale. This is the system's grace-note / ornament form: it states that a very short note occurs without asserting an exact length.

It is the inverse of a rest and an extension: a rest is a neutral halo with no shape, an extension is a colored halo with no shape, and a sub-64th note is a shape with no halo.

---

## 11. Chords *(normative)*

### 11.1 Grouping

A chord is two or more Form Events that share **all** of:

1. `isChord = true`
2. the same `position`
3. the same `part`

Each chord member remains an independent event with its own shape, color, and duration; grouping is for reading, not a merge of data. Events are processed in `position` order: contiguous `isChord` events at one position form one group; a non-chord event or a change of position starts a new group.

### 11.2 Display

A chord is a set of notes stacked vertically at the same position, one note glyph above another, each with its own shape, color, and halo. The vertical stack is what reads as a chord.

---

## 12. Rests *(normative)*

A rest is a Form Event with:

- `shape = null`, `color = null`
- `isRest = true`
- `velocity = 0`
- a `duration` and `position` like any event

A rest MUST render its **duration halo in a neutral color (gray) with no shape**, so silence is distinguishable from both attacks (shaped, colored) and extensions (shapeless, colored). The halo still communicates the length of the silence.

A rest denotes silence **within its own voice or lane**. It MUST NOT obscure, overwrite, or replace a note that sounds at the same `position` in another lane or part: where a rest would occupy a display slot already taken by a sounding note, the implementation MUST suppress the rest so the note remains visible. Silence in one voice never hides sound in another.

---

## 13. Parts, Voices, and Lanes *(normative)*

### 13.1 Parts (optional)

`part` is a non-negative integer that separates independent voices or hands. **Parts are optional.** Much music is single-part, in which case every event is `part` 0. Parts exist mainly for translating multi-staff sheet music, such as a piano **grand staff**, where the right and left hands are conventionally separated.

| Part | Typical role |
|:----:|--------------|
| 0 | Primary (right hand / melody) |
| 1 | Secondary (left hand / bass) |
| 2+ | Additional voices |

Notes in different parts tend to stack independently, but nothing requires them to; `part` is an organizational convenience, not a structural requirement. Chord grouping (§11) and timeline position are evaluated **per part**; events in different parts never merge into one chord, even at the same position.

### 13.2 Simultaneity vs. Lanes

Whether notes share a vertical slot is determined by **when they are triggered**, not by how long they last:

- **Same start → one stack.** Notes that begin at the same `position` are simultaneous and read as a single stacked group (a chord, §11) **even when their durations differ**. The staff draws no distinction between equal- and unequal-length notes that are triggered together, because they all attack at the same moment; a chord may freely contain notes that sustain longer than others.
- **Overlap without a shared start → separate lanes.** A vertical distinction is only required when a note is still sounding and *another note is struck beneath it during that sustain*; that is, the notes overlap but do **not** share a start point. These cannot occupy the same slot, so they are placed in **separate vertical lanes** (voices) so that both the held note and the new attack remain legible.

A **lane** is therefore a display-layout row that arises from overlapping, non-coincident durations (the held voice and the moving voice each get their own row); it is distinct from `part`. Implementations MAY record the assigned lane as a per-event display field.

### 13.3 Collapsing Horizontal Space

A Form-compliant layout MUST eliminate unnecessary horizontal space so the grid stays as compact as the content allows.

- **Equal-width columns.** Each grid item (a note, a chord, a rest, or an empty placeholder) occupies one column of the same width. Horizontal position conveys order and beat alignment, not proportional duration; duration is carried by the halo (§8).
- **The shortest duration sets the resolution.** Within each section (typically a measure), find the shortest note duration present across all parts. That smallest duration sets the grid resolution (the beat unit) for the whole section, and it applies to every part: whichever part holds the shortest note sets the grid for all of them.
- **Columns appear only where events occur.** A column is created only at a position that actually contains an event (a note, a chord, or a rest). Positions with no event get no column, so sparse passages do not open up wide gaps.
- **Shared columns align the parts.** All parts share a column at a given position, so simultaneous events line up vertically across hands and voices (§13.1).

The result: a measure of two whole notes stays compact, while a measure of running sixteenths expands only as much as its content requires.

---

## 14. Velocity and Dynamics *(normative range, non-normative mapping)*

`velocity` follows the MIDI range 0–127, where 64 is the default when unspecified. A suggested mapping to dynamic markings:

| Velocity | Dynamic |
|:--------:|:-------:|
| 0–31 | pp |
| 32–47 | p |
| 48–63 | mp |
| 64–79 | mf |
| 80–95 | f |
| 96–111 | ff |
| 112–127 | fff |

The numeric range is normative; the marking boundaries are a recommendation.

### 14.1 Expression Data Lane *(planned, not yet finalized)*

Velocity changes, and other articulations and expression that the core shape / color / duration system does not encode, will be surfaced in Form through a dedicated **data lane**: an optional, supplementary row that carries this per-event information alongside the notes.

Because such detail is reference information you do not always need to see once you know it is there, the data lane is **toggleable**: it can be shown or hidden without affecting the notes themselves.

The full set of articulations, how each is represented, and the exact behavior of the data lane have **not yet been formalized**. In the meantime the `velocity` and `hasArticulation` fields (§9) reserve space for this information in the event model.

---

## 15. Conformance

A conforming implementation of Form Notation **MUST**:

1. Map all 12 shapes to their §3 pitch classes.
2. Map all 11 colors to their §4 octaves.
3. Use 960 ticks per quarter note (whole note = 3840 ticks) as the tick resolution, and constrain every event's duration to a value from the normative set (§7), used exactly as shown with no divergence, quantizing any non-aligned input onto that set.
4. Convert losslessly between Form (shape, color) and MIDI note numbers across 0–127 (§5).
5. Render duration on the two-tier circular indicator (standard circle for whole-through-eighth values, smaller circle for sixteenth and finer), scaling fill linearly within each tier and reproducing the canonical halo geometry exactly, with no divergence (§8); render any duration below a 64th as a single bare shape with no halo (§7.6, §10.5).
6. Represent chords as multiple events sharing `position`, `part`, and `isChord = true` (§11).
7. Represent rests with `null` shape/color and `isRest = true` (§12).
8. Properly render rests: a neutral duration halo sized to the silence, with no shape, and suppressed wherever it would obscure a note sounding at the same position in another lane or part (§12).
9. Represent sustained notes as extensions (`isExtension = true`) that repeat shape/color but render the halo only (§10.3).
10. Keep attacks, extensions, and rests mutually distinguishable (§10).
11. Lay events on the grid so that simultaneous events stack vertically and unnecessary horizontal space is eliminated: within each section the shortest note duration sets the grid resolution, columns appear only where events occur, and all parts share columns for vertical alignment (§13).

An implementation that does all of the above **MAY** describe itself as *Form-compliant*. Use of the **name** "Form Notation" / "Form™" and of the **reference glyph artwork** in branding or product identity is reserved to Polyhedra LLC; such use requires a prior written license and is not permitted without one (see the Brand & Trademark policy accompanying this specification).

**Open use of the notation.** The Form notation itself (the shape, color, and duration system defined in this specification) is free for anyone to use for almost any purpose, commercial or not. This expressly includes: building software and applications around it; integrating Form into any DAW, plug-in, or other tool; designing original instruments; and producing your own educational materials, tutorials, courses, books, or videos. Describing a product as "works with Form notation" (a truthful, descriptive reference) is also permitted.

This open permission does **not** extend to two reserved uses: the formal curriculum of a school system, college, or university (a reserved education field), and video games that incorporate Form (a reserved entertainment use). Both require a prior written license from Polyhedra LLC and are governed by the Brand & Trademark policy (§18). Independent and professional courses, tutorials, and other public teaching remain open, as does non-game software of any kind.

Reserved to Polyhedra LLC are the **Form** and **FormKey** names and branding, the **reference glyph artwork**, Polyhedra LLC's own products and official offerings, and the reserved commercial fields described in the Brand & Trademark policy (§18). Outside those reserved items and fields, nothing here restricts your use of the Form notation in the open ways described above.

---

## 16. Reference Tables

### 16.1 Shapes

```
Circle   = C  (0)     Key      = F♯ (6)
Moon     = C♯ (1)     Leaf     = G  (7)
Star     = D  (2)     Crown    = G♯ (8)
Plus     = D♯ (3)     Cube     = A  (9)
Triangle = E  (4)     Minus    = A♯ (10)
Square   = F  (5)     Heart    = B  (11)
```

### 16.2 Colors

```
Black  = Oct −1     Cyan   = Oct 5
Gold   = Oct 0      Blue   = Oct 6
Red    = Oct 1      Purple = Oct 7
Orange = Oct 2      Pink   = Oct 8
Yellow = Oct 3      Silver = Oct 9
Green  = Oct 4  (middle C)
```

### 16.3 Worked Examples

| Note | MIDI | Pitch Class | Octave | Shape | Color |
|------|:----:|:-----------:|:------:|-------|-------|
| C4 (middle C) | 60 | 0 | 4 | Circle | Green |
| A4 | 69 | 9 | 4 | Cube | Green |
| C5 | 72 | 0 | 5 | Circle | Cyan |
| F♯3 | 54 | 6 | 3 | Key | Yellow |
| B♭2 | 46 | 10 | 2 | Minus | Orange |

**C major triad (C4–E4–G4) at position 0, part 0:**
```
{ shape: Circle,   color: Green, position: 0, part: 0, isChord: true, duration: 960 }
{ shape: Triangle, color: Green, position: 0, part: 0, isChord: true, duration: 960 }
{ shape: Leaf,     color: Green, position: 0, part: 0, isChord: true, duration: 960 }
```

**A whole note tied to a half note (G4, 5760 ticks total):**
```
{ shape: Leaf, color: Green, position: 0,    duration: 3840, isExtension: false }
{ shape: Leaf, color: Green, position: 3840, duration: 1920, isExtension: true  }
```

---

## 17. Versioning and Governance

This specification is versioned. Changes to the **normative** vocabulary (shapes, colors, duration model, event model, conformance) are made only through the project's governance process and result in a new version number. Non-normative material (reference glyph artwork, palettes, recommendations) may be updated independently.

Form Notation was created by **Brent Brinkley**. He stewards the standard as its **Benevolent Dictator for Life (BDFL)**, holding final authority over what does and does not become part of the language. **Polyhedra LLC** owns the trademark, the reference implementation, and the reserved commercial fields. The governance process and the brand/trademark policy are published alongside this document.

---

## 18. License, Trademark & Copyright

- **Specification text:** © 2009–2026 Brent Brinkley / Polyhedra LLC, licensed **CC BY 4.0**. You may share and adapt with attribution.
- **Required attribution (CC BY 4.0):** *"Form Notation, created by Brent Brinkley, © 2009–2026 Brent Brinkley / Polyhedra LLC. Used under CC BY 4.0."*
- **Author / Originator:** Brent Brinkley. **Maintainer:** Brent Brinkley (BDFL).
- **Name / brand:** "Form Notation" and "Form™" are trademarks of Polyhedra LLC. The CC BY license covers the *text*, not the *name*.
- **Reference glyph artwork & palette:** © 2009–2026 Brent Brinkley / Polyhedra LLC. Not included in the CC BY grant; available under a separate asset license for Form-compliant, non-reserved use (see the Brand & Trademark policy). All other rights reserved.

*End of specification. v1.0, 2026-06-19.*
