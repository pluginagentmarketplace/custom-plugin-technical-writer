---
name: code-examples
description: Generate clear, well-documented code examples, implementation patterns, configuration files, and integration samples. Use this skill when creating code snippets, implementation guides, or showing how to use code.
---

# Code Examples Skill

## Quick Start

### Anatomy of a Good Code Example

```markdown
## Example: [What This Shows]

**Use case:** When you want to [practical use case]

**Prerequisites:** [What's needed before running this]

\`\`\`python
# Language-specific example
# Clear, working code
import library

def do_something():
    # Helpful comments explaining key parts
    result = perform_action()
    return result
\`\`\`

**Explanation:**
- Line 1-2: Why we import this
- Line 4-6: What this function does
- Return value: What to expect

**Expected output:**
\`\`\`
Result of running the code
\`\`\`

**Common variations:**
- How to modify for different needs
- Alternative approaches

**Next steps:**
- Link to more advanced example
- Related functionality
```

## Code Example Patterns

### Pattern 1: Hello World / Getting Started

```markdown
## Getting Started: [Language/Framework]

### Installation

\`\`\`bash
# Install the library
npm install library-name
# or
pip install library-name
\`\`\`

### Your First Program

\`\`\`javascript
// Import the library
const library = require('library-name');

// Create a simple example
const result = library.doSomething();

// Display the result
console.log(result);
\`\`\`

To run:
\`\`\`bash
node example.js
\`\`\`

**Expected output:**
\`\`\`
Output here
\`\`\`

### Next: [Related Feature]
See [link to next example]
```

### Pattern 2: Common Task

```markdown
## How To: [Specific Task]

### Complete Working Example

\`\`\`python
import requests
from datetime import datetime

def fetch_user_data(user_id):
    \"\"\"
    Fetch user data from API

    Args:
        user_id (str): The user's unique identifier

    Returns:
        dict: User information or None if not found
    \"\"\"
    url = f"https://api.example.com/users/{user_id}"
    headers = {
        "Authorization": "Bearer YOUR_API_KEY",
        "Content-Type": "application/json"
    }

    try:
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()  # Raise exception for bad status
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error fetching user: {e}")
        return None

# Usage
if __name__ == "__main__":
    user = fetch_user_data("user-123")
    if user:
        print(f"User: {user['name']}")
        print(f"Email: {user['email']}")
\`\`\`

**Key points:**
- Error handling with try-except
- Type hints for clarity
- Docstring explains the function
- Real-world headers and timeout

**Run it:**
\`\`\`bash
python example.py
\`\`\`

**See also:**
- [Error handling patterns]
- [Async version]
- [Batch operations]
```

### Pattern 3: Configuration Example

```markdown
## Configuration: [System/Tool]

### Setup File Example

\`\`\`yaml
# config.yml
# Example configuration for [System]

# Server settings
server:
  host: localhost
  port: 3000
  ssl_enabled: false

# Database configuration
database:
  type: postgresql
  host: db.example.com
  port: 5432
  name: myapp_db
  pool_size: 10

# Authentication
auth:
  strategy: jwt
  secret: your-secret-key-here
  expiration: 3600

# Logging
logging:
  level: info
  format: json
  output:
    - console
    - file: logs/app.log
\`\`\`

**Explanation:**
- `server`: Basic server configuration
- `database`: Connection pool settings for performance
- `auth`: Security settings
- `logging`: Diagnostic output configuration

**Environment-specific files:**
- `config.dev.yml` - Development settings
- `config.prod.yml` - Production settings
- `config.local.yml` - Local overrides (gitignored)

**Usage in code:**
\`\`\`python
import yaml

with open('config.yml') as f:
    config = yaml.safe_load(f)

db_host = config['database']['host']
\`\`\`

**Common mistakes:**
- ❌ Hardcoding secrets
- ✅ Use environment variables: `${DB_PASSWORD}`
- ❌ Non-working defaults
- ✅ Include example config with safe defaults
```

### Pattern 4: API Integration Example

```markdown
## Integration: Using [Service] API

### Complete Implementation

\`\`\`javascript
const axios = require('axios');

class PaymentProcessor {
  constructor(apiKey) {
    this.apiKey = apiKey;
    this.baseURL = 'https://api.payment.example.com/v1';
    this.client = axios.create({
      baseURL: this.baseURL,
      headers: {
        'Authorization': `Bearer ${apiKey}`,
        'Content-Type': 'application/json'
      }
    });
  }

  async createPayment(amount, currency, description) {
    try {
      const response = await this.client.post('/payments', {
        amount: amount * 100,  // Convert to cents
        currency: currency.toUpperCase(),
        description: description,
        metadata: {
          timestamp: new Date().toISOString()
        }
      });
      return response.data;
    } catch (error) {
      if (error.response?.status === 409) {
        throw new Error('Duplicate payment detected');
      }
      throw new Error(`Payment failed: ${error.message}`);
    }
  }

  async getPaymentStatus(paymentId) {
    const response = await this.client.get(`/payments/${paymentId}`);
    return response.data.status;
  }

  async refundPayment(paymentId, amount) {
    const response = await this.client.post(
      `/payments/${paymentId}/refunds`,
      { amount: amount * 100 }
    );
    return response.data;
  }
}

// Usage example
async function main() {
  const processor = new PaymentProcessor(process.env.PAYMENT_API_KEY);

  try {
    // Create payment
    const payment = await processor.createPayment(99.99, 'USD', 'Order #123');
    console.log('Payment created:', payment.id);

    // Check status
    const status = await processor.getPaymentStatus(payment.id);
    console.log('Payment status:', status);
  } catch (error) {
    console.error('Error:', error.message);
  }
}

main();
\`\`\`

**Features shown:**
- ✅ Proper error handling
- ✅ Axios client configuration
- ✅ Error code specific handling
- ✅ Type-safe operations
- ✅ Environment variable usage

**Testing this:**
\`\`\`javascript
// test.js
const processor = new PaymentProcessor('test_key_123');

// Mock the axios client for testing
processor.client.post = jest.fn().mockResolvedValue({
  data: { id: 'pay_123', status: 'completed' }
});

processor.createPayment(50, 'USD', 'Test').then(p => {
  console.log('Test passed:', p.id === 'pay_123');
});
\`\`\`

**Error scenarios to handle:**
- Network timeout → Retry with backoff
- Rate limiting (429) → Wait before retry
- Invalid input (400) → Validate before sending
- Server error (500) → Log and alert
```

### Pattern 5: Error Handling Example

```markdown
## Handling Errors Properly

### Problem: Bare Try-Catch

\`\`\`python
# ❌ Bad: Generic error handling
try:
    result = do_something()
except:
    print("Error occurred")
\`\`\`

### Solution: Specific Error Handling

\`\`\`python
# ✅ Good: Handles different errors appropriately
try:
    result = do_something()
except ValueError as e:
    # Validation error - user input is wrong
    logger.warning(f"Validation failed: {e}")
    return {"error": "Invalid input", "details": str(e)}
except TimeoutError as e:
    # Network timeout - retry might help
    logger.error(f"Request timeout: {e}")
    return retry_operation()
except Exception as e:
    # Unexpected error - log for investigation
    logger.error(f"Unexpected error: {e}", exc_info=True)
    return {"error": "Internal server error"}
\`\`\`

**Pattern explanation:**
1. Catch specific exceptions first
2. Log appropriately (warning/error)
3. Return user-friendly messages
4. Include details for developers
5. Log stack trace for unexpected errors

### Best Practices

✅ **DO:**
- Catch specific exceptions
- Log with appropriate level
- Provide context in error messages
- Use error codes for programmatic handling
- Log stack trace for unexpected errors

❌ **DON'T:**
- Use bare except clauses
- Swallow exceptions silently
- Return generic "Error" messages
- Log sensitive data
- Let exceptions crash the program
```

### Pattern 6: Multi-Language Example

```markdown
## Implementing [Feature] in Different Languages

### JavaScript

\`\`\`javascript
const calculateTotal = (items) => {
  return items.reduce((sum, item) => sum + item.price, 0);
};

const items = [{price: 10}, {price: 20}];
console.log(calculateTotal(items));  // 30
\`\`\`

### Python

\`\`\`python
def calculate_total(items):
    return sum(item['price'] for item in items)

items = [{'price': 10}, {'price': 20}]
print(calculate_total(items))  # 30
\`\`\`

### Go

\`\`\`go
func CalculateTotal(items []Item) int {
    total := 0
    for _, item := range items {
        total += item.Price
    }
    return total
}
\`\`\`

### Java

\`\`\`java
public static int calculateTotal(List<Item> items) {
    return items.stream()
               .mapToInt(item -> item.getPrice())
               .sum();
}
\`\`\`

**Language comparison:**
- JavaScript: Functional style with reduce
- Python: Generator expression for simplicity
- Go: Explicit loop for clarity and performance
- Java: Stream API for functional approach
```

## Best Practices for Code Examples

### 1. Make Examples Runnable

\`\`\`
✅ Include all imports and setup
✅ Show complete working code
✅ Provide sample data if needed
✅ Include expected output
✅ Tested and verified to work
\`\`\`

### 2. Clear Comments

```python
# ❌ Bad: Obvious comments
x = x + 1  # Increment x

# ✅ Good: Why not what
balance = calculate_monthly_fee(balance)  # Add service fee each month
```

### 3. Progressive Complexity

```
1. Simple "hello world" first
2. Basic usage next
3. Real-world patterns
4. Advanced scenarios
5. Edge cases and errors
```

### 4. Error Handling

```python
# ✅ Always include error handling
try:
    result = process_data()
except ProcessingError as e:
    handle_error(e)
```

### 5. Consistent Styling

- Follow language conventions
- Use language-specific formatting
- Be consistent across examples
- Include necessary imports

## Creating Code Example Documentation

### Example Documentation Structure

```markdown
# Code Examples for [Feature]

## Basic Usage
[Simple example]

## Common Patterns
- Pattern 1
- Pattern 2
- Pattern 3

## Advanced Usage
[Complex example]

## Error Handling
[How to handle errors]

## Performance Tips
[Optimization advice]

## Common Mistakes
[What to avoid]

## Full Working Project
[Link to complete example repo]
```

## Tools for Code Examples

### Documentation
- Markdown with syntax highlighting
- GitHub Gists for sharing
- GitBook, Docusaurus for organized docs

### Hosting
- GitHub repositories (best!)
- GitLab snippets
- Repl.it or CodeSandbox for interactive
- Glitch for live demos

### Testing Examples
- Unit tests for correctness
- CI/CD to prevent outdated examples
- Automated checks in PRs

## Next Steps

1. **Write first example** - Simple, complete, runnable
2. **Add progressive examples** - Build complexity gradually
3. **Include error handling** - Show real-world usage
4. **Test everything** - Ensure code actually works
5. **Keep updated** - Update when libraries change
6. **Organize** - Group related examples
