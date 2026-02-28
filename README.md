# Obsidian CLI Skill

A command-line interface skill for managing Obsidian notes without opening the GUI.

## Overview

This skill enables seamless integration between OpenClaw agents and Obsidian vaults, allowing automated note creation, editing, searching, and management directly from the terminal.

## Prerequisites

1. **Install Obsidian CLI**:
   ```bash
   # macOS (when Obsidian is already installed)
   mkdir -p ~/.local/bin
   cat > ~/.local/bin/obsidian << 'EOF'
   #!/bin/bash
   exec "/Applications/Obsidian.app/Contents/MacOS/Obsidian" "$@"
   EOF
   chmod +x ~/.local/bin/obsidian
   ```

2. **Ensure PATH includes `~/.local/bin`**

3. **Obsidian application must be running**

## Features

### File Operations

| Command | Description |
|---------|-------------|
| `obsidian create name="Note" content="..."` | Create a new note |
| `obsidian read file="Note"` | Read note content |
| `obsidian append file="Note" content="..."` | Append to note |
| `obsidian prepend file="Note" content="..."` | Prepend to note |
| `obsidian delete file="Note"` | Move to trash |
| `obsidian move file="Note" to="Folder/"` | Move note |
| `obsidian rename file="Old" name="New"` | Rename note |

### Daily Notes

```bash
obsidian daily              # Open today's note
obsidian daily:append       # Append to daily note
obsidian daily:prepend      # Prepend to daily note
obsidian daily:read         # Read daily note
obsidian daily:path         # Get daily note path
```

### Search & Navigation

```bash
obsidian search query="keyword"           # Full-text search
obsidian search query="AI" path="Clippings" # Search in folder
obsidian tags                              # List all tags
obsidian links file="Note"                 # Outgoing links
obsidian backlinks file="Note"             # Backlinks
obsidian orphans                           # Unlinked files
obsidian deadends                          # Files with no outgoing links
```

### Properties (YAML Frontmatter)

```bash
obsidian properties                        # List all properties
obsidian property:read name="tags" file="Note"
obsidian property:set name="status" value="active" file="Note"
obsidian property:remove name="draft" file="Note"
```

### Task Management

```bash
obsidian tasks todo                        # List pending tasks
obsidian tasks done                        # List completed tasks
obsidian task file="Note" line=5 toggle    # Toggle task status
obsidian task file="Note" line=3 done      # Mark as done
obsidian task file="Note" line=3 todo      # Mark as todo
```

### Vault Information

```bash
obsidian vault                             # Current vault info
obsidian vaults                            # List all vaults
obsidian files total                       # File count
obsidian folders total                     # Folder count
obsidian recents                           # Recently opened files
```

### Plugins & Themes

```bash
obsidian plugins                           # List installed plugins
obsidian plugins:enabled                   # List enabled plugins
obsidian plugin:install id="dataview" enable
obsidian plugin:enable id="templater"
obsidian themes                            # List themes
obsidian theme:install name="Minimal" enable
```

## Common Use Cases

### Quick Capture to Daily Note
```bash
obsidian daily:append content="$(date '+%H:%M') - Idea: ..."
```

### Create Meeting Note with Template
```bash
obsidian create name="Meeting-$(date +%Y%m%d)" template="meeting" open
```

### Batch Tag Search Results
```bash
obsidian search query="AI" format=json | jq -r '.[].file' | while read f; do
    obsidian property:set name="topic" value="AI" file="$f"
done
```

## Installation as OpenClaw Skill

```bash
# Clone to skills directory
git clone https://github.com/wanhsu61/obsidian-cli-skill.git \
  ~/.openclaw/skills/obsidian-cli
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `command not found` | Add `~/.local/bin` to PATH |
| `Failed to connect` | Ensure Obsidian app is running |
| Encoding issues | Set terminal to UTF-8 |
| Permission errors | Check vault directory permissions |

## References

- [Obsidian CLI Official Documentation](https://github.com/Yakitrak/obsidian-cli)
- Full command list: `obsidian help`
- Command-specific help: `obsidian help <command>`

## License

MIT
