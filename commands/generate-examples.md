---
name: generate-examples
description: generate_code_examples - Production-grade code example generator with multi-language support
allowed-tools: Read
version: "2.0.0"
sasmp_version: "1.3.0"

# Command Configuration
command_config:
  verb_noun: generate_code_examples
  aliases: [examples, code-gen, samples]
  category: documentation

# Input Validation
input_validation:
  required_args:
    - name: languages
      type: array
      min_items: 1
  optional_args:
    - name: type
      type: enum
      values: [basic, common-task, advanced, integration, error-handling]
    - name: complexity
      type: enum
      values: [beginner, intermediate, advanced]
    - name: include-tests
      type: boolean
      default: false

# Exit Codes
exit_codes:
  0: success
  1: validation_error
  2: generation_error
  3: syntax_validation_failed
---

# /generate-examples - Code Example Generator v2.0

## Quick Reference

```
Usage: /generate-examples --languages LANGS [--type TYPE] [--complexity LEVEL]

Arguments:
  --languages    javascript,python,go,... (required, comma-separated)
  --type         basic | common-task | advanced | integration | error-handling
  --complexity   beginner | intermediate | advanced (default: intermediate)
  --include-tests  Include unit test examples (default: false)

Examples:
  /generate-examples --languages javascript,python
  /generate-examples --languages typescript --type integration
  /generate-examples --languages go --complexity advanced --include-tests
```

## Input Schema

```typescript
interface GenerateExamplesInput {
  // Required
  languages: Language[];

  // Optional
  type?: 'basic' | 'common-task' | 'advanced' | 'integration' | 'error-handling';
  complexity?: 'beginner' | 'intermediate' | 'advanced';
  include_tests?: boolean;

  // Context
  context?: {
    api_endpoint?: string;
    library_name?: string;
    framework?: string;
    use_case?: string;
  };
}

type Language = 'javascript' | 'typescript' | 'python' | 'go' | 'rust' |
                'java' | 'csharp' | 'php' | 'ruby' | 'kotlin' | 'swift';
```

## Output Schema

```typescript
interface GenerateExamplesOutput {
  status: 'success' | 'partial' | 'failed';
  exit_code: 0 | 1 | 2 | 3;

  examples: LanguageExample[];

  validation: {
    syntax_valid: boolean;
    all_imports_valid: boolean;
    security_issues: SecurityIssue[];
  };

  metadata: {
    languages_generated: string[];
    total_lines: number;
    processing_time_ms: number;
  };
}

interface LanguageExample {
  language: string;
  code: string;
  explanation: string;
  dependencies: Dependency[];
  run_command: string;
  expected_output: string;
  test_code?: string;
}
```

## Supported Languages

| Language | Features | Style Guide |
|----------|----------|-------------|
| JavaScript | ES2024, async/await | Airbnb |
| TypeScript | Strict types, generics | Google |
| Python | Type hints, async | PEP 8 |
| Go | Goroutines, channels | Effective Go |
| Rust | Ownership, Result/Option | Rust Book |
| Java | Streams, OOP | Google |
| C# | LINQ, async | Microsoft |
| PHP | Modern PHP 8.x | PSR-12 |
| Ruby | Blocks, idioms | Ruby Style |
| Kotlin | Coroutines | Kotlin Style |
| Swift | Protocol-oriented | Swift API |

## Example Types

| Type | Focus | Use Case |
|------|-------|----------|
| `basic` | Hello world, minimal | Getting started docs |
| `common-task` | Typical operations | Feature documentation |
| `advanced` | Complex patterns | Deep dive guides |
| `integration` | API/service usage | Integration guides |
| `error-handling` | Exception patterns | Troubleshooting docs |

## Generated Code Features

```yaml
all_examples_include:
  - Complete imports
  - Error handling
  - Type annotations (where applicable)
  - Comments explaining logic
  - Expected output
  - Run instructions
  - Security best practices

optional_features:
  - Unit test templates
  - Performance considerations
  - Alternative approaches
  - Common pitfalls
```

## Output Structure

```markdown
## Example: ${feature} in ${language}

### Prerequisites
- ${dependency_1} (${version})
- ${dependency_2} (${version})

### Installation
\`\`\`bash
${install_command}
\`\`\`

### Code
\`\`\`${language}
${code}
\`\`\`

### Explanation
${line_by_line_explanation}

### Run
\`\`\`bash
${run_command}
\`\`\`

### Expected Output
\`\`\`
${output}
\`\`\`

### Common Variations
- ${variation_1}
- ${variation_2}

### Error Handling
${error_patterns}
```

## Quality Checks

```yaml
syntax_validation:
  javascript: eslint --no-eslintrc
  typescript: tsc --noEmit
  python: python -m py_compile
  go: go build -o /dev/null
  rust: rustc --edition 2021 --emit=metadata

security_checks:
  - No hardcoded secrets
  - No eval() usage
  - No SQL injection patterns
  - Safe deserialization
```

## Error Handling

| Exit Code | Condition | Recovery |
|-----------|-----------|----------|
| 0 | All examples valid | Use directly |
| 1 | Invalid input | Check language names |
| 2 | Generation failed | Simplify request |
| 3 | Syntax validation failed | Review generated code |

## Integration

```yaml
invokes_skills:
  - code-examples (primary)

related_commands:
  - /write-docs (embed examples)
  - /api-template (endpoint examples)
  - /review-docs (validate examples)
```

## Best Practices

**DO:**
- Request multiple languages at once
- Specify framework context
- Include error handling examples
- Test before publishing

**DON'T:**
- Use without review
- Ignore security warnings
- Skip dependency listing
- Hardcode credentials

## Troubleshooting

### Issue: Syntax errors in output
**Solution:** Specify framework version in context

### Issue: Missing imports
**Solution:** Re-run with explicit library name

### Issue: Examples too complex
**Solution:** Use `--complexity beginner`

---

**Version:** 2.0.0 | **Languages:** 11 | **Validation:** Syntax + Security
