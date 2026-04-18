# Template Instructions

When you create a new repository from this template:

## Immediate Steps

1. **Delete this file** (`.github/template-instructions.md`)

2. **Add your voice note** to `audio-samples/`
   ```bash
   # Convert to Opus if needed
   ffmpeg -i your-recording.mp3 -c:a libopus -b:a 24k audio-samples/project-idea.opus
   ```

3. **Generate transcripts** using Claude Code:
   ```
   Transcribe audio-samples/project-idea.opus using both raw and cleaned tools.
   Save to docs/transcripts/
   ```

4. **Generate development spec**:
   ```
   Based on docs/transcripts/cleaned/project-idea.md, populate the spec files
   in docs/spec/ with requirements, architecture, challenges, and stack decisions.
   ```

5. **Update CLAUDE.md** with your project-specific information

6. **Update README.md** to describe your actual project

## Files to Customize

- [ ] `README.md` - Replace with your project documentation
- [ ] `CLAUDE.md` - Fill in project-specific details
- [ ] `docs/spec/*.md` - Populate from your transcript
- [ ] `audio-samples/` - Remove example files, add your own

## Files to Keep

- `docs/claude/` - Customize agents and commands for your project
- `docs/transcripts/` - Store your generated transcripts

## Optional Cleanup

- Remove example `.opus` files from `audio-samples/` once you've tested the workflow
