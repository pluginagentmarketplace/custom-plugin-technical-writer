---
name: api-documentation
description: Master API documentation creation including OpenAPI/Swagger specifications, REST endpoint documentation, authentication flows, and error handling guides. Use this skill when documenting APIs, creating endpoint specifications, or writing API reference guides.
---

# API Documentation Skill

## Quick Start

### OpenAPI/Swagger Specification

```yaml
openapi: 3.0.0
info:
  title: Example API
  version: 1.0.0
  description: A professional API with clear documentation

servers:
  - url: https://api.example.com/v1
    description: Production server

paths:
  /users:
    get:
      summary: List all users
      description: Retrieve a paginated list of all users
      tags:
        - Users
      parameters:
        - name: page
          in: query
          description: Page number (starts at 1)
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          description: Items per page (1-100)
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Successful response with user list
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'
                  pagination:
                    $ref: '#/components/schemas/Pagination'
        '400':
          description: Invalid query parameters
        '401':
          description: Unauthorized - missing or invalid token
        '500':
          description: Server error

    post:
      summary: Create a new user
      description: Create a new user account
      tags:
        - Users
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Invalid input
        '409':
          description: Email already exists

components:
  schemas:
    User:
      type: object
      required:
        - id
        - email
        - name
      properties:
        id:
          type: string
          format: uuid
          description: Unique user identifier
        email:
          type: string
          format: email
          description: User email address
        name:
          type: string
          description: User full name
        created_at:
          type: string
          format: date-time
          description: User creation timestamp

    CreateUserRequest:
      type: object
      required:
        - email
        - name
        - password
      properties:
        email:
          type: string
          format: email
        name:
          type: string
        password:
          type: string
          minLength: 8

    Pagination:
      type: object
      properties:
        page:
          type: integer
        limit:
          type: integer
        total:
          type: integer
        pages:
          type: integer

  securitySchemes:
    BearerToken:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: JWT token obtained from /auth/login endpoint
```

## REST API Endpoint Documentation

### Structure

Each endpoint should include:

```markdown
## GET /api/v1/users/{id}

Retrieve a specific user by ID

### Parameters

| Name | Type | Location | Required | Description |
|------|------|----------|----------|-------------|
| id | string | path | Yes | User ID (UUID format) |
| include | string | query | No | Related data to include (comma-separated) |

### Authentication
- Bearer Token (JWT)

### Request Example
\`\`\`bash
curl -X GET https://api.example.com/v1/users/550e8400 \
  -H "Authorization: Bearer YOUR_TOKEN"
\`\`\`

### Response (200 OK)
\`\`\`json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "user@example.com",
  "name": "John Doe",
  "created_at": "2024-01-15T10:30:00Z"
}
\`\`\`

### Error Responses

#### 404 Not Found
\`\`\`json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 550e8400 does not exist"
  }
}
\`\`\`

#### 401 Unauthorized
\`\`\`json
{
  "error": {
    "code": "INVALID_TOKEN",
    "message": "Token is missing or invalid"
  }
}
\`\`\`
```

## Authentication Documentation

### Bearer Token (JWT)

```markdown
## Authentication

All API requests require a valid JWT token in the Authorization header.

### Obtaining a Token

**POST** `/auth/login`

Request:
\`\`\`json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
\`\`\`

Response:
\`\`\`json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
\`\`\`

### Using the Token

Include the token in Authorization header:
\`\`\`bash
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
\`\`\`

### Token Expiration

Tokens expire after 1 hour. Use the refresh endpoint:

**POST** `/auth/refresh`

Request:
\`\`\`json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
\`\`\`

### Common Errors

- **401 Unauthorized**: Token missing or invalid
- **403 Forbidden**: Token expired
- **422 Unprocessable**: Invalid token format
```

## Error Handling Documentation

```markdown
## Error Responses

All errors follow a consistent format:

\`\`\`json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {
      "field": "Additional error details"
    },
    "timestamp": "2024-01-15T10:30:00Z",
    "request_id": "req_1234567890"
  }
}
\`\`\`

### Error Codes

| Code | Status | Meaning |
|------|--------|---------|
| INVALID_REQUEST | 400 | Request validation failed |
| INVALID_TOKEN | 401 | Missing or invalid authentication |
| FORBIDDEN | 403 | Insufficient permissions |
| NOT_FOUND | 404 | Resource not found |
| CONFLICT | 409 | Resource already exists |
| RATE_LIMITED | 429 | Too many requests |
| SERVER_ERROR | 500 | Unexpected server error |
| SERVICE_UNAVAILABLE | 503 | Service temporarily unavailable |

### Handling Errors

**Best Practices:**
1. Always check HTTP status code
2. Parse error code for specific handling
3. Display user-friendly message from response
4. Log request_id for debugging
5. Implement retry logic for 5xx errors
```

## Rate Limiting Documentation

```markdown
## Rate Limiting

API requests are rate-limited to prevent abuse.

### Limits

- **Free Tier**: 100 requests/hour
- **Pro Tier**: 10,000 requests/hour
- **Enterprise**: Custom limits

### Rate Limit Headers

All responses include rate limit information:

\`\`\`
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 42
X-RateLimit-Reset: 1705318200
\`\`\`

### When Rate Limited (429)

\`\`\`json
{
  "error": {
    "code": "RATE_LIMITED",
    "message": "Too many requests",
    "retry_after": 60
  }
}
\`\`\`

### Retry Strategy

\`\`\`javascript
async function makeRequestWithRetry(url, options, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    const response = await fetch(url, options);

    if (response.status === 429) {
      const retryAfter = response.headers.get('Retry-After') || 60;
      console.log(`Rate limited. Retrying in ${retryAfter}s`);
      await new Promise(r => setTimeout(r, retryAfter * 1000));
      continue;
    }

    return response;
  }
}
\`\`\`
```

## API Versioning

```markdown
## API Versioning

APIs are versioned via URL path to ensure backward compatibility.

### Version Strategy

- **Current**: `/v1/` (latest stable)
- **Beta**: `/beta/` (testing new features)
- **Deprecated**: Versions older than 2 years

### Migration Guide

When a new version is released:
1. Old version continues working for 1 year
2. Deprecation notice in response headers
3. Migration guide provided
4. Support team assists transitions

### Checking Version

Request:
\`\`\`bash
curl -X GET https://api.example.com/version
\`\`\`

Response:
\`\`\`json
{
  "api_version": "1.2.3",
  "release_date": "2024-01-15",
  "deprecated_versions": ["0.9.0"],
  "sunset_date": "2025-01-15"
}
\`\`\`
```

## Best Practices

✅ **DO:**
- Use clear, descriptive endpoint names
- Include request/response examples
- Document all error codes
- Provide authentication examples
- Explain rate limiting
- Version your API
- Document breaking changes
- Include sample code in multiple languages

❌ **DON'T:**
- Expose sensitive information in examples
- Use vague parameter descriptions
- Skip error documentation
- Omit authentication details
- Leave examples outdated
- Ignore deprecations
- Use unclear HTTP status codes

## Tools and Formats

### OpenAPI/Swagger
- Industry standard for REST APIs
- Tools: Swagger UI, Redoc, Stoplight
- Format: YAML or JSON

### AsyncAPI
- For async/event-driven APIs
- Similar to OpenAPI for message protocols
- Great for WebSockets, Kafka, AMQP

### GraphQL
- Schema-driven documentation
- Introspection provides auto-documentation
- Tools: GraphQL Playground, Apollo Studio

## Next Steps

1. **Choose format**: OpenAPI, AsyncAPI, or GraphQL
2. **Create specification**: Use YAML for readability
3. **Add examples**: Provide real usage scenarios
4. **Test documentation**: Verify examples work
5. **Publish**: Use Swagger UI, Redoc, or similar
6. **Maintain**: Keep in sync with code changes
