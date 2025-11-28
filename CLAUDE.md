# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

claude-code-memory is a Claude Code plugin that automatically maintains CLAUDE.md files as codebases evolve. The plugin uses a token-efficient architecture with hooks to track file changes and an isolated agent to process updates without bloating the main conversation context.

## Current Status

This project is in the **design phase** - only PRD.md and SPEC.md exist. Implementation has not started.

## Architecture

The plugin uses a multi-stage pipeline:

1. **PostToolUse hook** (Edit|Write matcher) - Appends changed file paths to `.claude/.dirty-files` with zero token cost
2. **Stop hook** - Fires at end of turn, checks for dirty files, blocks and requests agent spawn if files exist
3. **memory-updater agent** - Lightweight orchestrator running in isolated context
4. **memory-processor skill** - Progressive disclosure pattern, loads detailed instructions only when invoked

Key design decisions:
- Uses `stop_hook_active` flag to prevent infinite loops when stop hook fires after agent completion
- PostToolUse hook has no output (zero tokens) - purely tracks file paths
- Agent isolation keeps main conversation context clean
- Skills use progressive disclosure for token efficiency

## Planned Plugin Structure

```
.claude-plugin/
├── settings.json           # Hook configurations
├── agents/
│   └── memory-updater.md   # Lightweight orchestrator
├── skills/
│   ├── memory-processor/SKILL.md    # File analysis and CLAUDE.md updates
│   └── codebase-analyzer/SKILL.md   # Init wizard logic
├── commands/
│   ├── memory-init.md      # Interactive setup wizard
│   ├── memory-sync.md      # Force recalibration
│   └── memory-status.md    # View sync status
└── hooks/
    ├── post-tool-use.sh    # Track file changes
    └── stop.sh             # End-of-turn trigger
```

## Key Files

- `PRD.md` - Product requirements, user stories, acceptance criteria
- `SPEC.md` - Technical specification, component details, data structures

## Implementation Notes

When implementing, refer to SPEC.md for:
- Hook configurations and environment variables (`$CLAUDE_FILE_PATHS`, `$CLAUDE_PROJECT_DIR`)
- Marker syntax for auto-managed sections (`<!-- AUTO-MANAGED: section-name -->`)
- Dirty files format (plain text, one path per line)
- Error handling strategies
- Dependencies: `jq` for JSON parsing, `node-fetch` and `cheerio` for docs fetching
