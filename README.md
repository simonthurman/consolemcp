# consoleMCP

A minimal .NET MCP server exposing two tools over stdio.

- Echo(message): returns "Hello from C#: {message}"
- ReverseEcho(message): returns the reversed message

## Prerequisites
- .NET SDK 9.0+
- Optional (for interactive testing): Node.js 18+ to run the MCP Inspector

## Build and run
From the repo root:

```bash
# restore and build
dotnet restore
dotnet build --no-restore

# run the server (stdio transport)
dotnet run --project ./consoleMCP.csproj
```
The server logs to stderr; MCP protocol messages flow over stdio.

## Test the tools (recommended: MCP Inspector)
1) Start the Inspector (no install needed):
```bash
npx @modelcontextprotocol/inspector@latest
```
2) In the Inspector UI:
- Transport: "stdio"
- Command: `dotnet run --project /absolute/path/to/consoleMCP.csproj`
- Click Connect.

3) Open the Tools panel and invoke:
- Echo with `{ "message": "test" }` → "Hello from C#: test"
- ReverseEcho with `{ "message": "stressed" }` → "desserts"

Tip: Use an absolute path in the Command so the Inspector can spawn the process reliably.

## Test using VS Code tasks
- Restore: Tasks → "restore"
- Build: Tasks → "build"
- Run: Tasks → "run" (spawns the stdio server)

To exercise the tools, connect an MCP client (e.g., the Inspector) with the same Command shown above.

## Use this in GitHub Codespaces
This repository works well in a Codespace (web or desktop VS Code connected to Codespaces):

- Open the repo in a Codespace.
- Use the built-in tasks (Terminal → Run Task…):
	- "restore" → restores packages
	- "build" → builds the project
	- "run" → starts the MCP server over stdio
- Or run directly in a terminal:

```bash
cd /workspaces/consolemcp
dotnet restore
dotnet build --no-restore
dotnet run --project ./consoleMCP.csproj
```

### Test with MCP Inspector inside Codespaces
Open a second terminal in your Codespace and launch the Inspector:

```bash
npx @modelcontextprotocol/inspector@latest
```

Codespaces will forward the Inspector port; open it from the Ports panel if prompted. In the Inspector UI:
- Transport: "stdio"
- Command: `dotnet run --project /workspaces/consolemcp/consoleMCP.csproj`
- Click Connect, then invoke the tools:
	- Echo → `{ "message": "test" }` → "Hello from C#: test"
	- ReverseEcho → `{ "message": "stressed" }` → "desserts"

Tip: The absolute project path in a Codespace is typically `/workspaces/consolemcp/consoleMCP.csproj`.

## Use GitHub Copilot to test (MCP)
If your GitHub Copilot environment includes MCP tool support, you can connect this server as a custom MCP tool:

1) Start the server via stdio command:
	 - `dotnet run --project /workspaces/consolemcp/consoleMCP.csproj`
2) In Copilot, add a custom MCP tool using that stdio command (wording varies by Copilot version/preview).
3) From Copilot Chat, call the tools:
	 - Echo with `{ "message": "from Copilot" }`
	 - ReverseEcho with `{ "message": "desserts" }` → expect "stressed"

Notes:
- Copilot’s MCP integration may be in preview or behind a feature flag. If unavailable, use the MCP Inspector steps above.
- Ensure only one server instance is running on stdio before connecting Copilot or the Inspector.

## Troubleshooting
- If the Inspector fails to connect, run `dotnet run --project ./consoleMCP.csproj` once in a terminal to ensure it builds.
- Ensure Node 18+ for the Inspector. If `npx` is unavailable, install Node.js.
- Logs go to stderr; they won’t interfere with stdio protocol payloads.
