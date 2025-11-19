# /write-docs - Documentation Project Assistant

## Purpose
Start a new documentation project with guided assistance. Choose your documentation type and get structured templates and best practices for writing clear, professional technical documentation.

## What This Command Does

This command helps you:
1. Choose documentation type
2. Define your target audience
3. Get a structured outline
4. Receive best practices guidance
5. Start writing immediately

## Documentation Types

### 1. **API Documentation**
For documenting REST, GraphQL, or other APIs

**Best for:**
- Developers integrating with your API
- SDK users
- Microservices documentation

**What you'll get:**
- OpenAPI/Swagger template
- Endpoint documentation template
- Authentication guide template
- Error handling guide template
- Rate limiting documentation
- Examples in multiple languages

**Estimated length:** 30-50 pages
**Time to complete:** 2-4 weeks

**Example structure:**
```
- Overview & Getting Started
- Authentication
- Base URL & Versioning
- Endpoints (organized by resource)
- Error Handling
- Rate Limiting
- Webhooks
- Code Examples (multiple languages)
- Troubleshooting
```

---

### 2. **User Guide**
For end-users learning to use your product

**Best for:**
- SaaS products
- Desktop applications
- Mobile apps
- Complex features

**What you'll get:**
- Overview template
- Getting started guide
- Feature explanation templates
- Tutorial templates
- Troubleshooting guide
- FAQ template

**Estimated length:** 20-40 pages
**Time to complete:** 2-3 weeks

**Example structure:**
```
- Introduction & What You'll Learn
- System Requirements
- Installation/Setup
- Basic Usage
- Key Features (with tutorials)
- Advanced Usage
- Troubleshooting
- FAQ
```

---

### 3. **Developer Guide**
For developers implementing or extending your system

**Best for:**
- Open source projects
- SDKs and libraries
- Developer tools
- Extensible platforms

**What you'll get:**
- Architecture overview template
- Setup & development environment
- Code structure guide
- Common tasks templates
- API reference template
- Extension/plugin guide
- Contributing guidelines

**Estimated length:** 40-60 pages
**Time to complete:** 3-4 weeks

**Example structure:**
```
- Project Overview & Architecture
- Development Setup
- Project Structure
- Core Concepts
- Common Development Tasks
- API Reference
- Extending the System
- Contributing Guide
- Troubleshooting
```

---

### 4. **Getting Started Guide**
Quick introduction for new users

**Best for:**
- Onboarding new users
- Quick wins
- Simple products
- Demo/trial period

**What you'll get:**
- 5-minute setup template
- First task template
- Quick reference
- Common questions

**Estimated length:** 5-10 pages
**Time to complete:** 1 week

**Example structure:**
```
- What You'll Need (Prerequisites)
- 5-Minute Setup
- Your First Task
- Next Steps
- Quick Reference Card
```

---

### 5. **Technical Reference**
Comprehensive reference documentation

**Best for:**
- Complex systems
- Configuration options
- API specifications
- Parameter documentation

**What you'll get:**
- Configuration reference template
- Parameter documentation template
- Command/function reference template
- Options matrix template

**Estimated length:** 30-100+ pages
**Time to complete:** 3-6 weeks

**Example structure:**
```
- Configuration Reference
- Environment Variables
- Command-Line Options
- API Reference
- Return Codes & Errors
- Performance Tuning
- Security Considerations
```

---

### 6. **Release Notes**
Document changes in new versions

**Best for:**
- Version updates
- Feature releases
- Bug fix documentation
- Migration guides

**What you'll get:**
- Release note template
- Changelog template
- Migration guide template
- Breaking changes template

**Estimated length:** 2-5 pages per release
**Time to complete:** 1-2 hours

**Example structure:**
```
- Version Number & Date
- New Features
- Improvements
- Bug Fixes
- Breaking Changes
- Migration Guide (if needed)
- Known Issues
```

## How to Use This Command

### Step 1: Choose Your Type
When you run `/write-docs`, you'll be asked:

**"What type of documentation are you creating?"**
- API Documentation
- User Guide
- Developer Guide
- Getting Started
- Technical Reference
- Release Notes

### Step 2: Define Your Audience
Describe your target readers:
- Technical level (beginner, intermediate, advanced)
- What they know already
- What they want to accomplish
- Pain points

### Step 3: Get Your Template
You'll receive:
- Complete outline
- Section templates
- Best practices for each section
- Example content
- Writing tips
- Formatting guidelines

### Step 4: Start Writing
Using the template:
1. Fill in content section by section
2. Check against best practices
3. Get feedback on clarity
4. Refine and improve
5. Publish!

## Example Workflows

### Creating API Documentation
```
1. Run /write-docs
2. Choose "API Documentation"
3. Select your API type (REST, GraphQL, etc.)
4. Provide API overview
5. Receive OpenAPI template
6. Fill in endpoints
7. Add examples
8. Generate code samples
9. Review with /review-docs
```

### Creating a User Guide
```
1. Run /write-docs
2. Choose "User Guide"
3. Define target user level
4. Describe main features
5. Receive guide structure
6. Write feature sections
7. Add screenshots/diagrams
8. Create troubleshooting
9. Write FAQ
```

### Creating Getting Started
```
1. Run /write-docs
2. Choose "Getting Started"
3. Describe setup complexity
4. Receive quick start template
5. Fill in 5-minute setup
6. Create first task example
7. Add quick reference
8. Keep to 5-10 pages
```

## Best Practices This Command Emphasizes

### Clarity First
- Use simple language
- Define technical terms
- Provide examples
- Show expected results

### Structure Matters
- Clear headings
- Logical flow
- Easy navigation
- Consistent formatting

### Examples Are Essential
- Real working code
- Common use cases
- Multiple languages
- Step-by-step tasks

### Keep Updated
- Version documentation
- Track changes
- Update examples
- Note deprecations

### Accessibility
- Descriptive headings
- Alt text for images
- Plain language
- Keyboard navigation

## Pro Tips

✅ **Before You Start:**
1. Gather technical information
2. Identify your audience
3. List key topics
4. Collect existing docs
5. Plan structure

✅ **While Writing:**
1. Follow the template structure
2. Write section by section
3. Include plenty of examples
4. Get peer review
5. Test all code examples

✅ **After Writing:**
1. Proofread carefully
2. Check all links
3. Verify code examples work
4. Test with target users
5. Publish and promote

## Common Challenges

### Challenge 1: "I don't know where to start"
**Solution:** Run `/write-docs` and choose your type. The template will guide you.

### Challenge 2: "My API is complex"
**Solution:** Break into logical sections. Use the API Documentation template. Request `/generate-examples` for code samples.

### Challenge 3: "Examples are outdated"
**Solution:** Keep examples in version control. Test them regularly. Update when code changes.

### Challenge 4: "Need better structure"
**Solution:** Use the templates provided. Follow the best practices. Request `/review-docs` for structural feedback.

## Related Commands

- **`/api-template`** - Get OpenAPI specification templates
- **`/review-docs`** - Get feedback on your documentation
- **`/generate-examples`** - Create code examples automatically

## Timeline Estimates

| Type | Pages | Time |
|------|-------|------|
| Getting Started | 5-10 | 1 week |
| Release Notes | 2-5 | 1-2 hours |
| API Docs | 30-50 | 2-4 weeks |
| User Guide | 20-40 | 2-3 weeks |
| Developer Guide | 40-60 | 3-4 weeks |
| Reference | 30-100+ | 3-6 weeks |

## Next Steps

1. **Run the command:** `/write-docs`
2. **Choose your type:** Pick what fits your needs
3. **Follow the template:** Structure is provided
4. **Get feedback:** Use `/review-docs` while writing
5. **Use examples:** Request `/generate-examples` for code
6. **Publish:** Share with your users!

---

**Remember:** Good documentation is a process, not a one-time task. Start with this command, iterate, and improve over time! 📝
