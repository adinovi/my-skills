# Local-Confluence Synchronization Skill

A powerful agentic skill for bidirectional content synchronization between a Local vault and Confluence.

## Features
- **Multi-Mode Support**: Choose between **Local** (via MCP) or a **Generic Folder** (direct filesystem access).
- **Bidirectional Sync**: Keeps local files and remote pages in sync.
- **Agent Shortcuts**: Quick commands like `@push`, `@pull`, and `@sync`.
- **Natural Language Support**: Ask to "sync this note" or "update from confluence".
- **Metadata Management**: Uses YAML frontmatter to track sync state.
- **MANDATORY Setup Check**: Mode selection and connectivity verification at startup.

## Installation

1.  Clone this repository into your agent's skills directory (e.g., `.agents/skills/`).
2.  Ensure you have the following configured:
    - **Local Mode**: Requires the `mcp-obsidian` server.
    - **Generic Mode**: Requires direct access to a local folder path.
    - **Confluence**: Requires the `atlassian` MCP server.

## Usage

Simply mention the skill or use the shortcuts in your conversation with the agent:
- `@push`: Overwrite Confluence with local note.
- `@pull`: Overwrite local note with Confluence content.
- `@sync`: Perform an intelligent bidirectional sync.

## Structure
- `SKILL.md`: The core skill definition and instructions.
- `evals/`: Evaluation test cases for verification.

---
Created by Antigravity AI.
