# Form Notation Governance

How the Form Notation standard is stewarded, changed, and versioned.

## 1. Model

Form Notation is stewarded under a **Benevolent Dictator for Life (BDFL)** model. One person holds final authority over what is and is not part of the language. This keeps the standard coherent and fast-moving while it matures.

## 2. Roles

- **BDFL: Brent Brinkley.** Final authority over the normative content of the standard (vocabulary, durations, the halo, the event model, conformance). The BDFL decides what becomes part of Form.
- **Owner: Polyhedra LLC.** Holds the trademark, the reference implementation, the reference glyph artwork, and the reserved commercial fields.

These roles are deliberately separate: the **person** holds authority over the language; the **entity** holds the property. Authority is not transferred by transferring assets, and assets are not transferred by transferring authority.

## 3. How Changes Are Made

1. Anyone may **suggest** a change by opening an issue. Suggestions are ideas and feedback only; the specification text is authored solely by the BDFL.
2. The BDFL reviews suggestions and decides, at sole discretion, whether to act on any of them. There is no obligation to respond to, adopt, implement, or explain a decision about any suggestion.
3. When the BDFL adopts a change, the BDFL writes the edit. Accepted **normative** changes result in a new version (see §4); **non-normative** material (reference glyph artwork, palette, recommendations, examples, wording) may be updated independently.

This process is intentionally lightweight for the standard's early life, and it will likely be revised in the future, once the standard and its ecosystem settle, to allow for more formal outside contribution (for example, accepted pull requests under a contributor agreement). See [CONTRIBUTING.md](CONTRIBUTING.md).

## 4. Versioning

The specification carries a version number and a date in its header.

- A change to the **normative** set (shapes, colors, the duration set, the halo, the event model, or conformance) increments the version.
- Non-normative updates may be released without a normative version change.
- Each release is recorded in [CHANGELOG.md](CHANGELOG.md). Published releases should be tagged so a given version is permanently identifiable.

## 5. What Is Stable

The normative vocabulary and rules are intended to be stable. Material explicitly marked *planned* or *provisional* in the specification (for example, future articulation handling) is not yet settled and may change before it becomes normative. The reference glyph artwork is versioned separately and may be refined without changing the standard.

## 6. Future Stewardship

The BDFL may transfer stewardship of the **standard** to a neutral foundation or similar body if and when the ecosystem warrants it. In that event the trademark and the reserved commercial fields would be handled separately from the open specification, so that opening the standard's governance does not by itself transfer the brand or the reserved fields.

## 7. Succession

If the BDFL is unable to continue, stewardship of the standard passes to a successor designated by the BDFL or, failing that, determined by Polyhedra LLC. The BDFL role (authority over the language) remains distinct from ownership of the assets (held by Polyhedra LLC).

© 2009–2026 Brent Brinkley / Polyhedra LLC.
