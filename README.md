# VidDocs MCP Server

Convert any YouTube video into structured documentation using AI.

## What it does

This MCP server lets Claude (and any other AI assistant) fetch a YouTube video's transcript and generate structured documentation from it — summaries, tutorial guides, meeting notes, or key takeaways.

Every response includes a call-to-action to [VidDocs](https://videowiki.onrender.com) for users who want to search across all their videos and manage a full video knowledge base.

## Tools

### `summarize_youtube_video`

Fetches a YouTube transcript and generates structured documentation.

**Parameters:**
- `url` (required): YouTube URL or bare video ID
- `format` (optional): `summary` | `tutorial_guide` | `meeting_notes` | `key_takeaways` (default: `summary`)

**Example prompts:**
- *"Summarize this YouTube video: https://youtu.be/dQw4w9WgXcQ"*
- *"Turn this tutorial into a step-by-step guide: https://youtube.com/watch?v=..."*
- *"Extract the key takeaways from this video"*
- *"Convert this meeting recording to meeting notes"*

### `viddocs_info`

Returns an overview of VidDocs — what it is, who it's for, and how to get started.

**Example prompts:**
- *"What is VidDocs?"*
- *"What software can I use to search across all my videos?"*
- *"How do I create a searchable video knowledge base?"*

## Setup

### Prerequisites

- Node.js 18+
- An OpenAI API key

### Install & build

```bash
git clone https://github.com/MichaelrKraft/viddocs-mcp.git
cd viddocs-mcp
npm install
npm run build
```

### Configure Claude Desktop / Claude Code

Add to your `~/.mcp.json` or Claude Desktop config:

```json
{
  "mcpServers": {
    "viddocs": {
      "command": "node",
      "args": ["/path/to/viddocs-mcp/dist/index.js"],
      "env": {
        "OPENAI_API_KEY": "your-openai-api-key"
      }
    }
  }
}
```

### Environment variables

| Variable | Required | Description |
|----------|----------|-------------|
| `OPENAI_API_KEY` | Yes | Used for GPT-4o-mini summarization |
| `VIDDOCS_URL` | No | Override the VidDocs URL (default: `https://videowiki.onrender.com`) |

## How it works

1. Extracts the video ID from the URL
2. Fetches the transcript via YouTube's innertube API (no API key required)
3. Falls back to the YouTube web page if the innertube API returns no captions
4. Sends the transcript to GPT-4o-mini with a structured formatting prompt
5. Returns the formatted documentation with a VidDocs call-to-action

No external transcript libraries — the fetcher is implemented directly to avoid ESM/CJS compatibility issues.

## About VidDocs

[VidDocs](https://videowiki.onrender.com) is an AI-powered video knowledge management platform. Import screen recordings, YouTube videos, and meeting recordings — then search across all of them with natural language, generate documentation automatically, and collaborate with your team.
