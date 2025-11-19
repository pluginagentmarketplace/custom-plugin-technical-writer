# /review-docs - Documentation Review & Improvement

## Purpose
Get comprehensive feedback on your documentation. Improve clarity, identify gaps, ensure accuracy, and optimize for your target audience.

## What This Command Does

Analyzes your documentation for:

### 1. **Clarity & Readability**
- ✅ Simple, clear language
- ✅ Appropriate technical level
- ✅ Consistent terminology
- ✅ Well-organized structure
- ✅ Logical flow

### 2. **Completeness**
- ✅ All major topics covered
- ✅ Prerequisites documented
- ✅ Examples included
- ✅ Edge cases addressed
- ✅ Troubleshooting provided

### 3. **Technical Accuracy**
- ✅ Correct information
- ✅ Current versions referenced
- ✅ Code examples work
- ✅ Screenshots up-to-date
- ✅ No deprecated features

### 4. **Structure & Format**
- ✅ Proper heading hierarchy
- ✅ Good visual hierarchy
- ✅ Consistent formatting
- ✅ Easy navigation
- ✅ Proper Markdown/HTML

### 5. **Accessibility**
- ✅ Descriptive headings
- ✅ Alt text for images
- ✅ Keyboard navigation
- ✅ Sufficient contrast
- ✅ Accessible tables

### 6. **Best Practices**
- ✅ Active voice preferred
- ✅ Examples are realistic
- ✅ Consistent style
- ✅ Professional tone
- ✅ SEO optimized

## How to Use

### Step 1: Prepare Your Documentation
Have ready:
- Your documentation file (Markdown, HTML, etc.)
- Target audience description
- Any specific concerns
- Context about the product/API

### Step 2: Run the Command
```
/review-docs
```

### Step 3: Provide Your Documentation
Paste or upload:
- The documentation text
- Or: Share what you want reviewed
- Or: Ask about specific sections

### Step 4: Specify Your Focus
Choose areas to emphasize:
- **Comprehensive Review** - Full analysis
- **Clarity Focus** - Readability and simplicity
- **Accuracy Check** - Technical correctness
- **Structure Review** - Organization and flow
- **SEO Optimization** - Search visibility
- **Accessibility Audit** - WCAG compliance

### Step 5: Get Detailed Feedback
Receive:
- Specific issues found
- Suggestions for improvement
- Examples of how to fix
- Best practices references
- Priority of improvements

## Review Categories

### Clarity Review

Checks:
- Is language simple and direct?
- Are technical terms defined?
- Is jargon explained?
- Are sentences concise?
- Is tone consistent?

**Feedback includes:**
- Sentences that are too complex
- Unexplained technical terms
- Inconsistent phrasing
- Redundant text
- Suggestions for simplification

---

### Completeness Review

Checks:
- Are all major topics covered?
- Are prerequisites clear?
- Are there enough examples?
- Is troubleshooting included?
- Are edge cases addressed?

**Feedback includes:**
- Missing sections
- Topics needing expansion
- Insufficient examples
- Gaps in instructions
- Uncovered error scenarios

---

### Accuracy Review

Checks:
- Is information correct?
- Are versions current?
- Do code examples work?
- Are URLs valid?
- Are screenshots recent?

**Feedback includes:**
- Incorrect information
- Outdated versions
- Non-functional examples
- Broken links
- Stale screenshots

---

### Structure Review

Checks:
- Is organization logical?
- Is hierarchy correct?
- Is flow smooth?
- Is navigation clear?
- Is formatting consistent?

**Feedback includes:**
- Better organization suggestions
- Heading hierarchy issues
- Poor section transitions
- Missing table of contents
- Inconsistent formatting

---

### Accessibility Review

Checks:
- Are headings descriptive?
- Do images have alt text?
- Is contrast sufficient?
- Can it be used with keyboard?
- Are tables accessible?

**Feedback includes:**
- Missing alt text
- Vague headings
- Color contrast issues
- Keyboard navigation problems
- Inaccessible table structures

---

### SEO Review

Checks:
- Are keywords natural?
- Is meta description good?
- Are headings SEO-optimized?
- Is content unique?
- Are internal links present?

**Feedback includes:**
- SEO keyword suggestions
- Meta description improvement
- Heading optimization tips
- Link suggestions
- Content freshness

## Review Levels

### Quick Review (5 min)
- Scan for major issues
- Identify biggest problems
- Quick suggestions

**Best for:** Quick feedback, before major revision

---

### Standard Review (15 min)
- Comprehensive analysis
- Detailed feedback
- Prioritized suggestions
- Examples of improvements

**Best for:** Regular improvement, before publishing

---

### Deep Review (30 min)
- Section-by-section analysis
- Detailed editing suggestions
- Alternative wording options
- Complete improvement plan
- Rewrite examples

**Best for:** Important documentation, final polish

---

### Audience-Focused Review
Customize review for:
- **Beginners** - Check assumptions, explain terms
- **Intermediate** - Check examples complexity
- **Advanced** - Check depth, missing details
- **Non-technical** - Check jargon, simplicity

## Review Checklist

### Before You Submit for Review

- [ ] Content is complete
- [ ] Grammar is checked
- [ ] Links are tested
- [ ] Code examples are verified
- [ ] Screenshots are current
- [ ] Formatting is consistent
- [ ] Audience is clear
- [ ] Purpose is clear

### After You Get Feedback

- [ ] Read all suggestions
- [ ] Categorize by priority
- [ ] Plan revisions
- [ ] Make changes
- [ ] Test code examples
- [ ] Update screenshots if needed
- [ ] Re-review if major changes
- [ ] Publish with confidence

## Common Issues Found

### Top Issues Reviewers Find

1. **Unclear Language**
   - Technical jargon without explanation
   - Complex sentences
   - Passive voice
   - Vague pronouns

2. **Missing Information**
   - No examples
   - Incomplete instructions
   - Missing prerequisites
   - No error handling

3. **Structural Problems**
   - Illogical order
   - Wrong heading levels
   - Poor transitions
   - Hard to navigate

4. **Accuracy Problems**
   - Outdated information
   - Incorrect commands
   - Wrong parameters
   - Deprecated features

5. **Formatting Issues**
   - Inconsistent styling
   - Missing alt text
   - Broken links
   - Poor code formatting

## Improvement Process

### Phase 1: Identify Issues
- Get comprehensive review
- Categorize by severity
- Understand impact
- Prioritize fixes

### Phase 2: Plan Changes
- Review suggestions
- Outline improvements
- Identify quick wins
- Plan major revisions

### Phase 3: Make Improvements
- Address high-priority items
- Improve clarity
- Add missing content
- Fix technical issues

### Phase 4: Verify Changes
- Test code examples
- Check links
- Update screenshots
- Verify accuracy

### Phase 5: Polish & Publish
- Final readthrough
- Format check
- Accessibility check
- Publish confidently

## Examples of Good Improvements

### Before (Unclear)
"The system uses configuration files to manage operations."

### After (Clear)
"Create a config.yml file in your application directory to specify database connection, authentication settings, and logging preferences."

---

### Before (Incomplete)
"Install the package and start using it."

### After (Complete)
```bash
# Install the package
npm install mypackage

# Create a configuration file
cp config.example.yml config.yml

# Edit config.yml with your settings
nano config.yml

# Start the application
npm start
```

---

### Before (Outdated)
"Use version 2.0 for best compatibility"

### After (Current)
"Use version 4.0 (released Jan 2024) for latest features. Version 3.x receives security updates until Dec 2024."

## Best Practices

✅ **DO:**
- Be specific with feedback
- Provide examples
- Suggest improvements
- Consider audience
- Offer alternatives
- Explain reasoning

❌ **DON'T:**
- Be vague
- Just say "fix this"
- Be harsh or dismissive
- Ignore context
- Suggest conflicting things
- Overwhelm with too many changes

## Related Commands

- **`/write-docs`** - Create new documentation
- **`/api-template`** - Get API doc templates
- **`/generate-examples`** - Create code examples

## Pro Tips

**Tip 1: Review Before Publishing**
Always get a fresh review before publishing. You become blind to your own mistakes.

**Tip 2: Share with Target Users**
Have actual users review if possible. They'll spot things experts miss.

**Tip 3: Review in Stages**
First: Structure and completeness
Second: Clarity and accuracy
Third: Polish and formatting

**Tip 4: Keep Improving**
Even published docs can improve. Update based on user feedback.

**Tip 5: Version Your Docs**
Keep track of changes. Revert if needed. Use version control.

## Review Turnaround

- **Quick Review**: 5 minutes
- **Standard Review**: 15-20 minutes
- **Deep Review**: 30-45 minutes
- **Audience-Focused**: 20-30 minutes

## Success Metrics

After review and improvements, your docs should:
- ✅ Be understandable by target audience
- ✅ Include all necessary information
- ✅ Have working code examples
- ✅ Be easy to navigate
- ✅ Be professionally formatted
- ✅ Be current and accurate
- ✅ Be optimized for search
- ✅ Be accessible to all users

---

**Ready to improve your documentation? Run `/review-docs` now!** 📝✨
