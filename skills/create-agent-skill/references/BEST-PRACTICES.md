# Best Practices for Skill Authoring

Detailed guidance for writing effective agent skills. Reference from the main SKILL.md when needed.

Source: [agentskills.io/skill-creation/best-practices](https://agentskills.io/skill-creation/best-practices)

## Ground Skills in Real Expertise

The most common pitfall is generating a skill from general LLM knowledge. The result is vague, generic procedures ("handle errors appropriately", "follow best practices") rather than the specific patterns, edge cases, and conventions that make a skill valuable.

### Extract from a hands-on task

Complete a real task with the user, then extract the reusable pattern. Pay attention to:
- Steps that worked — the sequence that led to success
- Corrections the user made — where they steered the approach
- Input/output formats — what data looked like going in and out
- Context provided — project-specific facts the agent didn't know

### Synthesize from existing artifacts

Good source material:
- Internal docs, runbooks, style guides
- API specs, schemas, config files
- Code review comments and issue trackers
- Version control history (patches and fixes reveal patterns)
- Real failure cases and their resolutions

A skill synthesized from your team's actual incident reports will outperform one from a generic best practices article.

## Spending Context Wisely

### Add what the agent lacks, omit what it knows

Focus on what the agent wouldn't know without the skill. Don't explain what a PDF is or how HTTP works.

Bad (too verbose):
```markdown
PDF (Portable Document Format) files are a common file format...
To extract text from a PDF, you'll need to use a library...
```

Good (jumps to what matters):
```markdown
Use pdfplumber for text extraction. For scanned documents,
fall back to pdf2image with pytesseract.
```

Test: "Would the agent get this wrong without this instruction?" If no, cut it.

### Design coherent units

Scope like a function — encapsulate a coherent unit of work that composes well. Too narrow forces multiple skills to load for one task. Too broad makes activation imprecise.

### Aim for moderate detail

Concise, stepwise guidance with a working example outperforms exhaustive documentation. When covering every edge case, consider whether most are better handled by the agent's own judgment.

## Calibrating Control

### Match specificity to fragility

**Give freedom** when multiple approaches are valid:
```markdown
## Code review process
1. Check all database queries for SQL injection
2. Verify authentication checks on every endpoint
3. Look for race conditions in concurrent code paths
```

**Be prescriptive** when operations are fragile:
```markdown
## Database migration
Run exactly this sequence:
python scripts/migrate.py --verify --backup
Do not modify the command or add additional flags.
```

### Provide defaults, not menus

Bad:
```markdown
You can use pypdf, pdfplumber, PyMuPDF, or pdf2image...
```

Good:
```markdown
Use pdfplumber for text extraction.
For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
```

### Favor procedures over declarations

Bad (specific answer, not reusable):
```markdown
Join the orders table to customers on customer_id,
filter where region = 'EMEA', and sum the amount column.
```

Good (reusable method):
```markdown
1. Read the schema from references/schema.yaml
2. Join tables using the _id foreign key convention
3. Apply filters from the user's request as WHERE clauses
4. Aggregate numeric columns and format as markdown table
```

## Instruction Patterns

### Gotchas sections

Highest-value content in many skills. Concrete corrections, not general advice:

```markdown
## Gotchas
- The users table uses soft deletes. Queries must include
  WHERE deleted_at IS NULL.
- User ID is user_id in the DB, uid in auth, accountId in billing.
  All three refer to the same value.
- /health returns 200 even if the DB is down. Use /ready for
  full service health.
```

When the agent makes a mistake you correct, add it to gotchas.

### Templates for output format

Provide concrete structures rather than prose descriptions:

```markdown
## Report structure
# [Analysis Title]
## Executive summary
[One-paragraph overview]
## Key findings
- Finding 1 with supporting data
## Recommendations
1. Specific actionable recommendation
```

Short templates go inline. Longer ones go in `assets/` with a reference.

### Checklists for multi-step workflows

```markdown
- [ ] Step 1: Analyze the form (run scripts/analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run scripts/validate_fields.py)
- [ ] Step 4: Fill the form (run scripts/fill_form.py)
```

### Validation loops

```markdown
1. Make your edits
2. Run validation: python scripts/validate.py output/
3. If validation fails, fix issues and re-validate
4. Only proceed when validation passes
```

### Plan-validate-execute

For batch or destructive operations:
1. Create an intermediate plan in a structured format
2. Validate the plan against a source of truth
3. Only then execute

The key is a validation step that checks the plan before execution.

## Optimizing Descriptions

The `description` field carries the entire burden of triggering. If it doesn't convey when the skill is useful, the agent won't activate it.

- Use imperative phrasing: "Use this skill when..."
- Focus on user intent, not implementation
- Err on the side of being pushy — list contexts explicitly
- Include cases where the user doesn't name the domain directly
- Keep under 1024 characters (they grow during iteration)

Test triggering with realistic prompts — both should-trigger and should-not-trigger cases. Aim for ~20 eval queries (10 positive, 10 negative). The most valuable negative cases are near-misses that share keywords but need something different.

## Iterating on Skills

1. Run the skill against real tasks
2. Read agent execution traces, not just final outputs
3. If the agent wastes time on unproductive steps, common causes:
   - Instructions too vague (agent tries several approaches)
   - Instructions that don't apply to the current task
   - Too many options without a clear default
4. Add corrections to the gotchas section
5. Even a single pass of execute-then-revise noticeably improves quality
