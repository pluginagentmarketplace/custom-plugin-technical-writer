---
name: specialized-roles
description: Master specialized technical roles including QA, technical writing, DevRel, and software architecture. Use this skill for quality assurance, documentation, testing, and team development.
---

# Specialized Roles Skill

## QA Engineering & Testing

### Test Planning & Strategy
```
Test Strategy Hierarchy:
├── Unit Tests (70%)
│   └── Test individual functions in isolation
├── Integration Tests (20%)
│   └── Test component interactions
└── E2E Tests (10%)
    └── Test complete user workflows
```

### Test Automation with Selenium
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait

driver = webdriver.Chrome()
driver.get("https://example.com")

# Find elements
element = driver.find_element(By.ID, "id")

# Interact
element.click()
element.send_keys("text")

# Wait for element
wait = WebDriverWait(driver, 10)
wait.until(EC.presence_of_element_located((By.ID, "id")))

driver.quit()
```

### Cypress for E2E Testing
```javascript
describe('User Login', () => {
  it('should login successfully', () => {
    cy.visit('https://example.com/login');
    cy.get('input[name="email"]').type('test@example.com');
    cy.get('input[name="password"]').type('password123');
    cy.get('button[type="submit"]').click();
    cy.url().should('include', '/dashboard');
    cy.contains('Welcome').should('be.visible');
  });
});
```

### Performance Testing
```python
# JMeter-like approach
import time
import requests

def performance_test():
    start = time.time()
    response = requests.get('https://api.example.com/endpoint')
    duration = time.time() - start

    assert duration < 1.0, f"Response too slow: {duration}s"
    assert response.status_code == 200
    assert len(response.json()) > 0
```

## Technical Writing

### API Documentation (OpenAPI/Swagger)
```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0
paths:
  /api/v1/users:
    post:
      summary: Create a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                email:
                  type: string
                  format: email
                password:
                  type: string
                  minLength: 8
                name:
                  type: string
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
          description: User already exists
```

### Markdown Best Practices
```markdown
# API Reference

## Users Endpoint

### Create User
**POST** `/api/v1/users`

#### Request
\`\`\`json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "name": "John Doe"
}
\`\`\`

#### Response
\`\`\`json
{
  "id": "user-123",
  "email": "user@example.com",
  "name": "John Doe",
  "created_at": "2024-01-01T12:00:00Z"
}
\`\`\`

#### Status Codes
- 201: User created successfully
- 400: Invalid request
- 409: Email already registered

### Error Handling
Use consistent error responses:
\`\`\`json
{
  "error": {
    "code": "USER_ALREADY_EXISTS",
    "message": "Email already registered",
    "details": "Use forgot password to recover"
  }
}
\`\`\`
```

### Documentation Tools
| Tool | Best For | Features |
|------|----------|----------|
| Swagger UI | API docs | Interactive, auto-generated |
| Sphinx | Python docs | Beautiful, searchable |
| MkDocs | Project docs | Simple, fast, themable |
| GitBook | Book-like docs | Reader-friendly, versioning |
| Docusaurus | Developer portals | Modern, multi-version |

## Developer Relations (DevRel)

### Community Program Structure
```
Community Program
├── Documentation (Getting started guides)
├── Content (Blog, tutorials, videos)
├── Events (Meetups, conferences, webinars)
├── Support (Forums, Discord, direct help)
└── Feedback (Feature requests, bug reports)
```

### DevRel Metrics
```python
# Engagement metrics
engagement_metrics = {
    'stars': 15000,
    'forks': 2000,
    'contributors': 250,
    'downloads': 1000000,
    'community_members': 5000,
    'issues_resolved': 95,  # % of issues
    'response_time': 24,    # hours
    'documentation_coverage': 98  # %
}
```

### Open Source Maintenance Checklist
- [ ] Clear README with quick start
- [ ] Contributing guidelines
- [ ] Code of conduct
- [ ] Issue templates
- [ ] Pull request template
- [ ] Security policy
- [ ] Changelog/release notes
- [ ] Active issue management
- [ ] Regular releases
- [ ] Community engagement

## Software Architecture

### Design Patterns
```
Creational Patterns:
├── Singleton (one instance)
├── Factory (create objects)
└── Builder (complex object construction)

Structural Patterns:
├── Adapter (incompatible interfaces)
├── Decorator (add behavior dynamically)
└── Facade (simplified interface)

Behavioral Patterns:
├── Observer (notify changes)
├── Strategy (algorithm selection)
└── Command (encapsulate requests)
```

### Architectural Patterns
```
├── Monolithic
│   └── Single deployable unit
├── Microservices
│   └── Independent services
├── Serverless
│   └── Function-as-a-Service
└── Event-Driven
    └── Event-based communication
```

### SOLID Principles

**Single Responsibility Principle (SRP)**
```python
# ❌ Bad: Multiple responsibilities
class UserManager:
    def create_user(self, user_data):
        user = User(user_data)
        db.save(user)
        send_email(user.email)
        log_activity(user)

# ✅ Good: Single responsibility
class UserRepository:
    def save(self, user):
        db.save(user)

class EmailService:
    def send_welcome_email(self, email):
        send_email(email)

class ActivityLogger:
    def log_user_creation(self, user):
        log_activity(user)
```

**Open/Closed Principle (OCP)**
```python
# ✅ Good: Open for extension, closed for modification
class PaymentProcessor:
    def process(self, payment, processor):
        return processor.process(payment)

class CreditCardProcessor:
    def process(self, payment):
        # Process credit card

class PayPalProcessor:
    def process(self, payment):
        # Process PayPal
```

## Product Management

### Feature Prioritization
```
Feature Evaluation Matrix:
┌──────────┬──────────────┐
│          │ High Impact  │
├──────────┼──────────────┤
│ Low Effort│ Do First     │
│ High Effort│ Plan Later  │
└──────────┴──────────────┘
```

### Roadmap Planning
- 60% Committed (customer requests, bugs)
- 30% Planned (strategic features)
- 10% Exploration (innovation, experiments)

## Best Practices

✅ **QA:**
- Automate repetitive tests
- Test edge cases and error paths
- Keep test code maintainable
- Document test scenarios
- Use descriptive assertion messages

✅ **Technical Writing:**
- Write for your audience's level
- Use consistent terminology
- Include code examples
- Keep docs updated
- Add diagrams and visuals

✅ **DevRel:**
- Engage with community genuinely
- Share knowledge openly
- Respond to questions quickly
- Celebrate community members
- Collect and act on feedback

✅ **Architecture:**
- Design for scalability
- Keep it simple (KISS)
- Document decisions
- Think about maintenance
- Consider trade-offs

## Resources

- Software Testing: TestNG, JUnit, Pytest
- Technical Writing: Google Tech Writing, Write the Docs
- DevRel: DevRel Collective, Ambassador programs
- Architecture: O'Reilly, Enterprise Integration Patterns

## Next Steps
1. Master testing frameworks for your language
2. Contribute technical documentation
3. Start a dev community initiative
4. Study architecture patterns deeply
5. Build complete project from design to deployment
