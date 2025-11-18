---
name: foundations-architecture
description: Build strong foundations in computer science, data structures, algorithms, and system design. Use this skill for learning fundamentals, complexity analysis, and architectural patterns.
---

# Foundations & Architecture Skill

## Computer Science Fundamentals

### Big O Complexity Analysis
```
Time Complexity (best to worst):
O(1) - Constant
O(log n) - Logarithmic
O(n) - Linear
O(n log n) - Linearithmic
O(n²) - Quadratic
O(n³) - Cubic
O(2ⁿ) - Exponential
O(n!) - Factorial

Space Complexity follows similar hierarchy
```

### Examples
```python
# O(1) - Constant time
def get_first(arr):
    return arr[0]

# O(log n) - Binary search
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# O(n) - Linear search
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1

# O(n²) - Nested loops (Bubble sort)
def bubble_sort(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - 1 - i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr
```

## Essential Data Structures

### Arrays & Lists
```python
# Static array (fixed size)
arr = [1, 2, 3, 4, 5]

# Dynamic list
lst = []
lst.append(1)          # O(1) amortized
lst.insert(0, 10)      # O(n)
lst.pop()              # O(1)
lst.remove(2)          # O(n)
```

### Linked Lists
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):          # O(n)
        if not self.head:
            self.head = Node(data)
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = Node(data)

    def insert(self, index, data):   # O(n)
        if index == 0:
            new_node = Node(data)
            new_node.next = self.head
            self.head = new_node
            return
        current = self.head
        for _ in range(index - 1):
            current = current.next
        new_node = Node(data)
        new_node.next = current.next
        current.next = new_node

    def delete(self, index):         # O(n)
        if index == 0:
            self.head = self.head.next
            return
        current = self.head
        for _ in range(index - 1):
            current = current.next
        current.next = current.next.next
```

### Stacks & Queues
```python
# Stack (LIFO)
class Stack:
    def __init__(self):
        self.items = []

    def push(self, item):    # O(1)
        self.items.append(item)

    def pop(self):           # O(1)
        return self.items.pop()

    def peek(self):          # O(1)
        return self.items[-1]

# Queue (FIFO)
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque()

    def enqueue(self, item):  # O(1)
        self.items.append(item)

    def dequeue(self):        # O(1)
        return self.items.popleft()
```

### Hash Tables/Dictionaries
```python
# Hash map basics
user_map = {}
user_map['user1'] = {'name': 'John', 'age': 30}

# Operations
user_map['user1']          # O(1) lookup
user_map['user2'] = {...}  # O(1) insert
del user_map['user1']      # O(1) delete

# Collision handling
class HashTable:
    def __init__(self, size=100):
        self.size = size
        self.table = [[] for _ in range(size)]  # Chaining

    def hash_function(self, key):
        return hash(key) % self.size

    def set(self, key, value):
        index = self.hash_function(key)
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                self.table[index][i] = (key, value)
                return
        self.table[index].append((key, value))

    def get(self, key):
        index = self.hash_function(key)
        for k, v in self.table[index]:
            if k == key:
                return v
        return None
```

### Trees & Graphs
```python
# Binary Tree
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# Binary Search Tree
class BST:
    def __init__(self):
        self.root = None

    def insert(self, value):        # O(log n) avg, O(n) worst
        if not self.root:
            self.root = TreeNode(value)
        else:
            self._insert_recursive(self.root, value)

    def _insert_recursive(self, node, value):
        if value < node.value:
            if node.left is None:
                node.left = TreeNode(value)
            else:
                self._insert_recursive(node.left, value)
        else:
            if node.right is None:
                node.right = TreeNode(value)
            else:
                self._insert_recursive(node.right, value)

    def search(self, value):        # O(log n) avg, O(n) worst
        return self._search_recursive(self.root, value)

    def _search_recursive(self, node, value):
        if node is None:
            return False
        if value == node.value:
            return True
        elif value < node.value:
            return self._search_recursive(node.left, value)
        else:
            return self._search_recursive(node.right, value)

# Graph representations
# Adjacency List
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'E'],
    'D': ['B'],
    'E': ['C']
}

# DFS (Depth-First Search)
def dfs(graph, node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
    return visited

# BFS (Breadth-First Search)
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)

    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return visited
```

## Classic Algorithms

### Sorting
```python
# Bubble Sort - O(n²)
def bubble_sort(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - 1 - i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

# Quick Sort - O(n log n) avg, O(n²) worst
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)

# Merge Sort - O(n log n) always
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    while left and right:
        if left[0] <= right[0]:
            result.append(left.pop(0))
        else:
            result.append(right.pop(0))
    return result + left + right
```

## System Design

### Scalability Principles
```
Vertical Scaling (Scale Up)
└─ Add more CPU/RAM to single server
   Pros: Simple, fast
   Cons: Limited, expensive

Horizontal Scaling (Scale Out)
└─ Add more servers
   Pros: Unlimited, cost-effective
   Cons: Complex, requires distributed systems
```

### Load Balancing
```
                    Clients
                       ↓
                 Load Balancer
                 /    |    \
            Server1  Server2  Server3
                     ↓
                 Database
                     ↓
                  Cache
```

### CAP Theorem
```
Distributed System can guarantee at most 2 of 3:
├── Consistency (all nodes see same data)
├── Availability (system always responsive)
└── Partition Tolerance (handles network failures)

Trade-offs:
├── CP Systems: Strong consistency, tolerates partitions
│   Example: HBase, MongoDB
├── AP Systems: Always available, tolerates partitions
│   Example: DynamoDB, Cassandra
└── CA Systems: Consistent and available (no partition tolerance)
    Example: Traditional RDBMS (single server)
```

### Database Design

#### Normalization (1NF, 2NF, 3NF)
```sql
-- 1NF: Atomic values
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(255),
    phone VARCHAR(20)  -- Not multi-valued
);

-- 2NF: No partial dependencies
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    user_id INT FOREIGN KEY,
    order_date DATE
);

-- 3NF: No transitive dependencies
CREATE TABLE order_items (
    item_id INT PRIMARY KEY,
    order_id INT FOREIGN KEY,
    product_id INT FOREIGN KEY,
    quantity INT
);
```

#### Denormalization (Read Optimization)
```sql
-- Denormalized view for fast reads
CREATE TABLE order_summary AS
SELECT o.order_id, u.user_email, COUNT(oi.item_id) as item_count
FROM orders o
JOIN users u ON o.user_id = u.id
LEFT JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id;
```

### Caching Strategies

| Strategy | When | Trade-offs |
|----------|------|-----------|
| Cache-Aside | Most common | App manages consistency |
| Write-Through | Important data | Slower writes |
| Write-Behind | High throughput | Consistency risk |
| Refresh-Ahead | Predictable access | Complexity |

## Security Fundamentals

### Cryptography Basics
```
Symmetric Encryption (AES)
├── Same key for encryption/decryption
├── Fast
└── Problem: Key distribution

Asymmetric Encryption (RSA)
├── Public key for encryption
├── Private key for decryption
├── Solves key distribution
└── Slower than symmetric

Hashing (SHA-256)
├── One-way function
├── Deterministic
├── Used for passwords
└── Salted: password + random salt
```

### Common Vulnerabilities (OWASP Top 10)

1. **Injection** - Use parameterized queries
2. **Broken Authentication** - Strong password policies, MFA
3. **Sensitive Data Exposure** - Encrypt in transit & at rest
4. **XML External Entities** - Disable external entities
5. **Broken Access Control** - Implement proper authorization
6. **Security Misconfiguration** - Follow security baselines
7. **XSS** - Validate/sanitize user input
8. **Insecure Deserialization** - Avoid untrusted data
9. **Using Components with Vulnerabilities** - Keep deps updated
10. **Insufficient Logging** - Monitor and alert

## Best Practices

✅ **DO:**
- Understand trade-offs deeply
- Choose appropriate data structures
- Analyze algorithm complexity
- Design for scalability
- Plan for failures
- Document architectural decisions
- Monitor and measure
- Start simple, optimize based on data

❌ **DON'T:**
- Optimize prematurely
- Ignore edge cases
- Design for hypothetical scale
- Forget about maintenance
- Skip security considerations
- Ignore monitoring

## Resources

- "Cracking the Coding Interview" - McDowell
- "Designing Data-Intensive Applications" - Kleppmann
- LeetCode, HackerRank (practice)
- System Design Primer (GitHub)
- OWASP Top 10

## Next Steps
1. Master data structures and algorithms
2. Practice with coding problems
3. Study system design patterns
4. Build distributed systems
5. Learn security best practices deeply
