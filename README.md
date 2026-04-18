# Voice-to-Spec Project Planning Template

A GitHub template for capturing project ideas via voice notes and transforming them into structured development specifications using AI.

You have a great idea but capturing it in writing feels like a bottleneck. Speaking your thoughts is faster and more natural - you can articulate challenges, edge cases, and technical requirements more fluidly than when typing.

This template provides a structured workflow for:
1. Recording a voice note with your project idea
2. Generating both verbatim and cleaned transcripts
3. Creating a development specification from your spoken requirements
4. Bootstrapping project documentation (`CLAUDE.md`, README, etc.)

## The Workflow

```
┌─────────────────┐
│  Record Voice   │
│      Note       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐     ┌──────────────────────┐
│ Gemini Audio    │────▶│  Verbatim Transcript │
│ Transcription   │     │  (raw, preserved)    │
│ MCP Server      │     └──────────────────────┘
│                 │
│                 │     ┌──────────────────────┐
│                 │────▶│  Cleaned Transcript  │
│                 │     │  (edited, formatted) │
└────────┬────────┘     └──────────┬───────────┘
         │                         │
         │                         ▼
         │              ┌──────────────────────┐
         │              │  Development Spec    │
         │              │  • Requirements      │
         │              │  • Challenges        │
         │              │  • Stack decisions   │
         │              │  • Architecture      │
         │              └──────────┬───────────┘
         │                         │
         ▼                         ▼
┌─────────────────────────────────────────────┐
│              Project Bootstrap              │
│  • CLAUDE.md (agent instructions)           │
│  • README.md (project documentation)        │
│  • /docs/spec/ (detailed specifications)    │
└─────────────────────────────────────────────┘
```

## Quick Start

### Prerequisites

1. **Claude Code CLI** with MCP support
2. **Google Gemini API key** (set as `GEMINI_API_KEY` environment variable)
3. **Audio recording capability** (phone, computer, or dedicated recorder)

### Install the Transcription MCP

Add to your Claude Code settings (user or project level):

```json
{
  "mcpServers": {
    "gemini-transcription": {
      "command": "npx",
      "args": ["-y", "gemini-transcription-mcp"],
      "env": {
        "GEMINI_API_KEY": "${GEMINI_API_KEY}"
      }
    }
  }
}
```

Or install globally:

```bash
npm install -g gemini-transcription-mcp
```

### Using This Template

1. **Use this template** on GitHub to create your new project repository

2. **Record your voice note** explaining your project idea:
   - What problem are you solving?
   - What are the key features?
   - What technical challenges do you foresee?
   - What stack/technologies are you considering?
   - Any constraints or requirements?

3. **Place the audio file** in the project root or `audio-samples/`

4. **Run Claude Code** and request transcription:

   ```
   Transcribe my-project-idea.opus using both the raw and cleaned transcription tools.
   Save the outputs to docs/transcripts/
   ```

5. **Generate the spec** from the cleaned transcript:

   ```
   Based on the cleaned transcript, create a development specification
   covering requirements, architecture, challenges, and recommended stack.
   ```

6. **Bootstrap documentation**:

   ```
   Create CLAUDE.md and update README.md based on the development spec.
   ```

## Template Structure

```
your-project/
├── audio-samples/           # Voice notes (Opus format recommended)
│   └── *.opus
├── docs/
│   ├── claude/              # Claude Code configuration
│   │   ├── agents/          # Custom agent definitions
│   │   └── commands/        # Custom slash commands
│   ├── transcripts/         # Generated transcripts
│   │   ├── verbatim/        # Raw transcriptions
│   │   └── cleaned/         # Edited transcriptions
│   └── spec/                # Development specifications
│       ├── requirements.md
│       ├── architecture.md
│       ├── challenges.md
│       └── stack.md
├── CLAUDE.md                # Agent instructions (generated)
└── README.md                # Project documentation (generated)
```

## The Transcription MCP

This workflow uses the [gemini-transcription-mcp](https://www.npmjs.com/package/gemini-transcription-mcp) server, which provides two transcription modes:

### `transcribe_audio` (Cleaned)
Returns a lightly edited transcript with:
- Filler words removed
- Verbal corrections applied
- Punctuation added
- Paragraph breaks inserted
- Metadata (title, description, timestamps)

### `transcribe_audio_raw` (Verbatim)
Returns an exact transcript:
- All filler words preserved
- False starts and repetitions included
- No cleanup or editing
- Useful for reference or when exact wording matters

Both tools accept:
- `file_path`: Absolute path to audio file
- `output_dir` (optional): Directory to save the transcript as markdown

Supported formats: MP3, WAV, OGG, FLAC, AAC, AIFF, **Opus**

## Audio Format Recommendations

For voice notes, **Opus** codec at 24-32 kbps is recommended:
- Excellent speech quality
- ~75% smaller than MP3 at 128 kbps
- Faster uploads to transcription APIs
- Wide compatibility

Convert existing files:
```bash
ffmpeg -i recording.mp3 -c:a libopus -b:a 24k recording.opus
```

## Sample Voice Notes

The `audio-samples/` directory contains real voice notes from various project ideations, provided as examples for testing the transcription workflow.

## Example Outputs

After processing a voice note about a "Local AI Transcription Tool", you might generate:

### Cleaned Transcript (excerpt)
> I want to build a local transcription tool that uses Whisper. The main challenge is getting real-time transcription working efficiently. I'm thinking of using a streaming approach where audio chunks are processed incrementally...

### Development Spec (excerpt)
```markdown
## Requirements
- Real-time speech-to-text using local Whisper model
- Streaming audio processing with incremental results
- Support for multiple audio input sources

## Technical Challenges
1. Latency optimization for real-time feedback
2. GPU memory management for continuous inference
3. Audio buffer handling and chunking strategy
```

## Integration with Claude Code

This template is designed to work seamlessly with Claude Code's features:

- **CLAUDE.md**: Generated project instructions for consistent agent behavior
- **Custom agents** in `docs/claude/agents/`: Specialized agents for your project
- **Custom commands** in `docs/claude/commands/`: Project-specific slash commands

### Using the Plugin (Recommended)

For the easiest experience, install the [gemini-transcription plugin](https://github.com/danielrosehill/Claude-Code-Plugins):

```bash
claude plugins add danielrosehill/gemini-transcription-plugin
```

This provides convenient slash commands:

| Command | Description |
|---------|-------------|
| `/transcribe <file>` | Transcribe audio (cleaned output) |
| `/transcribe-raw <file>` | Transcribe audio verbatim |
| `/voice-to-spec <file>` | Full workflow: transcribe + generate spec |
| `/generate-spec <transcript>` | Generate spec from existing transcript |
| `/bootstrap-project <name>` | Create new project with template structure |

## Contributing

Contributions welcome! This template aims to streamline the voice-to-code workflow.

## License

MIT
