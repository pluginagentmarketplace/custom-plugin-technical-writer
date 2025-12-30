---
name: write-docs
description: write_documentation - Production-grade documentation project assistant
allowed-tools: Read
version: "2.0.0"
sasmp_version: "1.3.0"

# Command Configuration
command_config:
  verb_noun: write_documentation
  aliases: [docs, create-docs]
  category: documentation

# Input Validation
input_validation:
  required_args: []
  optional_args:
    - name: type
      type: enum
      values: [api, user-guide, developer-guide, getting-started, reference, release-notes]
    - name: audience
      type: enum
      values: [beginner, intermediate, advanced, mixed]
    - name: format
      type: enum
      values: [markdown, html, rst, docx]

# Exit Codes
exit_codes:
  0: success
  1: validation_error
  2: generation_error
  3: user_cancelled
---

# /write-docs - Documentation Project Assistant v2.0

## Quick Reference

```
Usage: /write-docs [--type TYPE] [--audience LEVEL] [--format FORMAT]

Arguments:
  --type       api | user-guide | developer-guide | getting-started | reference | release-notes
  --audience   beginner | intermediate | advanced | mixed (default: mixed)
  --format     markdown | html | rst | docx (default: markdown)

Examples:
  /write-docs
  /write-docs --type api --audience intermediate
  /write-docs --type user-guide --format html
```

## Input Schema

```typescript
interface WriteDocsInput {
  type?: 'api' | 'user-guide' | 'developer-guide' | 'getting-started' | 'reference' | 'release-notes';
  audience?: 'beginner' | 'intermediate' | 'advanced' | 'mixed';
  format?: 'markdown' | 'html' | 'rst' | 'docx';
  context?: {
    product_name?: string;
    product_version?: string;
    existing_docs?: string[];
  };
}
```

## Output Schema

```typescript
interface WriteDocsOutput {
  status: 'success' | 'partial' | 'failed';
  exit_code: 0 | 1 | 2 | 3;
  content: {
    template: string;
    sections: Section[];
    best_practices: string[];
  };
  metadata: {
    doc_type: string;
    estimated_pages: number;
    estimated_time: string;
  };
}
```

## Documentation Types

| Type | Best For | Pages | Time |
|------|----------|-------|------|
| `api` | REST/GraphQL APIs | 30-50 | 2-4 weeks |
| `user-guide` | End-user products | 20-40 | 2-3 weeks |
| `developer-guide` | Developer tools/SDKs | 40-60 | 3-4 weeks |
| `getting-started` | Quick onboarding | 5-10 | 1 week |
| `reference` | Technical specs | 30-100+ | 3-6 weeks |
| `release-notes` | Version updates | 2-5 | 1-2 hours |

## Workflow

```
1. /write-docs                    # Start project
2. Choose documentation type      # Select from 6 types
3. Define target audience         # Specify skill level
4. Receive template               # Get structured outline
5. Fill in sections               # Write content
6. /review-docs                   # Get feedback
7. /generate-examples             # Add code samples
8. Publish                        # Deploy documentation
```

## Type-Specific Outputs

### API Documentation
```yaml
output_includes:
  - OpenAPI/Swagger template (3.1)
  - Endpoint documentation structure
  - Authentication guide template
  - Error code reference
  - Rate limiting documentation
  - Multi-language code examples
```

### User Guide
```yaml
output_includes:
  - Getting started section
  - Feature walkthroughs
  - Screenshot placeholders
  - Troubleshooting guide
  - FAQ template
  - Next steps
```

### Getting Started
```yaml
output_includes:
  - Prerequisites checklist
  - 5-minute setup guide
  - First task tutorial
  - Quick reference card
  - Common pitfalls
```

## Error Handling

| Exit Code | Condition | Recovery |
|-----------|-----------|----------|
| 0 | Success | None required |
| 1 | Invalid input | Check arguments |
| 2 | Generation failed | Retry with simpler config |
| 3 | User cancelled | Re-run command |

## Integration

```yaml
invokes_skills:
  - user-guides (for tutorials)
  - api-documentation (for API docs)
  - code-examples (for samples)

related_commands:
  - /review-docs (feedback)
  - /generate-examples (code)
  - /api-template (OpenAPI)
```

## Best Practices

**DO:**
- Start with clear audience definition
- Use provided templates as skeleton
- Include examples for complex topics
- Test all code samples
- Get peer review via `/review-docs`

**DON'T:**
- Skip prerequisites section
- Write without target audience
- Include outdated examples
- Ignore accessibility
- Publish without review

## Troubleshooting

### Issue: Wrong template generated
**Solution:** Specify `--type` explicitly

### Issue: Too technical for audience
**Solution:** Use `--audience beginner`

### Issue: Missing sections
**Solution:** Provide more context about product

---

**Version:** 2.0.0 | **Exit Codes:** Standardized | **Validation:** Schema-based
