# Content Patterns for Skills

Patterns for organizing skill references and structuring skill content. Covers how to split a skill into references (when, and by what axis) and how to structure the content itself (workflows, feedback loops, output templates).

**When using this reference, state:** "I'm applying skill patterns: reference splitting, workflows, feedback loops, and output templates."

## Contents

- [Reference Splitting Patterns](#reference-splitting-patterns)
- [Workflows and Checklists](#workflows-and-checklists)
- [Feedback Loops](#feedback-loops)
- [Template Patterns](#template-patterns)

## Reference Splitting Patterns

Use these patterns when a skill is approaching the 500-line limit or has content that only applies in specific contexts.

**Pattern 1: Tracker/tool-specific details**
```
story-writing/
├── SKILL.md (core principles + navigation)
└── references/
    ├── jira-markup.md
    ├── github-markdown.md
    └── linear-formatting.md
```
Load condition: "Read jira-markup.md when working in Jira." Agent reads only the relevant file.

**Pattern 2: Domain/platform-specific patterns**
```
story-writing/
├── SKILL.md (universal principles + navigation)
└── references/
    ├── cloud-foundry-api.md
    ├── kubernetes.md
    └── aws.md
```
Load condition: "Read cloud-foundry-api.md when writing Cloud Foundry stories."

**Pattern 3: Framework/language variants**
```
testing/
├── SKILL.md (testing principles + navigation)
└── references/
    ├── rspec.md
    ├── jest.md
    └── pytest.md
```
Load condition: "Read rspec.md when working in Ruby."

**Pattern 4: Conditional details**
```markdown
## Advanced Features
For advanced features, see:
- **Feature X**: Read [references/feature-x.md](references/feature-x.md) when doing X
- **Feature Y**: Read [references/feature-y.md](references/feature-y.md) when doing Y
```
Agent loads the reference only when the specific feature is needed.

**Key rule**: Always tell the agent *when* to load a reference, not just *what* it contains. "Read X when Y" is more useful than "X contains Y."

## Workflows and Checklists

For complex multi-step processes, provide a workflow with a copy-paste checklist that agents can track.

**When to use workflows:**
- Process has 3+ sequential steps
- Steps must be completed in order
- Easy to skip critical steps
- Progress tracking is valuable

**Pattern:**
```markdown
## [Process Name] Workflow

Copy this checklist and track your progress:

```
Task Progress:
- [ ] Step 1: [First action]
- [ ] Step 2: [Second action]
- [ ] Step 3: [Third action]
- [ ] Step 4: [Fourth action]
- [ ] Step 5: [Final verification]
```

**Step 1: [First action]**
[Detailed instructions for step 1]

**Step 2: [Second action]**
[Detailed instructions for step 2]

...
```

**Example (code-based workflow):**
```markdown
## PDF Form Filling Workflow

Copy this checklist and check off items as you complete them:

```
Task Progress:
- [ ] Step 1: Analyze the form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py)
- [ ] Step 4: Fill the form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py)
```

**Step 1: Analyze the form**
Run: `python scripts/analyze_form.py input.pdf`
This extracts form fields and their locations, saving to `fields.json`.

**Step 2: Create field mapping**
Edit `fields.json` to add values for each field.

**Step 3: Validate mapping**
Run: `python scripts/validate_fields.py fields.json`
Fix any validation errors before continuing.

**Step 4: Fill the form**
Run: `python scripts/fill_form.py input.pdf fields.json output.pdf`

**Step 5: Verify output**
Run: `python scripts/verify_output.py output.pdf`
If verification fails, return to Step 2.
```

**Example (analysis workflow):**
```markdown
## Research Synthesis Workflow

Copy this checklist and track your progress:

```
Research Progress:
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary
- [ ] Step 5: Verify citations
```

**Step 1: Read all source documents**
Review each document in the `sources/` directory. Note the main arguments and supporting evidence.

**Step 2: Identify key themes**
Look for patterns across sources. What themes appear repeatedly? Where do sources agree or disagree?

**Step 3: Cross-reference claims**
For each major claim, verify it appears in the source material. Note which source supports each point.

**Step 4: Create structured summary**
Organize findings by theme. Include:
- Main claim
- Supporting evidence from sources
- Conflicting viewpoints (if any)

**Step 5: Verify citations**
Check that every claim references the correct source document. If citations are incomplete, return to Step 3.
```

## Feedback Loops

For tasks where quality depends on validation, implement a "validate → fix → repeat" pattern.

**When to use feedback loops:**
- Output quality is critical
- Validation can be automated or systematic
- Errors are common and fixable
- Multiple iterations improve results

**Pattern:**
```markdown
## [Process Name]

1. [Perform initial action]
2. **Validate immediately**: [validation command or checklist]
3. If validation fails:
   - Review the error message/issues
   - Fix the problems
   - Run validation again
4. **Only proceed when validation passes**
5. [Continue with next steps]
```

**Example (with validation script):**
```markdown
## Document Editing Process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message carefully
   - Fix the issues in the XML
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python scripts/pack.py unpacked_dir/ output.docx`
6. Test the output document
```

**Example (with reference document):**
```markdown
## Content Review Process

1. Draft your content following the guidelines in STYLE_GUIDE.md
2. Review against the checklist:
   - Check terminology consistency
   - Verify examples follow the standard format
   - Confirm all required sections are present
3. If issues found:
   - Note each issue with specific section reference
   - Revise the content
   - Review the checklist again
4. Only proceed when all requirements are met
5. Finalize and save the document
```

**Key principle**: Make validation explicit and mandatory before proceeding. Use bold text like **"Only proceed when validation passes"** to emphasize critical checkpoints.

## Template Patterns

Provide templates for output format. Match the level of strictness to your needs.

**Strict templates** - Use when format is critical (APIs, data formats, required structure):

```markdown
## Report Structure

**ALWAYS use this exact template structure:**

```markdown
# [Analysis Title]

## Executive Summary
[One-paragraph overview of key findings]

## Key Findings
- Finding 1 with supporting data
- Finding 2 with supporting data
- Finding 3 with supporting data

## Recommendations
1. Specific actionable recommendation
2. Specific actionable recommendation
```

Do not deviate from this structure.
```

**Flexible templates** - Use when adaptation is valuable (content varies, context matters):

```markdown
## Report Structure

Here is a sensible default format, but use your best judgment based on the analysis:

```markdown
# [Analysis Title]

## Executive Summary
[Overview]

## Key Findings
[Adapt sections based on what you discover]

## Recommendations
[Tailor to the specific context]
```

Adjust sections as needed for the specific analysis type.
```

**When to be strict:**
- API responses or data formats
- Required compliance/regulatory formats
- Automated processing depends on structure
- Consistency is more important than flexibility

**When to be flexible:**
- Content varies significantly by context
- Agent judgment improves output
- Structure should adapt to findings
- Creativity and adaptation are valuable
