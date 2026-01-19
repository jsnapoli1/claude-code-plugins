# chat-gists

A Claude Code plugin to export chat transcripts to GitHub Gists.

## Installation

```bash
/plugin install chat-gists
```

Or install from a local marketplace during development.

## Setup

### 1. Create a GitHub Personal Access Token

1. Go to [GitHub Settings > Developer settings > Personal access tokens](https://github.com/settings/tokens)
2. Click "Generate new token (classic)"
3. Select only the `gist` scope
4. Copy the generated token

### 2. Add token to your shell profile

**For zsh (default on macOS):**
```bash
echo 'export GITHUB_TOKEN="ghp_your_token_here"' >> ~/.zshrc
source ~/.zshrc
```

**For bash:**
```bash
echo 'export GITHUB_TOKEN="ghp_your_token_here"' >> ~/.bashrc
source ~/.bashrc
```

### 3. Restart Claude Code

The plugin will automatically use `GITHUB_TOKEN` when you run `/gist`.

## Usage

```
/gist [--public] [token]
```

### Examples

```bash
# Create a private gist (uses GITHUB_TOKEN env var)
/gist

# Create a public gist
/gist --public

# Pass token directly (not recommended)
/gist ghp_xxxxxxxxxxxx

# Public gist with explicit token
/gist --public ghp_xxxxxxxxxxxx
```

### Options

| Option | Description |
|--------|-------------|
| `--public` | Create a public gist instead of private/secret |
| `token` | GitHub PAT (optional if `GITHUB_TOKEN` is set) |

## What Gets Exported

The command creates a markdown transcript containing:

- What the user asked for
- Actions taken during the conversation
- Key decisions made
- Final outcomes and results

## Security

- Gists are **private by default** (use `--public` to change)
- Using the `GITHUB_TOKEN` environment variable is preferred over passing tokens as arguments
- Tokens are used only for the single API call and are not logged

## License

MIT
