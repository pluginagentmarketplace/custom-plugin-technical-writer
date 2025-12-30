---
name: api-template
description: generate_api_template - Production-grade OpenAPI/Swagger template generator
allowed-tools: Read
version: "2.0.0"
sasmp_version: "1.3.0"

# Command Configuration
command_config:
  verb_noun: generate_api_template
  aliases: [openapi, swagger, api-spec]
  category: documentation

# Input Validation
input_validation:
  required_args: []
  optional_args:
    - name: api-type
      type: enum
      values: [rest, graphql, websocket, grpc]
    - name: template
      type: enum
      values: [simple, enterprise, microservices, public]
    - name: auth
      type: enum
      values: [api-key, bearer, oauth2, basic, none]
    - name: version
      type: enum
      values: ["3.0.0", "3.1.0"]

# Exit Codes
exit_codes:
  0: success_valid_spec
  1: validation_warnings
  2: generation_error
  3: invalid_input
---

# /api-template - OpenAPI Template Generator v2.0

## Quick Reference

```
Usage: /api-template [--api-type TYPE] [--template TEMPLATE] [--auth AUTH]

Arguments:
  --api-type   rest | graphql | websocket | grpc (default: rest)
  --template   simple | enterprise | microservices | public
  --auth       api-key | bearer | oauth2 | basic | none
  --version    3.0.0 | 3.1.0 (default: 3.1.0)

Examples:
  /api-template
  /api-template --template enterprise --auth oauth2
  /api-template --api-type graphql
```

## Input Schema

```typescript
interface APITemplateInput {
  api_type?: 'rest' | 'graphql' | 'websocket' | 'grpc';
  template?: 'simple' | 'enterprise' | 'microservices' | 'public';
  auth?: 'api-key' | 'bearer' | 'oauth2' | 'basic' | 'none';
  version?: '3.0.0' | '3.1.0';

  context?: {
    api_name?: string;
    api_version?: string;
    base_url?: string;
    description?: string;
  };
}
```

## Output Schema

```typescript
interface APITemplateOutput {
  status: 'success' | 'warnings' | 'failed';
  exit_code: 0 | 1 | 2 | 3;

  content: {
    specification: string;        // YAML OpenAPI spec
    components: ComponentSet;     // Reusable components
  };

  validation: {
    is_valid: boolean;
    errors: ValidationError[];
    warnings: ValidationWarning[];
  };

  metadata: {
    openapi_version: string;
    template_type: string;
    auth_type: string;
  };
}
```

## Template Types

| Template | Endpoints | Best For |
|----------|-----------|----------|
| `simple` | 3-5 | Small APIs, MVPs |
| `enterprise` | 10+ | Large internal APIs |
| `microservices` | Multi-spec | Service mesh |
| `public` | Complete | External developer APIs |

## Generated Specification Structure

```yaml
# OpenAPI 3.1.0 Template
openapi: 3.1.0
info:
  title: ${api_name}
  version: ${api_version}
  description: ${description}
  contact:
    name: API Support
    email: support@example.com
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: ${base_url}
    description: Production
  - url: ${staging_url}
    description: Staging

paths:
  /resources:
    get: # List
    post: # Create
  /resources/{id}:
    get: # Read
    put: # Update
    delete: # Delete

components:
  schemas: # Data models
  parameters: # Reusable params
  responses: # Standard responses
  securitySchemes: # Auth methods

security:
  - ${auth_scheme}: []
```

## Security Schemes

### API Key
```yaml
securitySchemes:
  ApiKeyAuth:
    type: apiKey
    in: header
    name: X-API-Key
```

### Bearer Token (JWT)
```yaml
securitySchemes:
  BearerAuth:
    type: http
    scheme: bearer
    bearerFormat: JWT
```

### OAuth 2.0
```yaml
securitySchemes:
  OAuth2:
    type: oauth2
    flows:
      authorizationCode:
        authorizationUrl: /oauth/authorize
        tokenUrl: /oauth/token
        scopes:
          read: Read access
          write: Write access
```

## Standard Components

### Error Schema
```yaml
Error:
  type: object
  required: [code, message]
  properties:
    code:
      type: string
      example: INVALID_REQUEST
    message:
      type: string
      example: Request validation failed
    details:
      type: object
    request_id:
      type: string
```

### Pagination Parameters
```yaml
PageParam:
  name: page
  in: query
  schema:
    type: integer
    minimum: 1
    default: 1

LimitParam:
  name: limit
  in: query
  schema:
    type: integer
    minimum: 1
    maximum: 100
    default: 20
```

### Standard Responses
```yaml
responses:
  BadRequest:
    description: Invalid request
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Error'
  Unauthorized:
    description: Authentication required
  NotFound:
    description: Resource not found
  RateLimited:
    description: Too many requests
    headers:
      Retry-After:
        schema:
          type: integer
```

## HTTP Status Codes

| Code | When | Response |
|------|------|----------|
| 200 | Success (GET, PUT) | Resource |
| 201 | Created (POST) | New resource |
| 204 | Deleted (DELETE) | Empty |
| 400 | Validation error | Error object |
| 401 | Auth missing | Error + WWW-Authenticate |
| 403 | Forbidden | Error object |
| 404 | Not found | Error object |
| 409 | Conflict | Error + existing resource |
| 429 | Rate limited | Error + Retry-After |
| 500 | Server error | Error object |

## Validation Checks

```yaml
validation:
  syntax:
    - Valid YAML/JSON
    - OpenAPI 3.x compliant
  references:
    - All $ref resolve
    - No circular refs
  completeness:
    - All paths have responses
    - All schemas have properties
  security:
    - Auth defined for protected paths
    - No secrets in examples
```

## Error Handling

| Exit Code | Condition | Action |
|-----------|-----------|--------|
| 0 | Valid spec | Ready to use |
| 1 | Warnings | Review optional fixes |
| 2 | Generation failed | Check input params |
| 3 | Invalid input | Verify arguments |

## Integration

```yaml
invokes_skills:
  - api-documentation (primary)

related_commands:
  - /write-docs (full API docs)
  - /generate-examples (endpoint examples)
  - /review-docs (spec review)

tools_compatible:
  - Swagger UI
  - ReDoc
  - Stoplight
  - Postman
  - OpenAPI Generator
```

## Best Practices

**DO:**
- Start with realistic examples
- Document all error responses
- Include rate limiting info
- Version your spec with code
- Validate before publishing

**DON'T:**
- Leave schemas undefined
- Skip authentication docs
- Include real credentials
- Ignore deprecation notices
- Mix OpenAPI versions

## Troubleshooting

### Issue: Invalid $ref errors
**Solution:** Check component paths

### Issue: Missing security
**Solution:** Specify `--auth` parameter

### Issue: Complex nested schemas
**Solution:** Break into reusable components

---

**Version:** 2.0.0 | **OpenAPI:** 3.1.0 | **Validation:** Built-in
