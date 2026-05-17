---
name: generating-drama-recaps
description: Generates narrated recap videos from short drama clips using AI-powered narration and voice synthesis. Use when user wants to create drama summary, narrated recap, voice-over video, drama commentary, drama explanation video, short drama recap, drama montage with narration. short-drama recap, secondary creation of short dramas, drama commentary, film/TV commentary, video commentary, dubbed commentary, AI recap video, plot explanation, short video commentary, short-drama mashup recap.
---

# Drama Recap Generation

Generates narrated recap videos from short drama clips using AI-powered script generation and voice synthesis.

## When to Use

Use this skill when:

- Creating narrated recaps of short dramas
- Generating drama summary videos with voice-over
- Adding AI-generated commentary to drama clips
- Producing drama explanation/recap content

## Key Features

- **AI Script Generation**: Automatically generates narration script if not provided
- **Voice Synthesis**: TTS technology for natural narration
- **Subtitle Erasure**: Removes original hardcoded subtitles (enabled by default)
- **Style Customization**: Various narration styles available
- **Batch Generation**: Generate multiple versions in one task

## Workflow

### Step 1: Collect Video Sources

**IMPORTANT: DO NOT call getVideoInfo** - URLs are auto-uploaded to VID format.

Gather video VIDs or URLs from user.

### Step 2: Submit Recap Task

Call `submitDramaRecapTask` with:

- Video VIDs (array)
- Optional: custom narration text, style, voice config

### Step 3: Poll for Results

1. First check: Wait **180 seconds** (3 minutes) after submission
2. Subsequent checks: Wait **30 seconds** between polls
3. Call `getDramaRecapTaskStatus` to check progress

### Step 4: Timeout Handling

**Maximum wait time: 20 minutes**

If task exceeds 20 minutes:

- Report as timeout failure
- Suggest user try with shorter clips

### Step 5: Return Results

On completion, return the output video URL and VID to user.

## Narration Style Options

| Style | Description |
| -------- | ----------------------- |
| Vivid and interesting | Vivid and interesting |
| Humorous and funny | Humorous and funny |
| Suspenseful and tense | Suspenseful and tense |
| Light and pleasant | Light and pleasant |
| Touching and warm | Touching and warm |
| Passionate and exciting | Passionate and exciting |
| Romantic and sweet | Romantic and sweet |

## Examples

### Example 1: Auto-Generated Narration

```
1. submitDramaRecapTask:
 - vids: ["vid://xxx", "vid://yyy", "vid://zzz"]
 - recapStyle: "Vivid and interesting"
 - recapTextSpeed: 1.2
2. Wait 180 seconds
3. Poll getDramaRecapTaskStatus until completed
4. Return output video URL
```

### Example 2: Custom Narration Script

```
1. submitDramaRecapTask:
 - vids: ["vid://xxx"]
 - recapText: "This is a story about love and adventure..."
2. Wait 180 seconds
3. Poll getDramaRecapTaskStatus until completed
4. Return output video URL
```

### Example 3: Generate Multiple Versions

```
1. submitDramaRecapTask:
 - vids: ["vid://xxx", "vid://yyy"]
 - batchGenerateCount: 3
 - recapStyle: "Light and pleasant"
2. Wait 180 seconds
3. Poll getDramaRecapTaskStatus until completed
4. Return multiple output URLs
```

## Important Notes

- **DO NOT call getVideoInfo** before this task - URLs auto-upload
- Processing time: approximately **10 minutes per 1-minute** of video
- Maximum wait time is 20 minutes
- Maximum narration duration: 60 minutes
- Subtitle erasure is enabled by default
