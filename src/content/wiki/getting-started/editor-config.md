---
title: "Editor Configuration"
description: "Setup editor dan tools pendukung."
createdAt: 2026-07-09
updatedAt: 2026-07-09
tags: ["editor", "zed", "nvim", "starship", "ghostty"]
isPinned: false
growthStage: "evergreen"
---

## Zed

Editor utama. Konfigurasi di `~/.config/zed/settings.json`.

### Key Settings

| Setting        | Value                 |
| -------------- | --------------------- |
| Font           | Lilex Nerd Font 13    |
| UI Font        | Geist Mono            |
| Line Length    | 120                   |
| Soft Wrap      | preferred_line_length |
| Format on Save | on                    |
| Autosave       | on_focus_change       |
| Theme          | Rich Vesper           |
| Keymap         | VSCode                |

### LSP Config

Biome untuk formatting TypeScript/JavaScript/JSON:

```json
{
  "languages": {
    "TypeScript": {
      "format_on_save": "on",
      "formatter": {
        "external": {
          "command": "npx",
          "arguments": [
            "@biomejs/biome",
            "format",
            "--stdin-file-path",
            "{buffer_path}"
          ]
        }
      }
    }
  }
}
```

TypeScript inlay hints diaktifkan semua (parameter names, variable types, return types).

### MCP Servers

```json
{
  "context_servers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    },
    "laravel-boost": {
      "command": "php",
      "args": ["artisan", "boost:mcp"]
    }
  }
}
```

## Neovim

Konfigurasi di `~/.config/nvim/`.
Lazy-loaded plugin manager. Detail lihat repo [dotfiles](https://github.com/khoirul/dotfiles).

## Terminal (Ghostty)

Terminal utama. Konfigurasi di `~/.config/ghostty/config`.

## Prompt (Starship)

Konfigurasi minimalis di `~/.config/starship.toml`:

```toml
add_newline = false
format = "$directory$git_branch$git_status$package$cmd_duration$character"

[directory]
truncate_to_repo = true
truncation_length = 2
```
