# vibe-coding
Template to get start with vibe coding with Claude in a Dev Container.

## Setup

1. Install VS Code and Docker for multiple isolated environments for vibe coding.
2. Create a new Repository based on this repository.
3. Run `Dev Container - Reopen in Container` from the VSCode command palette.
4. Start Claude Code; Run `claude` to launch REPL.
5. Complete authentication. Follow the one-time OAuth process with your Console account. You’ll need active billing at [console.anthropic](https://console.anthropic.com/).
6. Start vibing 🎉

| Platform | VSCode Installation | Docker Installation |
|----------|----------------------|---------------------|
| Windows  | [VSCode](https://code.visualstudio.com/docs/setup/windows) | [Docker](https://docs.docker.com/desktop/setup/install/windows-install/) |
| Mac      | [VSCode](https://code.visualstudio.com/docs/setup/mac)     | [Docker](https://docs.docker.com/desktop/setup/install/mac-install/)     |
| Linux    | [VSCode](https://code.visualstudio.com/docs/setup/linux)   | [Docker](https://docs.docker.com/desktop/setup/install/linux/ubuntu/)    |


## CLI Commands

Control Claude Code with commands:

| Command                | Description                        | Example                                |
|------------------------|------------------------------------|----------------------------------------|
| `claude`               | Start interactive REPL             | `claude`                               |
| `claude "query"`       | Start REPL with initial prompt     | `claude "explain this project"`        |
| `claude -p "query"`    | Run one-off query, then exit       | `claude -p "explain this function"`    |
| `cat file \| claude -p "query"` | Process piped content            | `cat logs.txt \| claude -p "explain"`  |
| `claude config`        | Configure settings                 | `claude config set --global theme dark`|
| `claude update`        | Update to latest version           | `claude update`                        |
| `claude mcp`           | Configure Model Context Protocol servers | See MCP section in tutorials           |

CLI flags:

- --print: Print response without interactive mode
- --verbose: Enable verbose logging
- --dangerously-skip-permissions: Skip permission prompts (only in Docker containers without internet)

## Slash Commands

Control Claude’s behavior within a session:

| Command          | Purpose                                      |
|------------------|----------------------------------------------|
| /bug             | Report bugs (sends conversation to Anthropic)|
| /clear           | Clear conversation history                   |
| /compact         | Compact conversation to save context space   |
| /config          | View/modify configuration                    |
| /cost            | Show token usage statistics                  |
| /doctor          | Checks the health of your Claude Code installation |
| /help            | Get usage help                               |
| /init            | Initialize project with CLAUDE.md guide      |
| /login           | Switch Anthropic accounts                    |
| /logout          | Sign out from your Anthropic account         |
| /pr_comments     | View pull request comments                   |
| /review          | Request code review                          |
| /terminal-setup  | Install Shift+Enter key binding for newlines (iTerm2 and VSCode only) |

## Manage permissions and security

Claude Code uses a tiered permission system to balance power and safety:

| Tool Type        | Example                | Approval Required | ”Yes, don’t ask again” Behavior |
|------------------|------------------------|-------------------|---------------------------------|
| Read-only        | File reads, LS, Grep   | No                | N/A                             |
| Bash Commands    | Shell execution        | Yes               | Permanently per project directory and command |
| File Modification| Edit/write files       | Yes               | Until session end               |

## Tools available to Claude

Claude Code has access to a set of powerful tools that help it understand and modify your codebase:

| Tool               | Description                                      | Permission Required |
|--------------------|--------------------------------------------------|---------------------|
| AgentTool          | Runs a sub-agent to handle complex, multi-step tasks | No                  |
| BashTool           | Executes shell commands in your environment      | Yes                 |
| GlobTool           | Finds files based on pattern matching            | No                  |
| GrepTool           | Searches for patterns in file contents           | No                  |
| LSTool             | Lists files and directories                      | No                  |
| FileReadTool       | Reads the contents of files                      | No                  |
| FileEditTool       | Makes targeted edits to specific files           | Yes                 |
| FileWriteTool      | Creates or overwrites files                      | Yes                 |
| NotebookReadTool   | Reads and displays Jupyter notebook contents     | No                  |
| NotebookEditTool   | Modifies Jupyter notebook cells                  | Yes                 |

## Protect against prompt injection

Prompt injection is a technique where an attacker attempts to override or manipulate an AI assistant’s instructions by inserting malicious text. Claude Code includes several safeguards against these attacks:

- Permission system: Sensitive operations require explicit approval
- Context-aware analysis: Detects potentially harmful instructions by analyzing the full request
- Input sanitization: Prevents command injection by processing user inputs
- Command blocklist: Blocks risky commands that fetch arbitrary content from the web like curl and wget

Best practices for working with untrusted content:
- Review suggested commands before approval
- Avoid piping untrusted content directly to Claude
- Verify proposed changes to critical files
- Report suspicious behavior with /bug

> [!WARNING]
> While these protections significantly reduce risk, no system is completely immune to all attacks. Always maintain good security practices when working with any AI tool.

## Set up Model Context Protocol (MCP)

Model Context Protocol (MCP) is an open protocol that enables LLMs to access external tools and data sources. For more details, see the MCP documentation.

> [!WARNING]
> Use third party MCP servers at your own risk. Make sure you trust the MCP servers, and be especially careful when using MCP servers that talk to the internet, as these can expose you to prompt injection risk.

[Configure MCP servers](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/tutorials#configure-mcp-servers)