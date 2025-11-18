# /api-template - OpenAPI/Swagger Template Generator

## Purpose
Generate ready-to-use OpenAPI/Swagger templates and specifications for documenting your APIs. Get professional, standards-based API documentation structure instantly.

## What This Command Provides

When you run `/api-template`, you'll get:

### 1. **Basic OpenAPI Specification**
```yaml
openapi: 3.0.0
info:
  title: Your API Name
  version: 1.0.0
  description: |
    Brief description of your API

servers:
  - url: https://api.example.com/v1
    description: Production

paths:
  /endpoint:
    get:
      summary: Endpoint summary
      # ... rest of specification
```

### 2. **Complete Endpoint Template**
```yaml
/resource/{id}:
  get:
    summary: Get a resource
    tags:
      - Resources
    parameters:
      - name: id
        in: path
        required: true
        schema:
          type: string
    responses:
      '200':
        description: Success
      '404':
        description: Not found
```

### 3. **Authentication Documentation**
```yaml
components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

### 4. **Error Response Templates**
```yaml
responses:
  '400':
    description: Bad Request
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Error'
  '401':
    description: Unauthorized
  '404':
    description: Not Found
```

### 5. **Schema Definitions**
```yaml
components:
  schemas:
    User:
      type: object
      required:
        - id
        - email
      properties:
        id:
          type: string
        email:
          type: string
```

## How to Use

### Step 1: Run the Command
```
/api-template
```

### Step 2: Choose API Type
- **REST API** - Traditional HTTP REST
- **GraphQL** - Query-based API
- **Webhook** - Event-driven API
- **WebSocket** - Real-time API

### Step 3: Describe Your API
Provide:
- API name
- Version
- Main purpose
- Authentication type
- Key endpoints/resources

### Step 4: Get Your Template
Receive a complete, ready-to-edit OpenAPI specification

### Step 5: Customize
Fill in your specific endpoints, schemas, and details

## Template Options

### Option 1: Simple REST API
**For:** Small to medium APIs with few endpoints

**Includes:**
- Basic OpenAPI structure
- 3-5 endpoint examples
- Simple authentication
- Basic error handling

**Uses:**
- Swagger UI compatibility
- ReDoc compatibility
- API client generation

---

### Option 2: Enterprise API
**For:** Large, complex APIs with many endpoints

**Includes:**
- Full OpenAPI 3.0 specification
- Multiple security schemes
- Comprehensive error handling
- Rate limiting documentation
- Pagination examples
- Filter/search parameters
- Versioning strategy

---

### Option 3: Microservices Collection
**For:** Multiple related APIs

**Includes:**
- Individual specs for each service
- Cross-service references
- Shared schema definitions
- Composite API documentation

---

### Option 4: Public API
**For:** APIs published for external developers

**Includes:**
- Clear getting started section
- Comprehensive examples
- Backward compatibility notes
- Deprecation strategy
- Sandbox/test environment
- API key management

## Common API Patterns

### Pattern 1: CRUD Operations
```yaml
/items:
  get: # List all
    summary: List items
  post: # Create
    summary: Create item

/items/{id}:
  get: # Read
    summary: Get item
  put: # Update
    summary: Update item
  delete: # Delete
    summary: Delete item
```

### Pattern 2: Nested Resources
```yaml
/users/{userId}/posts:
  get: # List user's posts
/users/{userId}/posts/{postId}:
  get: # Get specific post
```

### Pattern 3: Filtering & Pagination
```yaml
/items:
  get:
    parameters:
      - name: page
        in: query
        schema:
          type: integer
      - name: limit
        in: query
        schema:
          type: integer
      - name: filter
        in: query
        schema:
          type: string
```

### Pattern 4: Async Operations
```yaml
/long-operations:
  post:
    responses:
      '202':
        description: Accepted
        content:
          application/json:
            schema:
              type: object
              properties:
                operation_id:
                  type: string

/long-operations/{operationId}:
  get:
    summary: Check operation status
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

### Bearer Token / JWT
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
        authorizationUrl: https://example.com/oauth/authorize
        tokenUrl: https://example.com/oauth/token
        scopes:
          read: Read access
          write: Write access
```

## Error Response Structure

### Recommended Error Format
```yaml
components:
  schemas:
    Error:
      type: object
      required:
        - code
        - message
      properties:
        code:
          type: string
          description: Error code (e.g., INVALID_REQUEST)
        message:
          type: string
          description: Human-readable error message
        details:
          type: object
          description: Additional error details
```

### Standard HTTP Status Codes
| Code | When | Example |
|------|------|---------|
| 200 | Success | GET /users/123 |
| 201 | Created | POST /users |
| 400 | Bad request | Invalid parameters |
| 401 | Unauthorized | Missing token |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not found | Resource doesn't exist |
| 409 | Conflict | Duplicate resource |
| 429 | Rate limited | Too many requests |
| 500 | Server error | Unexpected error |

## Tools That Work with Templates

### Documentation Viewers
- **Swagger UI** - Interactive API explorer
- **ReDoc** - Beautiful API documentation
- **Stoplight** - Visual API designer

### Code Generation
- **OpenAPI Generator** - Generate SDKs in 40+ languages
- **Swagger Codegen** - Generate client libraries

### Testing
- **Postman** - API testing client
- **Insomnia** - REST client
- **Thunder Client** - VS Code extension

### Development
- **Prism** - Mock API server
- **Spectacle** - Generate HTML docs
- **OpenAPI 3.1 tools** - Latest standard tools

## Validation

### Check Your Specification
- Valid YAML/JSON syntax
- All references resolve
- Required fields present
- Examples match schemas
- Consistent terminology

### Tools to Validate
- **Swagger Editor** - Online editor with validation
- **Spectacle CLI** - Command-line validation
- **openapi-generator** - Validation included

## Next Steps

1. **Run `/api-template`**
2. **Choose your API type** (REST, GraphQL, etc.)
3. **Describe your API** (name, purpose, auth)
4. **Get your template** (complete OpenAPI spec)
5. **Customize** (add your specific endpoints)
6. **Validate** (check for errors)
7. **Generate docs** (use Swagger UI or ReDoc)
8. **Publish** (share with your users)

## Related Commands

- **`/write-docs`** - Complete API documentation guide
- **`/generate-examples`** - Create code examples for endpoints
- **`/review-docs`** - Get feedback on API design

## Tips

✅ **DO:**
- Start with your most important endpoints
- Keep descriptions clear and concise
- Include realistic examples
- Document error scenarios
- Keep spec in version control
- Update spec with code changes

❌ **DON'T:**
- Leave schemas undefined
- Forget authentication documentation
- Skip error responses
- Include sensitive examples
- Ignore schema validation
- Neglect keeping spec updated

---

**Your professional API documentation starts here!** 🚀
