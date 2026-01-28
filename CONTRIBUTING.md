# Contributing to Power Platform Journey

Thank you for your interest in contributing! This repository aims to share practical, hands-on knowledge about Power Platform and Copilot Studio.

## How to Contribute

### 1. Writing Blog Posts

We welcome blog posts that share:
- Real-world implementation experiences
- Solutions to common problems
- Best practices and patterns
- Troubleshooting guides
- Integration examples
- Performance optimization techniques

### 2. Content Guidelines

#### What Makes a Good Blog Post?

- **Practical focus** - Based on actual implementations
- **Clear examples** - Include code snippets and configurations
- **Real challenges** - Share problems you faced and how you solved them
- **Best practices** - What worked well in production
- **Resources** - Link to relevant documentation and tools

#### What to Avoid

- Copy-pasting from official documentation
- Purely theoretical content without practical examples
- Promotional content or product advertisements
- Outdated information (always indicate the date and version)

### 3. Blog Post Structure

Use the provided [BLOG_POST_TEMPLATE.md](./BLOG_POST_TEMPLATE.md) as a starting point.

Required sections:
```markdown
# Title
**Date:** Month Year
**Author:** Your Name
**Category:** Category Name
**Tags:** #relevant #tags

## Overview
## Main Content
## Real-World Example
## Best Practices
## Resources
```

### 4. File Naming Convention

Use descriptive, kebab-case names with a number prefix:

```
Good examples:
- 01-getting-started.md
- 02-building-first-agent.md
- 03-advanced-conversation-design.md

Avoid:
- mypost.md
- Blog1.md
- copilot_studio_guide.md
```

### 5. Categories

Place your content in the appropriate folder:

- **copilot-studio/** - Copilot Studio agents and conversational AI
- **power-automate/** - Workflow automation and flows
- **power-apps/** - Canvas and model-driven app development
- **integrations/** - API connections, custom connectors, Azure integrations
- **troubleshooting/** - Debugging, performance, error resolution

### 6. Submission Process

1. **Check existing content** - Avoid duplicating existing posts
2. **Create your blog post** - Use the template and follow guidelines
3. **Add to category README** - Update the appropriate category README.md with a link
4. **Submit a pull request** - Include a description of your contribution
5. **Respond to feedback** - Engage with reviewers constructively

### 7. Code Examples

When including code:

#### Power Automate Expressions
```
Use clear formatting:
if(greater(length(variables('items')), 0), 
   'Has items', 
   'Empty')
```

#### Power Apps Formulas
```
Include context:
// Gallery filter for active items only
Filter(Products, 
    Status = "Active" && 
    Category = drp_CategoryFilter.Selected.Value)
```

#### API Calls
```json
Show request/response:
{
  "method": "POST",
  "endpoint": "/api/customers",
  "headers": {
    "Authorization": "Bearer {token}",
    "Content-Type": "application/json"
  },
  "body": {
    "name": "Contoso",
    "industry": "Technology"
  }
}
```

### 8. Images and Screenshots

- Use images to clarify complex concepts
- Place images in an `images/` subfolder within the category
- Use descriptive filenames: `flow-error-handling-pattern.png`
- Optimize images for web (compress, reasonable dimensions)
- Include alt text for accessibility

### 9. Links

- **Internal links** - Use relative paths: `[Link text](./other-post.md)`
- **External links** - Use full URLs: `[Microsoft Docs](https://learn.microsoft.com/...)`
- **Always verify** - Ensure all links work before submitting

### 10. Tone and Style

- **Professional but approachable** - Write as if explaining to a colleague
- **Be honest** - Share failures and challenges, not just successes
- **Be specific** - Include versions, dates, and concrete details
- **Be helpful** - Focus on helping others learn from your experience

## Code of Conduct

- Be respectful and constructive
- Focus on the content, not the person
- Help create a welcoming learning environment
- Share knowledge generously
- Give credit where due

## Questions?

If you're unsure about anything:
1. Check existing blog posts for examples
2. Review the template and guidelines
3. Open an issue to ask questions
4. Start a discussion for broader topics

## Recognition

Contributors will be credited in:
- Their blog post author line
- Repository contributors list
- Special recognition for significant contributions

## License

By contributing, you agree that your contributions will be licensed under the same terms as the repository.

---

Thank you for helping make Power Platform learning more accessible and practical! 🚀
