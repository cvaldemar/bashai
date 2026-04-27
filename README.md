# bashai

Describe a shell command in plain English, get back the command.

Supports OpenRouter (default), Ollama, and LM Studio.

Disclaimer: this was vibe coded in 5 minutes. Use with caution and review commands before running them.

## Dependencies

- bash
- curl
- jq (preferred) or python3 (for JSON parsing)
- One of: xclip, xsel, pbcopy, wl-copy (for clipboard support)

## Install

    curl -o ~/.local/bin/bashai https://raw.githubusercontent.com/cvaldemar/bashai/main/bashai
    chmod +x ~/.local/bin/bashai

## Usage

    bashai <describe the command you want>

Examples:

    bashai ffmpeg all mp4s in this folder to mp3 audio only
    bashai find all files larger than 100MB
    bashai compress this directory into a tar.gz

The command is printed to stdout and copied to your clipboard.

## Configuration

All configuration is done via environment variables.

    OPENROUTER_API_KEY   Required when using OpenRouter.
    BASHAI_API_URL       API endpoint to use. Default: https://openrouter.ai/api/v1/chat/completions
    BASHAI_MODEL         Model to use. Default: openai/gpt-4o-mini
    BASHAI_DANGER        Set to true to auto-execute the returned command. Default: false
    BASHAI_CLIPBOARD     Set to false to skip copying to clipboard. Default: true

Optionally add an alias to your .bashrc

    alias ai='bashai'

## Local LLMs

Ollama:

    BASHAI_API_URL=http://localhost:11434/v1/chat/completions BASHAI_MODEL=llama3 bashai <prompt>

LM Studio:

    BASHAI_API_URL=http://localhost:1234/v1/chat/completions BASHAI_MODEL=local-model bashai <prompt>

## Auto-execute

When BASHAI_DANGER is set to true, the script will execute the returned command via eval.
You take full responsibility for what runs.

    BASHAI_DANGER=true bashai <prompt>

## Security

The system prompt instructs the model to:

- Return only the raw command, no explanation or markdown
- Avoid destructive operations without confirmation flags
- Avoid exfiltrating data or opening network ports unless explicitly asked
- Avoid touching system files outside the current directory unless explicitly asked
- Return "# unsafe request" if the prompt cannot be fulfilled safely

This is a best-effort guardrail. The model can still produce harmful commands.
Always read the output before running it.
