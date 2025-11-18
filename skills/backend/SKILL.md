---
name: backend-development
description: Build scalable backend systems with Node.js, Spring Boot, or ASP.NET Core. Use this skill for API development, database design, authentication, and server architecture.
---

# Backend Development Skill

## Quick Start

### Core Backend Concepts
1. **HTTP & REST APIs**
   ```javascript
   // Node.js/Express example
   app.get('/api/users/:id', async (req, res) => {
     const user = await User.findById(req.params.id);
     res.json(user);
   });
   ```

2. **Request/Response Cycle**
   - HTTP methods (GET, POST, PUT, DELETE)
   - Status codes and error handling
   - Headers and content negotiation
   - Request validation

3. **Database Fundamentals**
   - SQL vs NoSQL tradeoffs
   - Schema design and normalization
   - Indexes and query optimization
   - Transaction management

4. **Authentication & Security**
   - JWT tokens and sessions
   - OAuth 2.0 flows
   - Password hashing (bcrypt, Argon2)
   - Rate limiting and CORS

## Architecture Patterns

### Monolithic Architecture
```
Client → Load Balancer → API Server → Database
```
- Simpler deployment
- Easier debugging
- Shared database and resources
- Scaling entire application only

### Microservices Architecture
```
Client → API Gateway → [Service 1, Service 2, Service 3]
                       ↓        ↓         ↓
                      DB1      DB2       DB3
```
- Independent scaling
- Technology flexibility
- Operational complexity
- Data consistency challenges

## Framework Comparison

| Framework | Language | Best For | Maturity |
|-----------|----------|----------|----------|
| Express | Node.js | Lightweight APIs | Very stable |
| Spring Boot | Java | Enterprise apps | Production ready |
| ASP.NET Core | C# | Windows/Azure | Modern & fast |
| Django | Python | Rapid development | Stable |
| FastAPI | Python | Modern async APIs | Growing |

## Database Design

### SQL Example (Normalization)
```sql
-- Users table
CREATE TABLE users (
  id INT PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Posts table
CREATE TABLE posts (
  id INT PRIMARY KEY,
  user_id INT FOREIGN KEY REFERENCES users(id),
  title VARCHAR(255) NOT NULL,
  content TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Common Patterns
- Normalization (3NF)
- Denormalization for read performance
- Indexing strategies
- Query optimization
- Connection pooling

## API Design Best Practices

### RESTful Endpoints
```
GET    /api/v1/users          # List all users
POST   /api/v1/users          # Create new user
GET    /api/v1/users/{id}     # Get specific user
PUT    /api/v1/users/{id}     # Update user
DELETE /api/v1/users/{id}     # Delete user
```

### Error Handling
```javascript
{
  "status": 400,
  "error": "Bad Request",
  "message": "Email is required",
  "timestamp": "2024-01-01T12:00:00Z"
}
```

### Versioning
- URL path: `/api/v1/resource`
- Header: `Accept: application/vnd.company.v1+json`
- Parameter: `?version=1`

## Common Backend Challenges

### Scalability
- Database bottlenecks
- Server resource limits
- State management across instances
- **Solutions**: Read replicas, caching, load balancing

### Data Consistency
- Race conditions
- Distributed transactions
- Cache invalidation
- **Solutions**: Transactions, locks, event sourcing

### Security Vulnerabilities
- SQL Injection
- XSS attacks
- CSRF protection
- **Solutions**: Prepared statements, input validation, CORS

## Caching Strategies

### In-Memory Cache (Redis)
```javascript
// Cache user profile
const cacheKey = `user:${userId}`;
const cached = await redis.get(cacheKey);

if (!cached) {
  const user = await db.User.findById(userId);
  await redis.setex(cacheKey, 3600, JSON.stringify(user)); // 1 hour TTL
  return user;
}
return JSON.parse(cached);
```

### Cache Patterns
- Cache-aside (Lazy loading)
- Write-through caching
- Write-behind caching
- Cache invalidation strategies

## Testing Backend Services

### Unit Testing
- Test business logic in isolation
- Mock external dependencies
- 70-80% code coverage target

### Integration Testing
```javascript
describe('User API', () => {
  it('creates a new user', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com' });

    expect(response.status).toBe(201);
    expect(response.body.id).toBeDefined();
  });
});
```

### E2E Testing
- Test complete workflows
- Use test databases
- Clean up after tests

## Production Readiness

### Monitoring & Logging
- Structured logging (JSON format)
- Error tracking (Sentry, LogRocket)
- Performance monitoring (APM)
- Health check endpoints

### Deployment Considerations
- Environment configuration
- Database migrations
- Blue-green deployments
- Rollback strategies

### Performance Optimization
- Query optimization
- N+1 query prevention
- Connection pooling
- Database indexing

## Best Practices

✅ **DO:**
- Validate all inputs
- Use prepared statements
- Implement proper error handling
- Version your APIs
- Document endpoints
- Use HTTPS always
- Implement logging
- Rate limit endpoints
- Use environment variables

❌ **DON'T:**
- Store passwords in plain text
- Trust client input
- Expose sensitive information in errors
- Block on I/O operations
- Hardcode configuration
- Ignore security warnings
- Skip testing
- Use SELECT * in production

## Resources

- Official Docs: Express, Spring Boot, ASP.NET Core
- "Building Microservices" - Sam Newman
- REST API Best Practices
- Database design resources

## Next Steps
1. Master your chosen framework
2. Build a real API with database
3. Implement authentication
4. Add comprehensive testing
5. Deploy to production with monitoring
