# The assistants and how each one connects

The web app's **Settings › Your external agent** supports these assistants. For each one it shows the exact steps,
with your server address and client ID filled in, and which of the maker's plans include custom MCP servers.

| Assistant | Maker | Where you use it | How it connects |
| --- | --- | --- | --- |
| Claude | Anthropic | Web, desktop, Cowork, mobile | Sign in with Nexa; Claude Desktop can also use a key in its settings file |
| Claude Code | Anthropic | Terminal, VS Code, JetBrains | One command |
| Codex | OpenAI | Terminal, IDE, ChatGPT desktop app | A small settings file |
| GitHub Copilot | GitHub | VS Code, terminal, coding agent | A small settings file |
| Cursor | Anysphere | Desktop | A small settings file |
| Antigravity | Google | App, IDE, terminal | A small settings file |
| Grok Build | xAI | Terminal | One command |
| Mistral Vibe | Mistral | Terminal | A small settings file |
| Gemini CLI | Google | Terminal | One command |
| Hermes Agent | Nous Research | Terminal, messaging gateway | A small settings file |

Claude on mobile uses the connector added from Claude on the web or desktop. In a Claude team, an Owner adds the
connector for everyone.

## Rules that hold for every assistant

- **The assistant's app is what you configure.** Each assistant has its own MCP settings, and configuring one does not
  configure the others. Choosing a Claude model inside Cursor still uses Cursor's settings.
- **One sign-in method per server entry.** Use either Sign in with Nexa or a key for the same entry, never both.
- **The key is the connection key.** It is the key the web app showed once, not your Nexa password, a GitHub token or
  a model provider's key. Keep it in a password manager or your system's secret store. Never paste it into a chat or
  into a file you share or commit.
- **A key in an environment variable reaches only new processes.** When a step puts the key in `NEXA_MCP_TOKEN`, set
  it where the assistant's app starts, then quit the app completely and open it again. An app already running keeps
  its old environment.
- **Other assistants.** An assistant not listed can work if its app supports authenticated remote Streamable HTTP,
  custom headers or the sign-in flow the web app offers, and MCP tool execution. Follow its maker's setup and run the
  [read-only check](README.md#4-check-without-changing-anything). A model's name alone does not establish MCP support.

## Makers' documentation

- [Claude Code MCP](https://code.claude.com/docs/en/mcp)
- [Codex MCP](https://learn.chatgpt.com/docs/extend/mcp)
- [Cursor MCP](https://cursor.com/docs/mcp)
- [VS Code MCP servers](https://code.visualstudio.com/docs/agent-customization/mcp-servers)
