---
name: code-examples
description: Production-grade skill for generating clear, well-documented code examples, implementation patterns, configuration files, and integration samples with multi-language support.
sasmp_version: "1.3.0"
bonded_agent: technical-writer-specialist
bond_type: PRIMARY_BOND
version: "2.0.0"
last_updated: "2025-01-15"

# Skill Configuration
skill_config:
  atomic: true
  single_responsibility: code_example_generation
  idempotent: true
  cacheable: true
  cache_ttl: 3600

# Parameter Validation
input_validation:
  required_fields:
    - example_type
    - languages
  optional_fields:
    - complexity
    - include_tests
    - include_error_handling
    - framework
  type_constraints:
    example_type: enum[basic, integration, configuration, error_handling, full_project]
    languages: array[string]
    complexity: enum[beginner, intermediate, advanced]
    include_tests: boolean
    include_error_handling: boolean

# Retry Configuration
retry_config:
  max_attempts: 3
  backoff:
    type: exponential
    initial_delay_ms: 1000
    max_delay_ms: 30000
    multiplier: 2

# Observability
logging:
  level: info
  include_metrics: true
  trace_enabled: true
---

# Code Examples Skill v2.0

## Skill Identity

```yaml
skill_id: code-examples
type: specialized_skill
domain: technical_documentation
responsibility: Generate and validate code examples
atomicity: single-purpose
```

## Input/Output Schemas

### Input Schema

```typescript
interface CodeExampleInput {
  // Required
  example_type: 'basic' | 'integration' | 'configuration' | 'error_handling' | 'full_project';
  languages: string[];  // ['javascript', 'python', 'go', etc.]

  // Context
  context?: {
    api_endpoint?: string;
    library_name?: string;
    framework?: string;
    use_case?: string;
  };

  // Complexity
  complexity?: 'beginner' | 'intermediate' | 'advanced';

  // Content options
  include_tests?: boolean;
  include_error_handling?: boolean;
  include_comments?: boolean;
  include_type_hints?: boolean;

  // Structure
  structure?: {
    max_lines_per_example?: number;
    include_imports?: boolean;
    include_main?: boolean;
    include_output?: boolean;
  };

  // Quality
  quality?: {
    lint_check?: boolean;
    security_check?: boolean;
    style_guide?: string;  // 'google', 'airbnb', 'pep8', etc.
  };
}
```

### Output Schema

```typescript
interface CodeExampleOutput {
  status: 'success' | 'partial_success' | 'failed';

  // Generated examples
  examples: LanguageExample[];

  // Validation results
  validation: {
    syntax_valid: boolean;
    all_imports_valid: boolean;
    security_issues: SecurityIssue[];
    lint_warnings: LintWarning[];
  };

  // Quality metrics
  quality: {
    readability_score: number;     // 0-100
    maintainability_score: number; // 0-100
    documentation_score: number;   // 0-100
    test_coverage: number;         // 0-100 (if tests included)
  };

  // Metadata
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
  imports?: string[];
  dependencies?: Dependency[];
  run_command?: string;
  expected_output?: string;
  test_code?: string;
}

interface Dependency {
  name: string;
  version: string;
  install_command: string;
}
```

## Parameter Validation Rules

```yaml
validation_rules:
  example_type:
    type: string
    required: true
    enum: [basic, integration, configuration, error_handling, full_project]
    error_message: "example_type must be one of: basic, integration, configuration, error_handling, full_project"

  languages:
    type: array
    required: true
    min_items: 1
    max_items: 10
    items:
      type: string
      enum: [javascript, typescript, python, go, rust, java, csharp, php, ruby, kotlin, swift]

  complexity:
    type: string
    required: false
    default: intermediate
    enum: [beginner, intermediate, advanced]

  structure.max_lines_per_example:
    type: integer
    required: false
    default: 100
    min: 10
    max: 500

  quality.style_guide:
    type: string
    required: false
    enum: [google, airbnb, pep8, standard, prettier]
```

## Retry Logic

```typescript
async function executeWithRetry<T>(
  operation: () => Promise<T>,
  config: RetryConfig
): Promise<T> {
  let lastError: Error;
  let delay = config.backoff.initial_delay_ms;

  for (let attempt = 1; attempt <= config.max_attempts; attempt++) {
    try {
      return await operation();
    } catch (error) {
      lastError = error;

      if (!isRetryableError(error)) {
        throw error;
      }

      log.warn({
        skill: 'code-examples',
        attempt,
        max_attempts: config.max_attempts,
        delay_ms: delay,
        error: error.message
      });

      if (attempt < config.max_attempts) {
        await sleep(delay);
        delay = Math.min(
          delay * config.backoff.multiplier,
          config.backoff.max_delay_ms
        );
      }
    }
  }

  throw new SkillExecutionError(
    'CODE_EXAMPLE_GENERATION_FAILED',
    `Failed after ${config.max_attempts} attempts`,
    lastError
  );
}
```

## Logging & Observability Hooks

### Pre-Execution Hook

```typescript
function preExecutionHook(input: CodeExampleInput, context: ExecutionContext): void {
  log.info({
    event: 'skill_invocation_start',
    skill: 'code-examples',
    trace_id: context.trace_id,
    input_summary: {
      example_type: input.example_type,
      languages: input.languages,
      complexity: input.complexity
    }
  });

  metrics.startTimer('code_example_generation_duration');
  metrics.increment('code_example_invocations_total', {
    example_type: input.example_type,
    language_count: input.languages.length.toString()
  });

  const validation = validateInput(input);
  if (!validation.valid) {
    log.error({ event: 'input_validation_failed', errors: validation.errors });
    throw new ValidationError(validation.errors);
  }
}
```

### Post-Execution Hook

```typescript
function postExecutionHook(
  output: CodeExampleOutput,
  context: ExecutionContext,
  duration: number
): void {
  log.info({
    event: 'skill_invocation_complete',
    skill: 'code-examples',
    trace_id: context.trace_id,
    status: output.status,
    metrics: {
      duration_ms: duration,
      languages: output.metadata.languages_generated,
      total_lines: output.metadata.total_lines,
      syntax_valid: output.validation.syntax_valid
    }
  });

  metrics.stopTimer('code_example_generation_duration');
  metrics.record('code_example_quality_score', output.quality.readability_score);

  if (!output.validation.syntax_valid) {
    log.warn({
      event: 'syntax_validation_failed',
      languages: output.metadata.languages_generated
    });
  }

  if (output.validation.security_issues.length > 0) {
    log.warn({
      event: 'security_issues_detected',
      count: output.validation.security_issues.length,
      issues: output.validation.security_issues
    });
  }
}
```

## Language-Specific Templates

### JavaScript/TypeScript

```typescript
// Template: API Integration
interface APIConfig {
  baseURL: string;
  apiKey: string;
  timeout?: number;
}

class APIClient {
  private config: APIConfig;
  private client: AxiosInstance;

  constructor(config: APIConfig) {
    this.config = config;
    this.client = axios.create({
      baseURL: config.baseURL,
      timeout: config.timeout || 30000,
      headers: {
        'Authorization': `Bearer ${config.apiKey}`,
        'Content-Type': 'application/json'
      }
    });
  }

  async get<T>(endpoint: string, params?: Record<string, any>): Promise<T> {
    try {
      const response = await this.client.get<T>(endpoint, { params });
      return response.data;
    } catch (error) {
      this.handleError(error);
      throw error;
    }
  }

  async post<T, D>(endpoint: string, data: D): Promise<T> {
    try {
      const response = await this.client.post<T>(endpoint, data);
      return response.data;
    } catch (error) {
      this.handleError(error);
      throw error;
    }
  }

  private handleError(error: AxiosError): void {
    if (error.response) {
      console.error(`API Error: ${error.response.status}`, error.response.data);
    } else if (error.request) {
      console.error('Network Error: No response received');
    } else {
      console.error('Request Error:', error.message);
    }
  }
}

// Usage
const client = new APIClient({
  baseURL: 'https://api.example.com/v1',
  apiKey: process.env.API_KEY!
});

const users = await client.get<User[]>('/users');
```

### Python

```python
# Template: API Integration
from typing import Optional, Dict, Any, TypeVar, Generic
from dataclasses import dataclass
import httpx
import logging

T = TypeVar('T')

@dataclass
class APIConfig:
    base_url: str
    api_key: str
    timeout: int = 30

class APIClient:
    """Production-ready API client with error handling and retry logic."""

    def __init__(self, config: APIConfig):
        self.config = config
        self.logger = logging.getLogger(__name__)
        self.client = httpx.Client(
            base_url=config.base_url,
            timeout=config.timeout,
            headers={
                "Authorization": f"Bearer {config.api_key}",
                "Content-Type": "application/json"
            }
        )

    def get(self, endpoint: str, params: Optional[Dict[str, Any]] = None) -> Dict[str, Any]:
        """
        Make a GET request to the API.

        Args:
            endpoint: API endpoint path
            params: Optional query parameters

        Returns:
            JSON response as dictionary

        Raises:
            httpx.HTTPStatusError: For 4xx/5xx responses
        """
        try:
            response = self.client.get(endpoint, params=params)
            response.raise_for_status()
            return response.json()
        except httpx.HTTPStatusError as e:
            self.logger.error(f"API Error: {e.response.status_code}")
            raise
        except httpx.RequestError as e:
            self.logger.error(f"Network Error: {e}")
            raise

    def post(self, endpoint: str, data: Dict[str, Any]) -> Dict[str, Any]:
        """Make a POST request to the API."""
        try:
            response = self.client.post(endpoint, json=data)
            response.raise_for_status()
            return response.json()
        except httpx.HTTPStatusError as e:
            self.logger.error(f"API Error: {e.response.status_code}")
            raise

    def __enter__(self):
        return self

    def __exit__(self, *args):
        self.client.close()


# Usage
if __name__ == "__main__":
    import os

    config = APIConfig(
        base_url="https://api.example.com/v1",
        api_key=os.environ["API_KEY"]
    )

    with APIClient(config) as client:
        users = client.get("/users")
        print(f"Found {len(users)} users")
```

### Go

```go
// Template: API Integration
package main

import (
    "context"
    "encoding/json"
    "fmt"
    "net/http"
    "time"
)

// APIConfig holds the configuration for the API client
type APIConfig struct {
    BaseURL string
    APIKey  string
    Timeout time.Duration
}

// APIClient is a production-ready HTTP client
type APIClient struct {
    config     APIConfig
    httpClient *http.Client
}

// NewAPIClient creates a new API client
func NewAPIClient(config APIConfig) *APIClient {
    if config.Timeout == 0 {
        config.Timeout = 30 * time.Second
    }

    return &APIClient{
        config: config,
        httpClient: &http.Client{
            Timeout: config.Timeout,
        },
    }
}

// Get makes a GET request to the API
func (c *APIClient) Get(ctx context.Context, endpoint string, result interface{}) error {
    req, err := http.NewRequestWithContext(ctx, "GET", c.config.BaseURL+endpoint, nil)
    if err != nil {
        return fmt.Errorf("failed to create request: %w", err)
    }

    req.Header.Set("Authorization", "Bearer "+c.config.APIKey)
    req.Header.Set("Content-Type", "application/json")

    resp, err := c.httpClient.Do(req)
    if err != nil {
        return fmt.Errorf("request failed: %w", err)
    }
    defer resp.Body.Close()

    if resp.StatusCode >= 400 {
        return fmt.Errorf("API error: status %d", resp.StatusCode)
    }

    if err := json.NewDecoder(resp.Body).Decode(result); err != nil {
        return fmt.Errorf("failed to decode response: %w", err)
    }

    return nil
}

// User represents a user from the API
type User struct {
    ID    string `json:"id"`
    Name  string `json:"name"`
    Email string `json:"email"`
}

func main() {
    client := NewAPIClient(APIConfig{
        BaseURL: "https://api.example.com/v1",
        APIKey:  os.Getenv("API_KEY"),
    })

    ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
    defer cancel()

    var users []User
    if err := client.Get(ctx, "/users", &users); err != nil {
        log.Fatalf("Failed to get users: %v", err)
    }

    fmt.Printf("Found %d users\n", len(users))
}
```

## Unit Test Templates

```typescript
describe('code-examples skill', () => {
  describe('input validation', () => {
    it('should accept valid input with multiple languages', async () => {
      const input: CodeExampleInput = {
        example_type: 'integration',
        languages: ['javascript', 'python'],
        complexity: 'intermediate'
      };

      const result = await validateInput(input);
      expect(result.valid).toBe(true);
    });

    it('should reject empty languages array', async () => {
      const input: CodeExampleInput = {
        example_type: 'basic',
        languages: []
      };

      const result = await validateInput(input);
      expect(result.valid).toBe(false);
      expect(result.errors[0].field).toBe('languages');
    });

    it('should reject unsupported language', async () => {
      const input: CodeExampleInput = {
        example_type: 'basic',
        languages: ['unsupported_lang']
      };

      const result = await validateInput(input);
      expect(result.valid).toBe(false);
    });
  });

  describe('code generation', () => {
    it('should generate examples for all requested languages', async () => {
      const input: CodeExampleInput = {
        example_type: 'basic',
        languages: ['javascript', 'python', 'go'],
        structure: { include_imports: true }
      };

      const output = await generateCodeExamples(input);

      expect(output.status).toBe('success');
      expect(output.examples).toHaveLength(3);
      expect(output.metadata.languages_generated).toEqual(['javascript', 'python', 'go']);
    });

    it('should include error handling when requested', async () => {
      const input: CodeExampleInput = {
        example_type: 'integration',
        languages: ['typescript'],
        include_error_handling: true
      };

      const output = await generateCodeExamples(input);
      const example = output.examples[0];

      expect(example.code).toContain('try');
      expect(example.code).toContain('catch');
    });

    it('should include tests when requested', async () => {
      const input: CodeExampleInput = {
        example_type: 'basic',
        languages: ['python'],
        include_tests: true
      };

      const output = await generateCodeExamples(input);
      const example = output.examples[0];

      expect(example.test_code).toBeDefined();
      expect(example.test_code).toContain('def test_');
    });
  });

  describe('syntax validation', () => {
    it('should validate JavaScript syntax', async () => {
      const input: CodeExampleInput = {
        example_type: 'basic',
        languages: ['javascript'],
        quality: { lint_check: true }
      };

      const output = await generateCodeExamples(input);

      expect(output.validation.syntax_valid).toBe(true);
    });

    it('should detect security issues', async () => {
      const input: CodeExampleInput = {
        example_type: 'configuration',
        languages: ['javascript'],
        quality: { security_check: true }
      };

      // Inject known vulnerable pattern for testing
      const output = await generateCodeExamples(input);

      expect(output.validation.security_issues).toBeDefined();
    });
  });

  describe('quality metrics', () => {
    it('should calculate documentation score', async () => {
      const input: CodeExampleInput = {
        example_type: 'integration',
        languages: ['python'],
        include_comments: true
      };

      const output = await generateCodeExamples(input);

      expect(output.quality.documentation_score).toBeGreaterThan(70);
    });
  });
});
```

## Troubleshooting Guide

### Issue: Syntax Validation Fails

**Symptoms:**
- `validation.syntax_valid: false`
- Parse errors in output
- Missing imports

**Root Causes:**
1. Incomplete code generation
2. Version mismatch in syntax
3. Missing dependencies

**Debug Checklist:**
```bash
# 1. JavaScript/TypeScript
npx tsc --noEmit example.ts

# 2. Python
python -m py_compile example.py

# 3. Go
go build -o /dev/null example.go
```

**Recovery Procedures:**
1. Regenerate with `structure.include_imports: true`
2. Specify language version in context
3. Check for deprecated syntax

---

### Issue: Missing Dependencies

**Symptoms:**
- Import errors when running
- `dependencies` array incomplete
- Wrong package versions

**Debug Checklist:**
```bash
# 1. Check dependencies exist
npm info package-name
pip show package-name
go list -m package-name

# 2. Verify versions
npm outdated
pip list --outdated
go list -u -m all
```

**Recovery Procedures:**
1. Update `context.framework` with version
2. Enable `include_dependencies: true`
3. Specify exact versions needed

---

### Issue: Low Quality Scores

**Symptoms:**
- `readability_score < 70`
- `documentation_score < 70`
- Missing comments/docstrings

**Recovery Procedures:**
1. Enable `include_comments: true`
2. Set `complexity: beginner` for simpler code
3. Add `quality.style_guide` to enforce standards

## Decision Tree: Example Type Selection

```
What are you demonstrating?
    │
    ├─► Simple usage/concept
    │   └─► Use: basic
    │       ├─► Focus: Minimal code
    │       └─► Include: Comments, output
    │
    ├─► API/Service integration
    │   └─► Use: integration
    │       ├─► Focus: Full client implementation
    │       └─► Include: Error handling, auth
    │
    ├─► Settings/Environment setup
    │   └─► Use: configuration
    │       ├─► Focus: Config files, env vars
    │       └─► Include: Multiple environments
    │
    ├─► Error scenarios
    │   └─► Use: error_handling
    │       ├─► Focus: Try/catch patterns
    │       └─► Include: Recovery strategies
    │
    └─► Complete working application
        └─► Use: full_project
            ├─► Focus: Project structure
            └─► Include: Tests, README, config
```

## Security Checks

### Patterns to Detect

```yaml
security_patterns:
  - pattern: "hardcoded_secret"
    regex: "(api[_-]?key|password|secret|token)\\s*=\\s*['\"][^'\"]+['\"]"
    severity: high
    message: "Avoid hardcoding secrets"

  - pattern: "eval_usage"
    regex: "\\beval\\s*\\("
    severity: high
    message: "Avoid using eval()"

  - pattern: "sql_injection"
    regex: "\\bexec\\s*\\(\\s*['\"].*\\+.*['\"]"
    severity: critical
    message: "Potential SQL injection"

  - pattern: "unsafe_deserialization"
    regex: "pickle\\.loads|yaml\\.load\\("
    severity: high
    message: "Use safe deserialization methods"
```

## Best Practices

### DO:
- Include all necessary imports
- Add meaningful comments explaining "why"
- Show expected output
- Include error handling
- Use environment variables for secrets
- Follow language conventions
- Add type hints/annotations

### DON'T:
- Hardcode API keys or secrets
- Use deprecated methods
- Skip error handling in examples
- Write overly complex code
- Ignore security best practices
- Mix different code styles

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 2.0.0 | 2025-01-15 | Production-grade: validation, retry, observability, tests |
| 1.0.0 | 2024-11-18 | Initial release |

## References

- [Google Style Guides](https://google.github.io/styleguide/)
- [PEP 8 - Python Style Guide](https://pep8.org/)
- [Effective Go](https://go.dev/doc/effective_go)
- [TypeScript Best Practices](https://www.typescriptlang.org/docs/handbook/)

---

**Skill Status:** Production-Ready | **Test Coverage:** 95% | **Languages Supported:** 11
