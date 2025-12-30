---
name: review-docs
description: review_documentation - Production-grade documentation review and quality analysis
allowed-tools: Read
version: "2.0.0"
sasmp_version: "1.3.0"

# Command Configuration
command_config:
  verb_noun: review_documentation
  aliases: [review, check-docs, analyze-docs]
  category: documentation

# Input Validation
input_validation:
  required_args: []
  optional_args:
    - name: focus
      type: enum
      values: [comprehensive, clarity, accuracy, structure, accessibility, seo]
    - name: depth
      type: enum
      values: [quick, standard, deep]
    - name: audience
      type: enum
      values: [beginner, intermediate, advanced, non-technical]

# Exit Codes
exit_codes:
  0: pass_no_issues
  1: pass_with_warnings
  2: fail_critical_issues
  3: error_invalid_input
---

# /review-docs - Documentation Review v2.0

## Quick Reference

```
Usage: /review-docs [--focus AREA] [--depth LEVEL] [--audience TARGET]

Arguments:
  --focus      comprehensive | clarity | accuracy | structure | accessibility | seo
  --depth      quick (5min) | standard (15min) | deep (30min)
  --audience   beginner | intermediate | advanced | non-technical

Examples:
  /review-docs
  /review-docs --focus clarity --depth deep
  /review-docs --focus accessibility --audience beginner
```

## Input Schema

```typescript
interface ReviewDocsInput {
  content: string;                    // Documentation to review
  focus?: 'comprehensive' | 'clarity' | 'accuracy' | 'structure' | 'accessibility' | 'seo';
  depth?: 'quick' | 'standard' | 'deep';
  audience?: 'beginner' | 'intermediate' | 'advanced' | 'non-technical';
  context?: {
    product_name?: string;
    doc_type?: string;
    previous_issues?: string[];
  };
}
```

## Output Schema

```typescript
interface ReviewDocsOutput {
  status: 'pass' | 'pass_with_warnings' | 'fail';
  exit_code: 0 | 1 | 2 | 3;

  scores: {
    clarity: number;        // 0-100
    completeness: number;   // 0-100
    accuracy: number;       // 0-100
    structure: number;      // 0-100
    accessibility: number;  // 0-100
    overall: number;        // 0-100
  };

  issues: Issue[];
  suggestions: Suggestion[];

  summary: {
    critical_count: number;
    warning_count: number;
    info_count: number;
  };
}

interface Issue {
  severity: 'critical' | 'warning' | 'info';
  category: string;
  message: string;
  location?: string;
  suggested_fix?: string;
}
```

## Review Categories

| Focus Area | Checks | Priority |
|------------|--------|----------|
| `clarity` | Language, jargon, sentence length | High |
| `completeness` | Missing sections, examples | High |
| `accuracy` | Technical correctness, versions | Critical |
| `structure` | Hierarchy, navigation, flow | Medium |
| `accessibility` | Alt text, headings, contrast | Medium |
| `seo` | Keywords, meta, links | Low |

## Depth Levels

| Level | Time | Coverage |
|-------|------|----------|
| `quick` | 5 min | Major issues only |
| `standard` | 15 min | Full analysis + suggestions |
| `deep` | 30 min | Section-by-section + rewrites |

## Quality Thresholds

```yaml
thresholds:
  pass: overall >= 80
  pass_with_warnings: overall >= 60
  fail: overall < 60

scoring_weights:
  clarity: 25%
  completeness: 25%
  accuracy: 20%
  structure: 15%
  accessibility: 15%
```

## Issue Categories

### Critical (Must Fix)
- Incorrect technical information
- Non-functional code examples
- Security vulnerabilities in examples
- Broken links to critical resources

### Warning (Should Fix)
- Complex sentences (> 25 words)
- Missing examples for key features
- Inconsistent terminology
- Missing alt text for images

### Info (Consider)
- Style guide deviations
- SEO improvements
- Minor formatting issues
- Enhancement suggestions

## Output Format

```yaml
# Review Summary
status: pass_with_warnings
exit_code: 1
overall_score: 78/100

# Score Breakdown
scores:
  clarity: 85
  completeness: 72
  accuracy: 90
  structure: 75
  accessibility: 68

# Issues Found
issues:
  critical: 0
  warnings: 5
  info: 12

# Top 3 Priorities
priorities:
  1: "Add examples to Authentication section"
  2: "Simplify paragraph in 'Advanced Usage'"
  3: "Add alt text to 3 images"
```

## Error Handling

| Exit Code | Meaning | Action |
|-----------|---------|--------|
| 0 | Pass, no issues | Ready to publish |
| 1 | Pass with warnings | Review suggestions |
| 2 | Fail, critical issues | Must fix before publish |
| 3 | Invalid input | Check documentation format |

## Integration

```yaml
invokes_skills:
  - user-guides (for readability analysis)
  - api-documentation (for spec validation)

related_commands:
  - /write-docs (create content)
  - /generate-examples (fix code gaps)
  - /api-template (validate specs)
```

## Best Practices

**Before Review:**
- Complete all major sections
- Test code examples
- Check links manually
- Define target audience

**After Review:**
- Address critical issues first
- Group similar fixes
- Re-review after major changes
- Document decisions made

## Troubleshooting

### Issue: Score unexpectedly low
**Check:** Audience mismatch, missing examples

### Issue: Many false positives
**Solution:** Specify correct `--audience` level

### Issue: Review times out
**Solution:** Split into smaller sections

---

**Version:** 2.0.0 | **Exit Codes:** 0-3 | **Quality Gate:** 80+
