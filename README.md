# Vibe Modelling Skill

An agent skill for building discrete-event simulations using SimPy and AI, based on the book *Vibe Modelling: Build Digital Twins and Simulations with AI* by Harry Munro.

## What It Does

This skill guides AI coding agents through the complete simulation modelling process: from a vague business problem to a working, verified SimPy simulation with what-if analysis. It follows the 6-step AI Modelling and Simulation Process:

1. **Problem Definition & Scoping** (OSVC Framework)
2. **Data Requirements & Collection**
3. **Conceptual Model Design** (Entities, Resources, Processes)
4. **Simulation Build** (complete SimPy Python code)
5. **Execution, Verification & Refinement**
6. **Reporting & Insights** (what-if scenarios, sensitivity analysis)

## Who It's For

- Non-technical managers who want to build simulations without coding
- Anyone following the Vibe Modelling approach to AI-assisted simulation
- Readers of the book looking for an AI co-pilot that follows the methodology

## Installation

### Agent Skills Standard (Cross-Platform)

This skill follows the [Agent Skills open standard](https://agentskills.io). Install it in any compatible tool:

**Claude Code:**
```bash
# Copy the skill directory to your skills folder
cp -r vibe-modelling ~/.claude/skills/

# Or for project-level use
cp -r vibe-modelling .claude/skills/
```

**Gemini CLI:**
```bash
# If npx skills is available
npx skills add harrymunro/vibe-modelling-skill --skill vibe-modelling

# Or manually
cp -r vibe-modelling ~/.gemini/skills/
```

**GitHub Copilot / VS Code:**
```bash
cp -r vibe-modelling .agents/skills/
```

**Any compatible agent:**
```bash
# Place in the tool's skills directory
cp -r vibe-modelling <your-tool-skills-path>/
```

### Portable Path (Broadest Compatibility)

For project-level installation recognised by most tools:
```bash
mkdir -p .agents/skills
cp -r vibe-modelling .agents/skills/
```

## Requirements

- Python 3.8+ environment (Google Colab, local Python, or similar)
- SimPy library (`pip install simpy`)
- matplotlib for visualisations (`pip install matplotlib`)

## Usage

Once installed, the skill activates automatically when you describe:
- A business process with queues, bottlenecks, or capacity constraints
- A "what-if" scenario you want to test
- An operational problem involving entities flowing through resources
- Any request to build a simulation or digital twin

**Example prompts that trigger the skill:**

> "I run a coffee shop and want to know if buying a faster machine would pay for itself"

> "Model my warehouse picking process - I have 4 forklifts and orders arrive every 2 minutes"

> "I need to simulate my hospital A&E department to reduce waiting times"

> "What happens to my call centre abandonment rate if I add 2 more agents in the morning?"

## Structure

```
vibe-modelling/
├── SKILL.md              # Main skill instructions (the core)
└── references/
    ├── frameworks.md     # OSVC framework, Three Bricks, worked examples
    └── simpy-patterns.md # Code patterns, distributions, troubleshooting
```

## The Book

*Vibe Modelling: Build Digital Twins and Simulations with AI* teaches managers how to build simulations using AI tools without writing code. Learn more at [www.schoolofsimulation.com](https://www.schoolofsimulation.com).

## Licence

MIT
