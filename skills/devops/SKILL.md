---
name: devops-operations
description: Master DevOps practices with Docker, Kubernetes, and Terraform. Use this skill for containerization, orchestration, CI/CD pipelines, and infrastructure automation.
---

# DevOps & Cloud Operations Skill

## Quick Start

### Docker Fundamentals
```dockerfile
# Dockerfile example
FROM node:18-alpine

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .

EXPOSE 3000
CMD ["node", "server.js"]
```

### Building and Running
```bash
docker build -t myapp:1.0 .
docker run -p 3000:3000 myapp:1.0
docker tag myapp:1.0 registry.example.com/myapp:1.0
docker push registry.example.com/myapp:1.0
```

### Docker Best Practices
- Use specific base image tags (not `latest`)
- Minimize layer count
- Multi-stage builds for smaller images
- Non-root user for security
- Health checks for containers

## Kubernetes Essentials

### Pod Deployment
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: myapp
    image: myapp:1.0
    ports:
    - containerPort: 3000
    livenessProbe:
      httpGet:
        path: /health
        port: 3000
      initialDelaySeconds: 10
      periodSeconds: 10
```

### Service & Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:1.0
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 500m
            memory: 512Mi
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer
```

## Infrastructure as Code with Terraform

### AWS Infrastructure Example
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true

  tags = {
    Name = "main-vpc"
  }
}

resource "aws_security_group" "allow_http" {
  name = "allow_http"
  vpc_id = aws_vpc.main.id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  security_groups = [aws_security_group.allow_http.id]

  tags = {
    Name = "web-server"
  }
}

output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

## CI/CD Pipelines

### GitHub Actions Example
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run lint
      - run: npm test
      - run: npm run build

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build and push Docker image
        run: |
          docker build -t myapp:${{ github.sha }} .
          docker push myapp:${{ github.sha }}
      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/myapp \
            myapp=myapp:${{ github.sha }}
```

## Cloud Platforms

### AWS Key Services
| Service | Use Case | Example |
|---------|----------|---------|
| EC2 | Virtual machines | Web servers, app servers |
| RDS | Managed databases | PostgreSQL, MySQL, Oracle |
| S3 | Object storage | Static files, backups |
| Lambda | Serverless compute | Event handlers, APIs |
| CloudFront | CDN | Static content, images |
| Route53 | DNS | Domain management |

### GCP Equivalents
- Compute Engine ↔ EC2
- Cloud SQL ↔ RDS
- Cloud Storage ↔ S3
- Cloud Functions ↔ Lambda

## Monitoring & Logging

### Prometheus Metrics
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
```

### ELK Stack (Elasticsearch, Logstash, Kibana)
```json
{
  "timestamp": "2024-01-01T12:00:00Z",
  "level": "ERROR",
  "service": "api",
  "message": "Database connection failed",
  "context": {
    "user_id": "123",
    "endpoint": "/api/users"
  }
}
```

## Common Challenges

### Container Issues
- Image size optimization
- Docker networking
- Volume management
- **Solutions**: Multi-stage builds, Docker Compose, named volumes

### Kubernetes Complexity
- Pod scheduling
- Service mesh complexity
- Helm chart management
- **Solutions**: Use managed Kubernetes (EKS, GKE, AKS)

### Infrastructure Drift
- Manual changes causing differences
- Terraform state issues
- **Solutions**: Version control, automation, state locking

## Best Practices

✅ **DO:**
- Use infrastructure as code
- Implement proper secret management
- Automate everything
- Monitor all systems
- Use health checks
- Plan for disasters
- Version all artifacts
- Implement least privilege
- Use resource limits
- Regular backups

❌ **DON'T:**
- Manual infrastructure changes
- Hardcode secrets
- Ignore monitoring
- Deploy without testing
- Use default credentials
- Skip documentation
- Forget disaster recovery
- Mix production and development
- Ignore cost optimization

## Tools Ecosystem

### Containerization
- Docker, Podman, Containerd

### Orchestration
- Kubernetes, Docker Swarm, ECS

### Infrastructure as Code
- Terraform, CloudFormation, Helm

### CI/CD
- GitHub Actions, GitLab CI, Jenkins

### Monitoring
- Prometheus, Grafana, Datadog, New Relic

## Resources

- Kubernetes official docs
- Terraform registry
- Docker Hub
- AWS documentation
- "The Phoenix Project" - DevOps culture

## Next Steps
1. Master Docker and containerization
2. Learn Kubernetes basics
3. Practice with Terraform
4. Build a complete CI/CD pipeline
5. Implement monitoring and logging
