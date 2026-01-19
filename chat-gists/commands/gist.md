# Export Chat Transcript to GitHub Gist

Export the current conversation to a GitHub Gist for sharing or archival.

## Usage

```
/gist [--public] [github-pat]
```

**Options:**
- `--public` - Create a public gist (default: private/secret)
- `github-pat` - Optional if `GITHUB_TOKEN` environment variable is set

## Setup (one-time)

Add your GitHub PAT to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
```

The token needs the `gist` scope.

## Instructions

**Arguments:** `$ARGUMENTS`

1. **Parse arguments** for:
   - `--public` flag (if present, create a public gist)
   - An optional PAT token (any argument that starts with `ghp_` or looks like a token)

2. **Get the PAT** using this priority:
   - First, check if a PAT was passed as an argument
   - If not, read the `GITHUB_TOKEN` environment variable: `echo $GITHUB_TOKEN`
   - If neither exists, ask the user to either provide a token or set `GITHUB_TOKEN`

3. **Create a transcript** of this conversation by summarizing the key exchanges:
   - What the user asked for
   - What actions were taken
   - Key decisions made
   - Final outcomes or results

   Format the transcript as clean markdown with timestamps if available.

4. **Create the gist** using the GitHub API:

   ```bash
   curl -X POST https://api.github.com/gists \
     -H "Authorization: token <PAT>" \
     -H "Accept: application/vnd.github.v3+json" \
     -d '{
       "description": "Claude Code Chat Transcript - <date>",
       "public": <true-or-false>,
       "files": {
         "claude-chat-transcript.md": {
           "content": "<transcript-content>"
         }
       }
     }'
   ```

   **Important:**
   - Set `"public": true` if `--public` flag was provided, otherwise `"public": false`
   - Properly escape the transcript content for JSON (escape quotes, newlines, backslashes)
   - Use today's date in the description

5. **Parse the response** and provide the user with:
   - The gist URL (from `html_url` in the response)
   - Confirmation that the export succeeded
   - If there's an error, explain what went wrong (e.g., invalid token, rate limit)

## Security Notes

- The PAT is used only for this single API call
- The gist is created as **private** by default (use `--public` to change)
- Do not log or display the PAT in output
- Using `GITHUB_TOKEN` env var is preferred over passing the token as an argument
