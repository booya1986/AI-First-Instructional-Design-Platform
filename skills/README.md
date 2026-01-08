# Agent Skills

Custom Claude Code skills for this instructional design platform project.

## What Are Skills?

Skills are organized folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. They provide domain expertise and best practices for specific workflows.

## Available Skills

### 🎨 Frontend Engineer
**Path:** `skills/frontend-engineer/`

**When to Use:**
- Building new presentation slides
- Converting decks to Hebrew RTL
- Creating interactive data visualizations
- Implementing navigation controls
- Ensuring design system compliance
- Optimizing images and performance

**What It Includes:**
- **SKILL.md**: Complete 5-phase implementation workflow
- **css-patterns.md**: Reusable CSS components and layouts
- **javascript-patterns.md**: Navigation and interaction patterns

**Key Capabilities:**
- Claude Code Dark design system expertise
- Single-file HTML architecture
- Bilingual (English/Hebrew) support with RTL
- Performance optimization guidance
- Accessibility best practices
- Component pattern library

## How to Use Skills

### Method 1: Manual Reference
Skills are markdown files you can read directly:
```bash
cat skills/frontend-engineer/SKILL.md
```

### Method 2: Claude Code Integration
If using Claude Code CLI with skill support:
```bash
# Load skill in conversation
/skill frontend-engineer

# Or reference specific patterns
/skill frontend-engineer css-patterns
```

### Method 3: API Integration
For Claude API usage, upload skills via the [Skills API](https://docs.claude.com/en/api/skills-guide).

## Skill Structure

Each skill follows this pattern:

```
skills/
└── skill-name/
    ├── SKILL.md              # Main instructions (required)
    ├── reference-1.md        # Supplementary context (optional)
    └── reference-2.md        # Additional resources (optional)
```

### Required: SKILL.md Format

```markdown
---
name: skill-name
description: Clear description of what this skill does and when to use it
---

# Skill Name

[Instructions that Claude will follow when this skill is active]

## Phase 1: Understanding
...

## Phase 2: Implementation
...
```

## Creating New Skills

To add a new skill for this project:

1. **Create Directory**
   ```bash
   mkdir -p skills/your-skill-name
   ```

2. **Create SKILL.md**
   - Start with YAML frontmatter (`name` and `description`)
   - Structure as phases or sections
   - Include examples and anti-patterns
   - Reference project-specific files

3. **Add Supplementary Files** (optional)
   - `patterns.md` - Common code patterns
   - `reference.md` - External resources
   - `examples.md` - Usage examples

4. **Update .gitignore**
   Skills are already whitelisted via `!skills/**`

5. **Update This README**
   Add your skill to the "Available Skills" section

## Best Practices

### Writing Effective Skills

✅ **Do:**
- Be specific to this project's needs
- Include concrete examples from the codebase
- Reference actual file paths ([user_story_deck.html](../user_story_deck.html))
- Provide both patterns and anti-patterns
- Structure for progressive disclosure (load context as needed)
- Include quality checklists

❌ **Don't:**
- Make skills too generic
- Duplicate what Claude already knows
- Include sensitive data or credentials
- Create overlapping skills
- Forget to test the skill with Claude

### Progressive Disclosure

Structure skills in layers:

1. **Metadata** (YAML frontmatter): Always loaded
2. **Core Instructions** (SKILL.md): Load when skill is invoked
3. **Detailed Patterns** (reference files): Load only when needed

Example:
```markdown
# Phase 1: Quick Start
[Common scenarios, 80% use cases]

# Phase 2: Advanced Patterns
[Complex scenarios, reference css-patterns.md for details]

# Phase 3: Edge Cases
[Rarely needed, load javascript-patterns.md if required]
```

## Testing Skills

Before committing a new skill:

1. **Test with Claude**
   ```
   User: "Build a new slide using the frontend-engineer skill"
   Claude: [Should load skill and follow its guidance]
   ```

2. **Verify Structure**
   - YAML frontmatter parses correctly
   - Markdown renders properly
   - File paths work relative to project root

3. **Check References**
   - All linked files exist
   - Code examples are accurate
   - No broken internal links

## Examples from This Project

### Frontend Engineer Skill Usage

**Scenario 1: Creating New Slide**
```
User: "Add a new slide showing team metrics"
Claude: [Loads frontend-engineer skill]
        [Follows design system from SKILL.md]
        [Uses stat-card pattern from css-patterns.md]
        [Implements navigation from javascript-patterns.md]
```

**Scenario 2: RTL Conversion**
```
User: "Convert investor deck to Hebrew"
Claude: [Loads frontend-engineer skill]
        [Follows RTL guidelines from Phase 2]
        [Applies RTL CSS patterns]
        [Updates navigation for Hebrew text]
```

## Resources

### Anthropic Documentation
- [What Are Skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Creating Custom Skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Agent Skills Engineering](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

### Skill Repositories
- [Anthropic Skills](https://github.com/anthropics/skills) - Official examples
- [Agent Skills Spec](http://agentskills.io) - Specification

### Project References
- [Design Principles](../sildeshow%20deck%20design%20principelce.md)
- [User Story Deck](../user_story_deck.html)
- [Hebrew RTL Version](../user_story_deck_he.html)

---

**Need Help?**
- Read existing skills for patterns
- Check Anthropic's skill examples
- Test skills in conversation before committing
- Keep skills focused and project-specific

*Skills make Claude an expert in your project's specific workflows and patterns.*
