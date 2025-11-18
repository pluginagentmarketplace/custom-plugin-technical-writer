---
name: programming-languages
description: Master multiple programming languages including Python, JavaScript, Go, Rust, Java, and TypeScript. Use this skill when learning language fundamentals, syntax, or ecosystem-specific patterns.
---

# Programming Languages Skill

## Quick Start: Multi-Language Foundation

### JavaScript Fundamentals
```javascript
// ES6+ features
const greet = (name) => `Hello, ${name}!`;

// Destructuring
const { name, age } = { name: 'John', age: 30 };

// Async/await
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
  }
}

// Promises
Promise.all([fetch1, fetch2, fetch3])
  .then(results => results.map(r => r.json()))
  .catch(error => console.error(error));
```

### Python Fundamentals
```python
# List comprehension
numbers = [x**2 for x in range(10) if x % 2 == 0]

# Decorators
def timing_decorator(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f'Took {time.time() - start}s')
        return result
    return wrapper

# Async/await
async def fetch_data():
    async with aiohttp.ClientSession() as session:
        async with session.get('http://api.example.com') as response:
            return await response.json()

# Type hints
def add(a: int, b: int) -> int:
    return a + b
```

### Go Fundamentals
```go
// Goroutines and channels
func main() {
    results := make(chan string)

    go func() {
        results <- fetchData()
    }()

    result := <-results
    fmt.Println(result)
}

// Error handling
result, err := someFunction()
if err != nil {
    log.Fatal(err)
}

// Interfaces
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

### Rust Fundamentals
```rust
// Ownership system
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // s1 is no longer valid
    // println!("{}", s1); // ERROR: borrow of moved value

    let s3 = String::from("hello");
    let s4 = &s3; // borrow, s3 still valid
    println!("{}", s3); // OK
}

// Pattern matching
match Some(5) {
    Some(5) => println!("five"),
    Some(x) => println!("other: {}", x),
    None => println!("none"),
}

// Result type
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err("Division by zero".to_string())
    } else {
        Ok(a / b)
    }
}
```

## Language Comparison Matrix

| Feature | Python | JavaScript | Go | Rust | Java |
|---------|--------|------------|-----|------|------|
| Type System | Dynamic | Dynamic | Static | Static | Static |
| Memory Mgmt | GC | GC | GC | Manual | GC |
| Concurrency | Threading/Async | Async | Goroutines | Ownership | Threads |
| Performance | Moderate | Good | Excellent | Excellent | Good |
| Learning Curve | Easy | Easy | Medium | Hard | Medium |

## Object-Oriented Programming

### Classes and Inheritance
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError

class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof!"

# SOLID principles:
# S: Single Responsibility
# O: Open/Closed
# L: Liskov Substitution
# I: Interface Segregation
# D: Dependency Inversion
```

## Functional Programming

### Higher-Order Functions
```python
# Map
numbers = [1, 2, 3, 4]
squared = list(map(lambda x: x**2, numbers))

# Filter
evens = list(filter(lambda x: x % 2 == 0, numbers))

# Reduce
from functools import reduce
product = reduce(lambda x, y: x * y, numbers)

# Composition
compose = lambda f, g: lambda x: f(g(x))
```

## Async/Concurrent Programming

### JavaScript Promises & Async
```javascript
// Promise chain
fetch('/api/user/1')
  .then(res => res.json())
  .then(user => fetch(`/api/posts/${user.id}`))
  .then(res => res.json())
  .then(posts => console.log(posts))
  .catch(error => console.error(error));

// Async/await (cleaner)
async function getUserPosts() {
  const userRes = await fetch('/api/user/1');
  const user = await userRes.json();
  const postsRes = await fetch(`/api/posts/${user.id}`);
  const posts = await postsRes.json();
  return posts;
}
```

### Go Goroutines
```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Println("worker", id, "started job", j)
        time.Sleep(time.Second)
        results <- j * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)

    for a := 1; a <= 9; a++ {
        <-results
    }
}
```

## Type Systems

### TypeScript Type Safety
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age?: number; // optional
}

type Role = 'admin' | 'user' | 'guest';

class UserService {
  getUser(id: number): Promise<User> {
    // implementation
  }

  updateUser(id: number, user: Partial<User>): Promise<User> {
    // implementation
  }
}
```

## Package Management

| Language | Package Manager | Central Registry |
|----------|---|---|
| Python | pip | PyPI |
| JavaScript | npm, yarn, pnpm | npmjs.com |
| Go | go get | pkg.go.dev |
| Rust | cargo | crates.io |
| Java | Maven, Gradle | Maven Central |

## Testing in Different Languages

### Python pytest
```python
def test_addition():
    assert add(2, 3) == 5

def test_user_creation():
    user = User(name="John")
    assert user.name == "John"
```

### JavaScript Jest
```javascript
test('addition', () => {
  expect(add(2, 3)).toBe(5);
});

test('user creation', () => {
  const user = new User({name: 'John'});
  expect(user.name).toBe('John');
});
```

## Common Pitfalls & Solutions

| Pitfall | Language | Solution |
|---------|----------|----------|
| Global state | All | Use functions/classes |
| Mutable default args | Python | Use None + lazy init |
| Type confusion | JavaScript | Use TypeScript |
| Inefficient loops | Python | Use list comprehensions |
| Memory leaks | JavaScript | Understand closures |
| Deadlocks | Go | Use channels carefully |
| Borrow checker | Rust | Understand ownership |

## Best Practices

✅ **DO:**
- Follow language conventions
- Use type hints when available
- Write testable code
- Document complex logic
- Use linters and formatters
- Keep functions small
- Name variables clearly
- Handle errors properly
- Use standard library first

❌ **DON'T:**
- Mix paradigms carelessly
- Ignore warnings
- Write overly complex code
- Neglect performance
- Use global state
- Copy-paste code
- Skip error handling
- Ignore community conventions

## Resources

- Python: python.org, Real Python
- JavaScript: javascript.info, MDN
- Go: golang.org, golangbyexample.com
- Rust: rust-lang.org, rustbyexample.com
- Java: oracle.com, baeldung.com

## Next Steps
1. Master one language deeply
2. Learn a second language (different paradigm)
3. Understand concurrency in your languages
4. Study type systems and safety
5. Contribute to open source
