# Claude Code Command Templates

A collection of specialized system prompts and command templates for Claude Code to enhance software development workflows.

## Overview

This repository contains a comprehensive set of Claude AI system prompts designed to provide specialized operational modes for software development tasks. Each mode is optimized for specific development scenarios and includes built-in safety mechanisms and user approval workflows.

## Available Commands

### Development Modes

- **`architect.md`** - Architecture planning and system design mode
- **`framework.md`** - Greenfield project creation and framework setup
- **`debug.md`** - Systematic debugging and troubleshooting
- **`cleanup.md`** - Codebase maintenance and refinement

### Analysis & Understanding

- **`explain.md`** - Code teaching and detailed explanations
- **`find.md`** - Code tracing and implementation flow analysis
- **`check-understanding.md`** - Code analysis and comprehension verification
- **`interrogate.md`** - Rigorous investigation and evidence-based validation

### Code Quality & Reviews

- **`pr.md`** - Comprehensive pull request reviews
- **`pre-commit review.md`** - Pre-commit review process guidelines
- **`commit.md`** - Automated git commit process with quality checks

### Information & Research

- **`ask.md`** - Q&A assistant with research capabilities
- **`research.md`** - Web research and external documentation lookup
- **`docs.md`** - Technical documentation creation
- **`ideate.md`** - Creative brainstorming and solution exploration mode

### Git & Change Management

- **`changes.md`** - Uncommitted changes analysis
- **`changes-branch.md`** - Git branch comparison utilities
- **`changes-working.md`** - Working directory change analysis

### Project Management

- **`set-phase.md`** - Development phase management (MVP, production, hotfix)
- **`check-pattern.md`** - Existing codebase pattern analysis
- **`enhance-command.md`** - Command prompt assessment and improvement tool

## Usage

These command templates are designed to be used with Claude Code to provide specialized assistant behaviors for different development tasks. Each file contains:

- Detailed system prompts
- Role-specific instructions
- Safety mechanisms and approval workflows
- Tool usage guidelines
- Best practices for the specific mode

## Features

- **Role-Based Operations**: Each mode provides specialized behavior for specific development tasks
- **Safety Mechanisms**: Built-in user approval workflows for destructive operations
- **Tool Integration**: Optimized for Claude Code's available tools (Bash, Git, file operations, etc.)
- **Best Practices**: Incorporates software development best practices and security considerations
- **Modular Design**: Each command can be used independently or as part of a larger workflow

## Installation

Just add these to your .claude/commands folder to use them

## Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests to improve these command templates.

## License

This project is open source and available under the MIT License.

## Acknowledgments

Created for use with [Claude Code](https://claude.ai/code) - Anthropic's official CLI for Claude AI.
