# pi-inspect-image

A pi extension that analyzes images using a separate vision-capable model — independent of your main chat model. This enables vision capabilities for non-vision models (e.g. Haiku, Sonnet) via tool calls to a selected vision model.

**When to use it:**
- Your primary chat model doesn't support vision — route image analysis to a dedicated model.
- You want to keep large image payloads out of your main conversation context — only the derived description enters your chat.
- You need specialized image tasks: describing screenshots, extracting text via OCR, analyzing UI mockups, reading diagrams or charts, debugging visual layout issues, or inspecting error screenshots.
- You're working with visual artifacts (designs, photos, graphs) and want pi to understand them before generating code.

Supported providers: **OpenAI**, **OpenRouter**, and any OpenAI-compatible API.

## Setup

Run `/setup-vision` for a 2-step interactive configuration:

```
/setup-vision
```

1. **Pick a provider** — auto-populated from providers you've authenticated via `/login` that have vision models.
2. **Enter the model ID** — e.g. `gpt-4o`, `openai/gpt-4o`, or any model available on your provider.

The extension validates your entry against the model registry and warns if the model isn't recognized or lacks vision support (both are advisory — the call will still be attempted). Configuration is saved to `.pi/settings.json`.

You can also configure manually:

```json
{
  "visionConfig": {
    "provider": "openai",
    "model": "gpt-4o"
  }
}
```

- **Project settings** (`.pi/settings.json`) — shared with your team, set via `/setup-vision`.
- **Global settings** (`~/.pi/agent/settings.json`) — personal, edit manually.

### API keys

Configure via `/login` or set the appropriate environment variable:

| Provider | Env var |
|----------|---------|
| OpenAI | `OPENAI_API_KEY` |
| OpenRouter | `OPENROUTER_API_KEY` |
| Custom | Any — handled by `/login` or `auth.json` |

## Configuration

| Field | Required | Description |
|-------|----------|-------------|
| `provider` | Yes | `"openai"`, `"openrouter"`, or any custom provider name |
| `model` | Yes | Model ID (e.g. `"gpt-4o"`, `"openai/gpt-4o"`) |
| `baseUrl` | No | Custom API base URL for compatible providers (auto-resolved for OpenAI/OpenRouter) |
| `maxTokens` | No | Max tokens in response (default: 4096) |

## Supported Image Formats

PNG, JPEG, GIF, WebP, BMP — up to 20 MB.

## Usage

The extension registers an `inspect_image` tool. Once configured, ask pi to describe an image and it will use this tool automatically.

You can also pass a custom prompt to guide the analysis:

```
inspect_image("screenshot.png", "Extract all text visible in this image")
```

If no vision model is configured when the tool runs, you'll be guided through setup on the spot.

## License

MIT
