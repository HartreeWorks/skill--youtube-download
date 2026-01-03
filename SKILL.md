---
name: youtube-download
description: This skill should be used when the user says "dl" followed by a YouTube URL, "download youtube", "youtube download", "yt download", "dl youtube", "download video from youtube", or provides a YouTube URL with intent to download. Triggers on patterns like "dl https://youtube.com/..." or "dl https://youtu.be/...". Handles video and audio downloads using yt-dlp.
---

# YouTube Download Skill

Download videos and audio from YouTube to the current working directory using yt-dlp.

## Prerequisites

Verify yt-dlp is installed:

```bash
which yt-dlp || brew install yt-dlp
```

## Quality Selection (Required)

**Before downloading, ALWAYS ask the user what quality they want using AskUserQuestion:**

- **Best quality** - Highest available resolution (may be large file)
- **1080p** - Full HD
- **720p** - HD (smaller file size)
- **Audio only** - MP3 audio extraction

## Download Commands

### Standard Video Download (MP4)

```bash
yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" --merge-output-format mp4 "<URL>"
```

### Audio Only (MP3)

```bash
yt-dlp -x --audio-format mp3 "<URL>"
```

### Specific Quality

```bash
# 1080p
yt-dlp -f "bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[height<=1080]" --merge-output-format mp4 "<URL>"

# 720p
yt-dlp -f "bestvideo[height<=720][ext=mp4]+bestaudio[ext=m4a]/best[height<=720]" --merge-output-format mp4 "<URL>"
```

### Custom Filename

```bash
yt-dlp -o "%(title)s.%(ext)s" "<URL>"
# Or specific name:
yt-dlp -o "my-video.%(ext)s" "<URL>"
```

## Post-Download

After download completes, report to user:
- Filename of downloaded file
- File size (use `ls -lh`)
- Location saved

## Common Options Reference

| Option | Description |
|--------|-------------|
| `-f FORMAT` | Video format selection |
| `-x` | Extract audio only |
| `--audio-format mp3` | Convert to MP3 |
| `-o TEMPLATE` | Output filename template |
| `--merge-output-format mp4` | Merge to MP4 container |
| `-v` | Verbose output |
| `--list-formats` | Show available formats |

## Troubleshooting

If download fails with format errors:

```bash
# List available formats first
yt-dlp --list-formats "<URL>"

# Then choose an available format
yt-dlp -f <format_id> "<URL>"
```


## Update check

This is a shared skill. Before executing, check `~/.claude/skills/.update-config.json`.
If `auto_check_enabled` is true and `last_checked_timestamp` is older than `check_frequency_days`,
mention: "It's been a while since skill updates were checked. Run `/check-skill-updates` to see available updates."
Do NOT perform network operations - just check the local timestamp.

