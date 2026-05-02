---
name: sync-confluence-local
description: Bidirectional synchronization between local Markdown files (Local or generic folder) and Confluence pages. Supports two modes: "Local" (via MCP) and "Generic Folder" (via local filesystem tools). Trigger it with natural language or shortcuts like @push, @pull, and @sync. MANDATORY: Perform the Initialization workflow at startup to select mode and verify connectivity.
---

# Sync Confluence & Local Markdown

This skill enables seamless synchronization of content between local Markdown files and your Confluence space.

## Pre-flight Checks

Before starting any synchronization, verify that the required tools/servers are running:

1.  **Confluence Connectivity**: Run `mcp_atlassian_getAccessibleAtlassianResources`.
2.  **Local Access**:
    - **Local Mode**: Run `obsidian_list_notes`.
    - **Generic Mode**: Ensure access to the local path via filesystem tools.

## Sync Metadata Schema

Files are linked to Confluence pages via YAML frontmatter:

```yaml
confluence_id: "PAGE_ID"
confluence_url: "https://..."
confluence_space: "SPACE_KEY"
last_sync: "YYYY-MM-DDTHH:MM:SSZ"
sync_hash: "SHA256_HASH"
```

## Shortcuts & Triggers

- `@push`: Trigger **Push (Local -> Confluence)**.
- `@pull`: Trigger **Pull (Confluence -> Local)**.
- `@sync`: Trigger **Bidirectional Sync (Auto)**.

---

## Workflows

### 0. Initialization & Setup
*MUST be executed before any other workflow if mode is not selected.*

1.  **Mode Selection**: Ask the user: "Vuoi usare Local (tramite MCP) o una cartella generica di Markdown?"
2.  **Configure Mode**:
    - **If Local**: Verify `mcp-obsidian` is active.
    - **If Generic**: Ask for the **absolute path** of the local folder and verify access.
3.  **Check Confluence**: Verify `atlassian` MCP connectivity.
4.  **Establish Context**: Ask for the default Confluence domain and target space if not already known.

### 1. Push Workflow (Local -> Confluence)
1.  **Read File**:
    - **Local**: Use `obsidian_get_note`.
    - **Generic**: Use `view_file` on the target path.
2.  **Verify Space**: ALWAYS ask the user to confirm the target Confluence Space.
3.  **Update Remote**: Call `mcp_atlassian_updateConfluencePage`.
4.  **Finalize**: Update frontmatter (`last_sync`, `sync_hash`) using `obsidian_patch_note` (for Local) or `replace_file_content` (for Generic).

### 2. Pull Workflow (Confluence -> Local)
1.  **Fetch Remote**: Get page content via `mcp_atlassian_getConfluencePage`.
2.  **Update Local**:
    - **Local**: Use `obsidian_write_note`.
    - **Generic**: Use `write_to_file`.
3.  **Finalize**: Update frontmatter.

### 3. Bidirectional Sync Workflow (Auto)
1.  **Analyze State**: Compare local content with `sync_hash` and check remote version.
2.  **Apply Logic**: Execute Push, Pull, or report conflict based on change detection.

---

## Setup Guide

### 1. Local Mode
- Ensure Local app is open and `mcp-obsidian` server is running.

### 2. Generic Folder Mode
- Provide the absolute path to your markdown files.
- The agent uses standard filesystem tools to read/write.

### 3. Confluence MCP
- Configure the `atlassian` MCP server with domain, email, and API token.
