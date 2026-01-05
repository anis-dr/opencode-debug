# OpenCode Debug Plugin

A debug HTTP server plugin for [OpenCode](https://github.com/opencode-ai/opencode) that enables remote debugging by exposing an endpoint to receive and log debug events.

## Installation

```bash
bun add opencode-debug
```

Add the plugin to your OpenCode configuration:

```json
{
  "plugins": ["opencode-debug"]
}
```

## Features

- **Local Debug Server** — Spin up an HTTP server to receive debug events
- **Ngrok Tunneling** — Expose your debug server publicly for remote debugging
- **Persistent Logging** — All debug events are written to `.opencode/debug.log`
- **Configurable** — Customize endpoints, paths, and auth settings

## Tools

| Tool | Description |
|------|-------------|
| `debug_start` | Start the debug HTTP server |
| `debug_stop` | Stop the debug HTTP server |
| `debug_clear` | Clear the debug log file |
| `debug_read` | Read the debug log file |

## Usage

### Starting the Debug Server

```
debug_start
```

This will output the local URL and (if ngrok is configured) a public URL.

### Sending Debug Events

From your application, send POST requests to the debug endpoint:

```javascript
fetch("http://localhost:PORT/debug", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ label: "my-event", data: { optional: "payload" } })
})
```

### Reading Logs

```
debug_read
debug_read tail=20
```

## Enabling Ngrok (Public URLs)

To expose your debug server publicly via ngrok:

1. Get an authtoken from [ngrok.com](https://dashboard.ngrok.com/get-started/your-authtoken)
2. In OpenCode, run the auth command:
   ```
   opencode auth login
   ```
3. Select **Other**
4. Select **ngrok**
5. Enter your ngrok authtoken when prompted

Alternatively, set the `NGROK_AUTHTOKEN` environment variable:

```bash
export NGROK_AUTHTOKEN=your_token_here
```

Once configured, `debug_start` will automatically create a public tunnel.

## License

MIT
