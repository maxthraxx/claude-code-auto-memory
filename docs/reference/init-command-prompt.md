# /init Command Prompt Reference

This document captures the raw instructions invoked by Claude Code's built-in `/init` slash command.

## Raw Prompt

```
Please analyze this codebase and create a CLAUDE.md file, which will be given to future instances of Claude Code to operate in this repository.

What to add:
1. Commands that will be commonly used, such as how to build, lint, and run tests. Include the necessary commands to develop in this codebase, such as how to run a single test.
2. High-level code architecture and structure so that future instances can be productive more quickly. Focus on the "big picture" architecture that requires reading multiple files to understand.

Usage notes:
- If there's already a CLAUDE.md, suggest improvements to it.
- When you make the initial CLAUDE.md, do not repeat yourself and do not include obvious instructions like "Provide helpful error messages to users", "Write unit tests for all new utilities", "Never include sensitive information (API keys, tokens) in code or commits".
- Avoid listing every component or file structure that can be easily discovered.
- Don't include generic development practices.
- If there are Cursor rules (in .cursor/rules/ or .cursorrules) or Copilot rules (in .github/copilot-instructions.md), make sure to include the important parts.
- If there is a README.md, make sure to include the important parts.
- Do not make up information such as "Common Development Tasks", "Tips for Development", "Support and Documentation" unless this is expressly included in other files that you read.
- Be sure to prefix the file with the following text:

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.
```

## Observations

The `/init` command instructs Claude to:

1. **Analyze the codebase** - Understand the project structure
2. **Create CLAUDE.md** - Generate context documentation for future sessions
3. **Focus on actionable content**:
   - Build/lint/test commands
   - High-level architecture (multi-file understanding)
4. **Avoid noise**:
   - No generic best practices
   - No obvious instructions
   - No made-up sections
   - No exhaustive file listings
5. **Integrate existing docs**:
   - Check for Cursor rules (`.cursor/rules/`, `.cursorrules`)
   - Check for Copilot rules (`.github/copilot-instructions.md`)
   - Include relevant README.md content
6. **Handle existing CLAUDE.md** - Suggest improvements if one exists

## Use Case for claude-code-memory

This prompt serves as a reference for implementing the `/memory-init` command, which should produce similar quality output but with automatic maintenance capabilities.

## Related Documentation

- **PRD.md** - See "Baseline: Claude Code's /init Command" section for how `/memory-init` enhances this baseline
- **SPEC.md** - See "Reference: Claude Code /init Baseline" section for technical implementation details
- **SPEC.md** - See template definitions for root (150-200 lines) and subtree (50-75 lines) CLAUDE.md files
