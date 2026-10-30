# Video Dubbing with AI Voices — Agent Skill

An AI-agent skill for dubbing a video episode or film into another language with **per-character AI voices**, the **original music and effects kept**, and **lip-sync fitted timing**.

The skill is a complete, battle-tested production method — not just a prompt. It was built and refined over many real dubbing jobs, and it keeps improving: after every job, new lessons are appended to the skill as method only, so the next video is better than the last.

## What it does

- Extracts timed dialogue lines (subtitle stream when available, frame OCR when subtitles are hardcoded)
- Attributes every line to its speaker using frames, dialogue meaning and pitch analysis
- Designs and **locks one voice per character** in a fixed voice map, reused across every episode of a series
- Generates clips per **dialogue turn** (one continuous clip while a character speaks — never per subtitle fragment)
- Translates in natural spoken style (Indian Hinglish by default: everyday English words stay English, script in Devanagari for correct TTS pronunciation)
- Engineers **emotion into every line** — terror, anger, grief, whispers must sound real, flat delivery is a defect
- Keeps **non-verbal emotional sounds original** — laughter, crying, screams are never synthesized and never deleted; the original sound is spliced in, level-matched
- Separates the original audio (Demucs) so the music/effects bed survives while the original dialogue is replaced
- Fits every clip to its lip-sync window, level-matches it to the original voice, and mixes dialogue clearly above the music
- Runs a **tester loop**: independent tester agents scan the finished dub with exact-second problem reports; fixes and re-tests repeat until one full pass is 100% clean

## The crew model

Every job runs four separate engineering passes so no role rubber-stamps its own work:

| Role | Owns |
| --- | --- |
| **Voice Designer** + **Voice Engineer** | Jointly design every new character's voice from a design sheet (gender, age band, build, personality, status, target pitch range), test candidates, F0-verify, lock into the voice map |
| **Voice Engineer** | Emotion and sound quality of every generated line |
| **Editor Engineer** | Final edit — trims, fades, overlaps, even loudness — and the full watch-through QC |
| **Lip-Sync Engineer** | Every dialogue point checked against the picture; a lip-sync report is a required deliverable |

## Files

- **`SKILL.md`** — the full skill: pipeline, crew roles, voice-map rules, turn grouping, translation style, emotion engineering, generation, separation, fitting, mixing, the tester loop, and all lessons learned from real jobs
- **`skill.yaml`** — skill metadata (name, title, description)

## How to use

1. Copy this folder into your agent's skills directory.
2. Point your agent at `SKILL.md` before any dubbing job — the skill's standing rule is: **read it fully before every job, apply everything in it, and append new lessons (method only) after the job.**
3. Keep one workspace per series so the locked voice map carries across episodes.

## Deliverables per episode

- The dubbed video (original picture, new dialogue, original music/effects)
- An editable pack: music-only video, full dialogue track, per-character clips, and the voice list
- Subtitle files and QA reports (tester reports + lip-sync report)

## Notes

- Dubbed timing is fitted to the picture; it is timing-accurate rather than mouth-perfect.
- Large-file delivery has real-world limits (single uploads can be cut around ~1 GiB by some networks) — the skill documents the split-into-parts workaround with checksum verification.
