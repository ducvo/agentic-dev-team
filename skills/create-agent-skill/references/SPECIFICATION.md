# Agent Skills Specification Reference

Complete field reference for SKILL.md files per the [agentskills.io](https://agentskills.io/specification) open standard.

## Directory Structure

```
skill-name/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
├── assets/           # Optional: templates, resources
└── ...               # Any additional files
```

## Frontmatter Fields

### Required

| Field | Constraints |
|-------|------------|
| `name` | 1-64 chars. Lowercase `a-z`, numbers, hyphens only. No leading/trailing/consecutive hyphens. Must match parent directory name. |
| `description` | 1-1024 chars. Non-empty. Describes what the skill does and when to use it. |

### Optional

| Field | Constraints |
|-------|------------|
| `license` | License name or reference to a bundled license file. |
| `compatibility` | 1-500 chars. Environment requirements: intended product, system packages, network access, etc. Most skills don't need this. |
| `metadata` | Arbitrary key-value mapping (string keys → string values). For additional properties not in the spec. |
| `allowed-tools` | Space-delimited list of pre-approved tools. Experimental — support varies by client. |

### Name Validation

Valid:
```yaml
name: pdf-processing
name: data-analysis
name: code-review
```

Invalid:
```yaml
name: PDF-Processing    # uppercase not allowed
name: -pdf              # cannot start with hyphen
name: pdf--processing   # consecutive hyphens not allowed
name: pdf_processing    # underscores not allowed
```

### Description Guidelines

- Use imperative phrasing: "Use this skill when..." not "This skill does..."
- Focus on user intent, not implementation
- Include specific keywords that help agents identify relevant tasks
- List contexts where the skill applies, including indirect references
- Err on the side of being pushy about when to activate

Good:
```yaml
description: >
  Analyze CSV and tabular data files — compute summary statistics,
  add derived columns, generate charts, and clean messy data. Use this
  skill when the user has a CSV, TSV, or Excel file and wants to
  explore, transform, or visualize the data, even if they don't
  explicitly mention "CSV" or "analysis."
```

Poor:
```yaml
description: Helps with PDFs.
```

### Compatibility Examples

```yaml
compatibility: Designed for Claude Code (or similar products)
compatibility: Requires git, docker, jq, and access to the internet
compatibility: Requires Python 3.14+ and uv
```

### Allowed-Tools Example

```yaml
allowed-tools: Bash(git:*) Bash(jq:*) Read
```

## Body Content

The Markdown body after frontmatter has no format restrictions. Recommended sections:
- Step-by-step instructions
- Examples of inputs and outputs
- Common edge cases and gotchas

Keep under 500 lines. The agent loads the entire body on activation.

## Optional Directories

### scripts/

Executable code agents can run. Scripts should be self-contained, include helpful error messages, and handle edge cases. Supported languages depend on the agent client.

### references/

Additional documentation loaded on demand. Keep files focused — smaller files mean less context usage. Examples: `REFERENCE.md`, `FORMS.md`, domain-specific files.

### assets/

Static resources: templates, images, diagrams, data files, schemas.

## Progressive Disclosure

Three tiers of context loading:

1. **Metadata** (~100 tokens): `name` + `description` loaded at startup for all skills
2. **Instructions** (<5000 tokens recommended): full SKILL.md body loaded on activation
3. **Resources** (as needed): files in subdirectories loaded only when required

## File References

Use relative paths from the skill root:

```markdown
See [the reference guide](references/REFERENCE.md) for details.
Run the extraction script: scripts/extract.py
```

Keep references one level deep. Avoid deeply nested chains.

## Validation

```bash
skills-ref validate ./my-skill
```

Checks frontmatter validity and naming conventions.
