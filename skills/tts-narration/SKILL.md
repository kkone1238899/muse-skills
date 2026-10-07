---
name: tts-narration
description: Synthesize reliable long-form narration audio with any TTS CLI. Use when the narration script is longer than ~80 characters or when a single TTS call silently truncates, drifts in pacing, or mispronounces mixed-language text.
---

# tts-narration

Produces one clean narration WAV/MP3 from a long script, with per-chunk verification so a bad chunk never silently poisons the final audio.

## Inputs

- `script`: full narration text (any length, CJK and English mixed is fine)
- `voice`: TTS voice id
- `out`: output file path (e.g. `narration.mp3`)

## Procedure

### 1. Chunk the script

Split on sentence boundaries into chunks of **at most 80 characters** (count CJK chars as 1). Never split mid-sentence; a chunk may be shorter. Number them `chunk_001`, `chunk_002`, …

Why 80: many TTS backends silently truncate or degrade on long inputs. Short chunks also isolate failures — one bad chunk gets re-synthesized instead of redoing everything.

### 2. Synthesize each chunk, then verify

For each chunk:

1. Call your TTS CLI to produce `chunk_NNN.wav`.
2. **Verify before moving on**: get the audio duration with `ffprobe`.
3. Sanity-check duration against text length:
   - CJK/mixed text runs roughly **5–10 chars/sec** → expected duration between `chars/12` and `chars/5` seconds.
   - Too short (< `chars/12`): the backend probably truncated — re-synthesize with a shorter chunk.
   - Too long (> `chars/5`): pacing drifted — apply `atempo` to bring it into range, or re-synthesize.
4. If a chunk fails verification twice, split it in half and retry the halves.

### 3. Pace and concatenate

1. Normalize loudness across chunks if your pipeline needs it (`loudnorm` or a fixed gain).
2. Concatenate in order:
   ```bash
   ffmpeg -f concat -safe 0 -i <(for f in chunk_*.wav; do echo "file '$f'"; done) -c copy narration_raw.wav
   ```
3. Optional: insert 150–250 ms silence between chunks for a natural cadence before concat (generate `silence.wav` with `anullsrc` and interleave).
4. Encode final: `ffmpeg -i narration_raw.wav -codec:a libmp3lame -b:a 128k narration.mp3`

## Failure modes

| Symptom | Cause | Fix |
|---|---|---|
| Audio ends mid-sentence | Backend truncation | Shorter chunks (≤60 chars) |
| Robotic speed-up/slow-down between chunks | Inconsistent pacing | `atempo` per chunk into the chars/sec band |
| Clicks at chunk boundaries | Hard cuts | 150 ms silence padding, or 20 ms crossfade |
| Wrong language pronunciation in mixed text | Backend language guess | Split CJK and English into separate chunks |

## Notes

- Keep chunks and the final file; debugging a narration is 10× faster when you can re-listen per chunk.
- This pattern was extracted from a production pipeline that renders daily narrated short videos — the duration check alone catches ~90% of silent TTS failures.
