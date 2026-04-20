# Contributing to Vibe Modelling Skill

Thanks for your interest in contributing to the Vibe Modelling skill. This guide covers how to propose changes, the standards we follow, and how to get your contribution merged.

## How to Contribute

### Reporting Issues

If you find a problem with the skill (incorrect SimPy patterns, unclear instructions, missing scenarios), open an issue on GitHub with:

- A description of the problem
- The prompt you used that triggered the issue
- What you expected to happen vs what actually happened
- Which AI tool you were using (Claude Code, Gemini CLI, etc.)

### Suggesting Enhancements

Ideas for new simulation patterns, additional industry examples, or workflow improvements are welcome. Open an issue tagged `enhancement` describing:

- What the skill currently lacks
- Your proposed addition or change
- A concrete example of when it would be useful

### Submitting Changes

1. **Fork the repository** and create a feature branch from `main`:
   ```bash
   git checkout -b feat/your-feature-name
   ```

2. **Make your changes** following the standards below.

3. **Test your changes** by installing the skill locally and running representative prompts through your AI tool of choice.

4. **Commit** with a conventional commit message:
   ```bash
   git commit -m "feat: add batch processing example to simpy-patterns"
   ```

5. **Open a pull request** against `main` with a clear description of what you changed and why.

## Standards

### Commit Messages

Follow conventional commits:

```
<type>: <description>

<optional body>
```

Types: `feat`, `fix`, `docs`, `refactor`, `chore`

### Writing Style

All content in this skill uses **British English**:
- colour, not color
- initialise, not initialize
- organisation, not organization
- modelling, not modeling
- analyse, not analyze

Do not use the Oxford comma. Do not use the em-dash.

### SKILL.md Guidelines

- Keep the main SKILL.md under 500 lines
- Use imperative form in instructions ("Generate code", not "You should generate code")
- Explain the *why* behind instructions, not just the *what*
- Avoid excessive use of MUST/NEVER/ALWAYS; explain reasoning instead

### Reference Files

- Place detailed content in `references/` rather than bloating SKILL.md
- Include a table of contents if a reference file exceeds 300 lines
- Code examples should be complete and runnable

### SimPy Code Standards

All code examples and patterns must:
- Use SimPy (not other simulation libraries)
- Include comments explaining each section for non-programmers
- Use British English in comments and output strings
- Put configurable parameters at the top of the script
- Include visualisation code using matplotlib
- Be compatible with Python 3.8+ and Google Colab

### Agent Skills Standard Compliance

This skill follows the [Agent Skills open standard](https://agentskills.io). Changes must:
- Maintain valid YAML frontmatter in SKILL.md
- Keep the `name` field as lowercase letters, numbers, and hyphens only (max 64 chars)
- Keep the `description` field under 1024 characters
- Not introduce platform-specific extensions without clear documentation

## Directory Structure

```
vibe-modelling-skill/
├── .gitignore
├── CONTRIBUTING.md          # This file
├── LICENSE                  # MIT
├── README.md                # Installation and usage docs
└── vibe-modelling/          # The skill itself
    ├── SKILL.md             # Core instructions (< 500 lines)
    └── references/
        ├── frameworks.md    # OSVC, Three Bricks, worked examples
        └── simpy-patterns.md # Code patterns, distributions, troubleshooting
```

If adding new reference files, update SKILL.md with a pointer explaining when the agent should read the new file.

## Testing Your Changes

Before submitting a PR, verify the skill works by:

1. Installing it locally in your AI tool
2. Running at least 3 representative prompts:
   - A simple queuing problem (e.g. coffee shop)
   - A multi-resource sequential process (e.g. hospital pathway)
   - A what-if scenario comparison
3. Confirming the generated code runs without errors in Google Colab or a local Python environment
4. Checking that the agent follows the 6-step process appropriately

## Code of Conduct

Be respectful and constructive. We are building a tool to help non-technical people access the power of simulation; contributions should maintain that spirit of accessibility and clarity.

## Questions?

Open a discussion on GitHub or reach out via [www.schoolofsimulation.com](https://www.schoolofsimulation.com).
