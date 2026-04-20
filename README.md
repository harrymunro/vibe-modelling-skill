# Vibe Modelling Skill 🤖🏭

[![Agent Skills](https://img.shields.io/badge/Standard-Agent_Skills-blue?style=flat-square)](https://agentskills.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg?style=flat-square)](https://www.python.org/downloads/)
[![SimPy](https://img.shields.io/badge/SimPy-Simulation-orange?style=flat-square)](https://simpy.readthedocs.io/)

> An agent skill for building discrete-event simulations using SimPy and AI, based on the book [*Vibe Modelling: Build Digital Twins and Simulations with AI*](https://a.co/d/0iKdgqba) by Harry Munro.

---

## 📖 Table of Contents
- [What It Does](#what-it-does)
- [Who It's For](#who-its-for)
- [Key Features](#key-features)
- [Installation](#installation)
- [Usage & Examples](#usage)
- [Project Structure](#structure)
- [The Book](#the-book)
- [License](#license)

---

## ✨ What It Does

This skill transforms your AI coding agent into an expert simulation modeller. It guides the AI through the complete simulation modelling process: from translating a vague business problem to delivering a working, verified SimPy simulation with what-if analysis.

It strictly adheres to the **6-step AI Modelling and Simulation Process**:
1. **Problem Definition & Scoping** (OSVC Framework)
2. **Data Requirements & Collection**
3. **Conceptual Model Design** (Entities, Resources, Processes)
4. **Simulation Build** (Robust, modular SimPy Python code)
5. **Execution, Verification & Refinement**
6. **Reporting & Insights** (What-if scenarios, sensitivity analysis)

## 🎯 Who It's For

- **Business Analysts & Managers** who want to build operational simulations without learning to code.
- **Process Improvement Specialists** following the *Vibe Modelling* approach to AI-assisted simulation.
- **Readers of the Book** looking to configure their AI agents to natively understand the methodology.
- **Developers** needing a structured way to prompt agents for complex discrete-event simulations.

## 🚀 Key Features

- **Built-in Prompts & Context**: Injects best practices for SimPy right into the agent's context.
- **OSVC Framework Integration**: Ensures the agent starts by defining the *Objective, Scope, Variables, and Constraints*.
- **Three Bricks Paradigm**: Forces the AI to model using the fundamental building blocks of discrete-event simulation (Entities, Resources, Processes).
- **Cross-Platform Compatibility**: Works seamlessly with Claude Code, Gemini CLI, GitHub Copilot, and other agent platforms.

## ⚙️ Installation

### Agent Skills Standard (Cross-Platform)

This skill follows the [Agent Skills open standard](https://agentskills.io). You can install it in any compatible AI coding tool.

**Claude Code:**
```bash
# Copy the skill directory to your global skills folder
cp -r vibe-modelling ~/.claude/skills/

# Or for project-level use
cp -r vibe-modelling .claude/skills/
```

**Gemini CLI:**
```bash
npx skills add harrymunro/vibe-modelling-skill --skill vibe-modelling

# Or manually:
cp -r vibe-modelling ~/.gemini/skills/
```

**GitHub Copilot / VS Code:**
```bash
cp -r vibe-modelling .agents/skills/
```

### Portable Path (Broadest Compatibility)

For project-level installation recognised by most modern agent tools:
```bash
mkdir -p .agents/skills
cp -r vibe-modelling .agents/skills/
```

## 💻 Usage

Once installed, the skill activates automatically when you prompt the agent with a scenario involving queues, bottlenecks, capacity planning, or simulation building.

### Requirements

To run the outputted code, your environment (e.g., local machine, Google Colab) will need:
- **Python 3.8+**
- **SimPy** (`pip install simpy`)
- **Matplotlib** (`pip install matplotlib`) for visualisations

### Example Prompts

Try these prompts to trigger the skill:

> ☕ *"I run a coffee shop and want to know if buying a faster machine would pay for itself."*

> 📦 *"Model my warehouse picking process. I have 4 forklifts and orders arrive every 2 minutes."*

> 🏥 *"I need to simulate my hospital A&E department to reduce waiting times."*

> 📞 *"What happens to my call centre abandonment rate if I add 2 more agents in the morning?"*

## 📁 Structure

```text
vibe-modelling/
├── SKILL.md              # Main skill instructions (the core agent prompt)
└── references/
    ├── frameworks.md     # OSVC framework, Three Bricks, worked examples
    └── simpy-patterns.md # Code patterns, distributions, troubleshooting logic
```

## 📚 The Book

*Vibe Modelling: Build Digital Twins and Simulations with AI* teaches managers how to build simulations using AI tools without writing code. 

Learn more, access resources, and join the community at **[www.schoolofsimulation.com](https://www.schoolofsimulation.com)**.

## 📄 License

This project is licensed under the [MIT License](LICENSE).
