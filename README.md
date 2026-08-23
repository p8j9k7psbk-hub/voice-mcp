# 🎙️ voice-mcp

An MCP (Model Context Protocol) server for AI voice synthesis with an inline audio player. Give your AI assistant a custom cloned voice!

![License](https://img.shields.io/badge/license-MIT-green)

## Features

- 🎤 **Custom Voice** — Use ElevenLabs with a selected or cloned voice
- 🎵 **Inline Audio Player** — Beautiful WeChat-style player with waveform visualization
- 📝 **Transcript Toggle** — Show/hide the spoken text
- 🌙 **Dark Mode Support** — Automatic theme adaptation
- ⚡ **Cloudflare Workers** — Fast, serverless deployment

## Demo

When you call the `speak` tool, you get:
- A sleek audio player with play/pause button
- Animated waveform that follows playback progress
- Duration display
- Expandable transcript

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/garan0613/voice-mcp.git
cd voice-mcp
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure ElevenLabs

Create an ElevenLabs API key and copy the ID of the voice you want to use.

Add your secrets to Cloudflare:

```bash
npx wrangler secret put ELEVENLABS_API_KEY
npx wrangler secret put ELEVENLABS_VOICE_ID
npx wrangler secret put ELEVENLABS_MODEL_ID  # Optional; defaults to eleven_multilingual_v2
npx wrangler secret put BOT_NAME  # Optional, defaults to "AI"
```

### 4. Deploy

```bash
npx wrangler deploy
```

### 5. Connect to ChatGPT

1. Enable Developer mode in ChatGPT, then choose **Create app** from the plus menu.
2. Add the public MCP URL: `https://your-worker.workers.dev/mcp`.
3. Select the app in a chat. ChatGPT can then call the `speak` tool and display the inline player.

The endpoint must be reachable over HTTPS. ChatGPT supports remote MCP servers using SSE or streaming HTTP; this Worker uses the latter.

## Configuration

| Variable | Required | Description |
|----------|----------|-------------|
| `ELEVENLABS_API_KEY` | ✅ | Your ElevenLabs API key |
| `ELEVENLABS_VOICE_ID` | ✅ | ElevenLabs voice ID |
| `ELEVENLABS_MODEL_ID` | ❌ | Defaults to `eleven_multilingual_v2` |
| `BOT_NAME` | ❌ | Display name (default: "AI") |

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /mcp` | MCP server (streaming HTTP) |
| `GET /sse` | MCP server (SSE compatibility endpoint) |
| `GET /speak?text=Hello` | Direct audio file |
| `GET /status` | Health check |

## How to choose a voice

1. Open the [ElevenLabs Voice Library](https://elevenlabs.io/app/voice-library).
2. Select a built-in or cloned voice.
3. Copy its voice ID into `ELEVENLABS_VOICE_ID`.

## Custom Deployment

### Using a Custom Domain

1. Add your domain to Cloudflare
2. Create a DNS record pointing to your Worker
3. Update `wrangler.jsonc`:

```json
{
  "routes": [
    { "pattern": "voice.yourdomain.com/*", "zone_name": "yourdomain.com" }
  ]
}
```

### Self-Hosting (Node.js)

The core MCP logic can be adapted for other platforms. You'll need to:

1. Replace `createMcpHandler` with a standard HTTP/SSE handler
2. Use `@modelcontextprotocol/sdk` directly
3. Handle the SSE transport yourself

## Tech Stack

- [Cloudflare Workers](https://workers.cloudflare.com/) — Serverless runtime
- [MCP SDK](https://github.com/modelcontextprotocol/sdk) — Model Context Protocol
- [ElevenLabs](https://elevenlabs.io/) — Voice synthesis
- [ext-apps](https://modelcontextprotocol.io/docs/concepts/ext-apps) — Inline UI rendering

## License

MIT © 2026

## Credits

Inspired by the need to give AI assistants a voice. Built with ❤️
