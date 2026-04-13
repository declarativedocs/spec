# DDF — Declarative Document Format

An open standard for describing documents in YAML. Designed as a token-efficient compilation target for LLMs.

**Created by [WeAreBrain](https://wearebrain.com)**

## What is DDF?

DDF is a set of YAML schemas that declaratively describe documents — presentations, reports, PDFs. A compiler reads the YAML and produces the real file (.pptx, .docx, .pdf).

The insight: LLMs shouldn't write imperative library code to produce documents. They should write declarative specs that a compiler turns into documents. This is 3-5x more token-efficient and significantly more reliable.

## Schemas

| Schema | Status | Target format | Spec |
|--------|--------|---------------|------|
| **Presentation** | v0.1.0 | .pptx | [presentation.v0.1.0.yaml](presentation.v0.1.0.yaml) |
| **Document** | Planned | .docx | — |
| **PDF** | Planned | .pdf | — |

## Key concepts

**Theme variables** — Define colors and fonts once, reference with `$name` everywhere.

**Masters** — Reusable slide/page templates with `{{placeholder}}` data binding.

**Auto-layout** — `group` elements with `row` or `grid` layout calculate positions automatically.

**Sensible defaults** — Omit anything you don't need. The compiler fills in reasonable values.

## Example

```yaml
presentation:
  layout: "16x9"
  theme:
    colors:
      primary: "1E2761"
      accent: "F96167"
      white: "FFFFFF"
    fonts:
      heading: Georgia

  slides:
    - background: $primary
      elements:
        - type: text
          x: 1  y: 2  w: 8  h: 1.5
          text: "Hello DDF"
          font: $heading
          size: 44
          color: $white
          bold: true
          align: center
```

## Compilers

| Language | Repository | Formats |
|----------|-----------|---------|
| Python | [declarativedocs/compiler-py](https://github.com/declarativedocs/compiler-py) | .pptx |

## Validation

The JSON Schema ([presentation.v0.1.0.schema.json](presentation.v0.1.0.schema.json)) can be used to validate DDF files before compilation:

```bash
pip install jsonschema pyyaml
python -c "
import yaml, json, jsonschema
spec = yaml.safe_load(open('my-deck.yaml'))
schema = json.load(open('presentation.v0.1.0.schema.json'))
jsonschema.validate(spec, schema)
print('Valid.')
"
```

## License

Apache 2.0
