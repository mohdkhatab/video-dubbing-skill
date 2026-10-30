# Video Dubbing with AI Voices

## Standing loop — learn from this skill, then teach it (permanent rule)

- Before starting every new episode, read this whole skill and apply everything in it.
- After every job, append everything newly learned to this skill as method only — including anything the user teaches or corrects during the session, such as delivery format, mix balance, or workflow fixes — so the skill keeps getting stronger with every video.

## Run the pipeline in order

- Download the episode file first and verify it with ffprobe for duration, resolution and streams before any other work.

- Extract or build the timed subtitle lines, attribute every line to a speaker, lock the voices, translate, generate clips, separate the original audio, build the dialogue track, mix, verify and deliver.

- Keep all working files for one series in one workspace folder so the next episode reuses the same voice map, translations style and scripts.

## Crew roles — four engineers on every job

- Every dubbing job runs with four named engineering roles, each as its own separate pass (own agent or own checklist pass) so no role rubber-stamps its own work: the Voice Designer, the Voice Engineer, the Editor Engineer and the Lip-Sync Engineer.
- The Voice Designer and the Voice Engineer design every NEW character's voice TOGETHER — neither one decides a voice alone. They first write a short voice design sheet for the character: gender, age band, body build, personality, social status, energy level and the F0 (pitch) range the voice must sit in. Only then do they pick or test candidate voices against that sheet.
- The Voice Engineer owns the sound of every generated line: voice selection settings, emotion delivery, level and clarity. A line that sounds flat, wrong-aged or emotionless is the Voice Engineer's job to regenerate or fix before mixing.
- The Editor Engineer owns the final video edit: assembling the mix, trimming heads and tails, fading clip edges, removing overlaps, keeping levels consistent across the whole episode, and a final full watch-through QC of the finished file before delivery.
- The Lip-Sync Engineer owns mouth sync for the whole video: he checks every single dialogue point and confirms the dubbed clip is running exactly while that character is speaking on screen — never before the mouth opens, never after it closes, never over another character's face.

## Extract subtitles when they are hardcoded

- Check for a real subtitle stream first with ffprobe and only fall back to OCR when the subtitles are burned into the picture.

- Sample frames at 0.25 second slabs across the subtitle band, OCR them, and merge repeated reads into timed segments so even sub-second lines survive.

- Run a repair pass that re-reads single-language or broken segments at a larger scale and drops junk such as scene signs, credits and decorative text.

- Install Python packages with `pip install --target` into the workspace and set TMPDIR inside the workspace, because system pip is externally managed and the default temp space fills up on large installs.

## Attribute every line to a speaker

- Build contact sheets of mid-segment frames labeled with segment number, time and a voice pitch estimate, then read them scene by scene.

- Treat frame evidence and dialogue meaning as primary and pitch only as support, because pitch reads falsely in loud music and crowd scenes.

- Use the second subtitle language and web research to resolve names, roles and ages, and mark every uncertain attribution as an estimate instead of presenting it as certain.

- Deliver a dubbing sheet with time, character, gender, apparent age, voice type and both dialogue languages before generating voices when the user wants to check it.

## Check gender and voice matching with independent agents

- Spawn several subagents as independent checkers before mixing, and give each one a slice of the episode with its frames, speaker attributions and assigned voices.

- Have every checker confirm that each character's on-screen gender matches the gender of the assigned voice, so a male character never receives a female voice and a female character never receives a male voice.

- Have the checkers also flag wrong age feel, such as a young voice on an elderly character, and any turn attributed to the wrong speaker.

- Fix every flagged mismatch in the attribution or the voice map and regenerate the affected clips before building the dialogue track.

- Never deliver a dub that has not passed this gender and voice matching check.

## Lock voices in a fixed voice map

- Assign exactly one voice ID per character and save the mapping in a voice map file in the series workspace before generating anything.

- Reuse the same voice ID for a character in every later episode of the same series, including regenerations, so a character never changes voice mid-episode or between episodes.

- Choose each voice by the character's apparent age, gender and role, such as deep and calm for a young hero, soft and high for a young heroine and heavy and slow for an old elder.

- For every new character, the Voice Designer and Voice Engineer follow the design-sheet procedure from the crew roles section: write the design sheet (gender, age band, build, personality, status, energy, target pitch range), test-generate at least two candidate voices speaking the same test line, verify each candidate's measured pitch sits inside the sheet's range and its age feel matches the sheet, then lock the winner into the voice map. Save the design sheet in the series workspace so later episodes reuse it.

- Change a voice only when the user asks, and record the change in the voice map so the new choice stays fixed from then on.

## Group lines into dialogue turns

- Merge consecutive segments spoken by the same character into one dialogue turn and generate one clip for the whole turn, however long it runs.

- Start a new clip only when a different character begins speaking, so a character who speaks for five seconds or for a full minute gets a single clip of that full length.

- Join the turn's subtitle fragments into the complete dialogue text for that one clip, including the small pauses between fragments.

- Capture the complete dialogue of the turn with nothing missing in the middle, so the merged text covers everything the character says from the first fragment to the last.

- Keep the turn's start point at the exact moment the character starts speaking and its end point at the exact moment the character stops, so the dubbed line begins and ends in sync with the picture.

- Use the character's one locked voice model for the entire turn and never switch voice models inside a turn or between fragments of the same speech.

- Place each turn clip at the turn's start time and fit it inside the window that ends where the next turn begins.

## Translate in the user's dubbing style — Indian spoken Hinglish

- Write every dialogue the way Indian people actually talk in daily life: a natural Hindi-English mix (Hinglish), NOT pure Hindi. Pure or literary Hindi words are banned — if a bookish Hindi word exists for something but no Indian says it in real conversation, do not use it.
- Whenever an Indian speaker would naturally use the English word, use the English word: time, plan, tension, problem, ready, start, stop, phone, photo, team, level, power, attack, danger, help, sorry, thanks and hundreds like them. Everyday English words stay English, transliterated into Devanagari script so the TTS pronounces them right.
- Use only the simple, everyday Hindi words that people really speak (hai, nahi, kya, matlab, lekin, bahut, jaldi, theek hai type words). If a line starts sounding like a Hindi textbook or a news reader, rewrite it the way a friend would say it.
- Keep genre and technical English words that sound wrong when translated, such as barrier, sword, talisman, formation, secret chamber, cultivation and rank names, transliterated in Devanagari.
- The written script stays Devanagari for correct TTS pronunciation, but the LANGUAGE is Indian Hinglish — spoken style first, always.

## Emotion engineering — every line must sound like the scene

- Tag every dialogue turn with the emotion of its scene when translating: calm, happy, angry, threat, shock, grief, whisper, fear/panic, or shout. Save the tag with the turn so generation and testing can check against it.
- The Voice Engineer makes the emotion audible in the speech itself, not just the words: a character in danger screaming for help must sound terrified and desperate (repeated cries, broken phrasing, exclamation punctuation), an angry threat must bite, a whisper must drop, grief must slow down. Flat, neutral delivery on an emotional scene is a defect.
- Drive the emotion through word choice, punctuation, repetition and phrasing in the written line, and regenerate any take that comes back flat until the delivery matches the tag.
- Testers check emotion match as their own item: for every emotional turn they compare the dubbed delivery against the original scene's intensity and flag any turn where the feeling is missing or weaker than the picture.

## Generate the clips

- Synthesize with `/opt/hatch/bin/tts speak --output <file> --voice <voice ID> --language <code> --text-stdin`, passing the line through stdin and using the voice ID from the voice map.

- Name every clip file with its start and end time and keep one folder per character, and also deliver a voice list that shows character name, time range and line text grouped by character.

- Run generation with a few parallel workers and skip clips whose file already exists with a healthy size, so a rerun only fills gaps.

- Retry an ultra short line such as a single name when synthesis returns empty audio, and if it still fails, reuse the identical take of the same word from the split fragment of the same utterance.

- Reconcile counts before mixing by confirming the number of generated clips equals the number of dialogue turns, and investigate any mismatch because an index offset once hid a missing line behind another line's success.

## Remove the original dialogue and keep the music

- Separate the original audio into vocals and accompaniment with Demucs using `python3 -m demucs --two-stems=vocals -n htdemucs`, and keep the no-vocals stem as the music and effects bed.

- Install torch and torchaudio from the CPU wheel index first, then install demucs with its remaining small dependencies, because the default install pulls several gigabytes of GPU wheels that this machine cannot use.

- Add the `sphn` package when demucs fails to import, because current demucs versions need it for audio input and output.

- Verify the separation by comparing RMS level at a dialogue-heavy window before and after, where the music bed should sit clearly lower than the original mix while music-only stretches stay close.

## Fit lip sync and build the track

- Measure each generated clip and compare its duration with its turn window before placing it.

- Speed a clip up with the ffmpeg atempo filter up to about 1.45 times when it overflows its window, and leave shorter clips at natural speed because a line ending early looks natural.

- Trim with a short fade out only when a clip still overflows at maximum speed, and never let a clip spill into the next turn's speech.

- Assemble all fitted clips onto a silent timeline at their turn start times and peak-normalize the finished dialogue track.

- Prefer the turn grouping above when a line does not fit, because a full turn window gives far more room than a single subtitle fragment and removes most trimming.

- After the track is built, the Lip-Sync Engineer checks EVERY dialogue point in the video, one turn at a time: the dubbed speech must start when the character's mouth opens (within about 0.3 seconds) and end when the character stops speaking. Speech running over a closed mouth, over a reaction shot of another character, or starting noticeably after the mouth moves is a lip-sync defect.
- Fix lip-sync defects by shifting the clip's offset inside its turn window first, then by re-fitting its speed, and only as a last resort by re-trimming the text. Record every defect with its exact second in the lip-sync report, and re-check the fixed points until the whole episode passes.
- The lip-sync report is a required deliverable of the QA stage, alongside the tester reports — no episode is delivered without it.

## Mix, verify and deliver

- Mix the music bed at about 0.9 volume with the dialogue track at full volume using amix with normalize off, then master with loudnorm at about minus 15 LUFS and a true peak of minus 1.5.

- Mux the finished audio with the original video stream copied, and confirm the output duration matches the source duration exactly.

- Spot-check the final video by comparing RMS level against the music-only version at several dialogue windows, where the dubbed version should measure clearly louder.

- Before delivery, the Editor Engineer runs the final edit pass on the finished video: check head and tail trims, fade any hard clip edges, remove any remaining overlap, confirm loudness feels even across the whole episode (no scene suddenly louder or softer), and watch the full video start to finish as the last QC gate. Anything the Editor Engineer catches goes back into the fix loop before delivery.

- Deliver the dubbed video plus an editable pack that contains the music-only video, the full dialogue track, the per-character clips and the voice list, so the user can import everything into a video editor and adjust it.

- Report honestly that lip sync is timing fitted rather than mouth perfect, and that faint traces of the original voice can survive separation in the loudest scenes.

## Non-verbal emotional sounds stay original — never synthesize them

- Dub ONLY spoken dialogue with synthesis. Non-verbal emotional sounds — laughing, crying, sobbing, screaming, gasping, groaning, sighing — cannot be produced convincingly by the synthesis voices. Never fake them with a synthesized attempt, and never leave them as silence either.
- When a character laughs, cries or makes any such sound in the original, keep the ORIGINAL sound: cut that exact moment from the separated original vocal stem and splice it into the dialogue track at its exact original timing, level-matched to the neighbouring dubbed turns, so it sits naturally in the final mix and matches the emotion on screen.
- Detect these moments from the vocal-stem speech-energy blocks that fall outside dialogue turns, and from subtitle cues that are purely non-verbal; classify every such block (speech vs laugh vs cry vs other non-verbal) before mixing, and carry every confirmed laugh/cry/scream over from the original stem.
- When one turn mixes speech with laughter or crying, dub the spoken words and keep the original non-verbal part, crossfading cleanly at the boundary so the voice and the emotion still feel like one performance.

## Test the finished dub in a fix and recheck loop

- After the full dub is mixed, spawn multiple tester agents and assign each one a part of the episode to test.

- Have every tester check its part for missing dialogues, wrong speaker or wrong voice, gender mismatches, bad timing, lip sync drift, leftover original voice, emotion that does not match the scene, and any audio problem. The Lip-Sync Engineer's full-episode lip-sync report and the Editor Engineer's final edit pass are part of this same loop, not separate extras.

- Have the testers write their findings into a markdown report file that names the exact second of every problem and what is wrong there.

- Read all reports as the main agent, locate each problem by its second, and fix it in the clips, the track or the mix.

- Run the tester agents again on the fixed parts after every fix, then fix and recheck again in the same loop.

- Keep the loop running until one full pass over the whole episode reports no problems, and deliver only after that clean pass.

## Lessons from later jobs

- Keep the dubbed dialogue clearly above the music bed in the final mix. A first mix that matched the original balance still felt voice-low to the listener, so the music bed was dropped and the dialogue gain raised until dialogue scenes measured about +9 to +16 dB over the bed. When in doubt, bias toward louder dialogue.


- When an episode's OCR pass is slow, split the frame range into two halves and run two OCR processes in parallel on separate frame ranges, then merge the segment lists with a seam check that merges identical text across the cut. On a 2-core box this roughly halves the wall time.
- Run the vocal separation in chunks of about 350 seconds and concatenate the music beds afterwards, saving each chunk separately. A single full-length separation run can die near the end (memory peak at save time, or a runtime restart) and lose everything, while finished chunks survive.
- After fitting, level-match every placed clip to the original vocal stem: scale each clip so its RMS equals the original voice RMS in the same window, with the gain clamped between 0.4x and 4.0x. Raw synthesis levels vary a lot per voice, and without this step many lines sit 5 to 10 dB under the original mix balance.
- Judge automated test reports before fixing them. Testers that measure the raw fitted clips report level errors that the track builder's gain step already corrected, and pitch checks on one-second shouted clips flag the correct voice because shouting raises the measured pitch. Verify flagged items against the final mixed track before regenerating anything.
- When several similar-looking elders or officials share a scene, expect attribution disagreements between checkers. Settle them with full-resolution frames of the exact seconds in dispute, and unify a recurring speaker's scattered lines under one voice once the face is confirmed.
- Join separation chunks with the ffmpeg concat DEMUXER and a file list, never the concat protocol: with WAV chunks the protocol silently keeps only the first chunk, which quietly truncates the bed and the vocal stem to one chunk's length. Always verify the joined files' total duration against the source before building the track.
- Level-matching to the original vocal stem still buries lines whose original was a quiet interjection under a loud bed (fight climaxes, crowd and market swells, ending-music swells). After the first mix, measure every turn's speech-frame dominance over the scaled bed in the FINAL mix; any turn under about +3 dB gets a targeted gain boost on the dialogue track inside its fitted window (+5 to +11 dB by deficit, 30 ms edge fades), then remix and re-test only the flagged turns.
- On loud, action-heavy episodes the pitch tracker jumps octaves (female-range readings on male shouts). Use an octave-robust F0 (median of the low candidates, or a second pass that folds readings above ~200 Hz down an octave before judging), and let frames plus dialogue logic overrule pitch whenever they disagree.
- A young male character with soft, feminine features can look female at contact-sheet size. Whenever gender is ambiguous on the sheet, decide from full-resolution frames of the exact seconds before locking a voice.
- A shouted phrase split across two adjacent OCR segments with identical text is often ONE speaker's continuous shout, not two speakers. Check continuity before assigning two different voices to the two halves.
- At an ambush-reveal moment, a short "come out" line can belong to the calm protagonist summoning the hidden attackers rather than to the loud newcomer. Decide from the close-up frame (whose mouth and face it is) and from who reacts with surprise in the next line.
- For file delivery from this sandbox, a guest upload to GoFile works (fetch a store server from its API, POST the file as multipart form data), and its response returns an md5 — always compare it with the local file's md5 before sharing the download link.
- This sandbox's egress cuts any single upload at about 1 GiB transferred (connection reset at the same byte count on multiple servers). Files bigger than that cannot go up in one POST no matter the host: split the master at scene/keyframe boundaries into playable parts safely under the wall (about 800 MB each), upload and md5-verify each part, and deliver the parts in order. A re-encode small enough to fit is the fallback when parts are unacceptable, but it costs hours and quality.
- Mind the chunk-naming trap when splitting audio for separation: ffmpeg's segment muxer names its outputs chunk0, chunk1, chunk2 while a separation script may expect chunk1, chunk2, chunk3. After every separation run, list the output folders and reconcile them against the source timeline before joining; a silent off-by-one leaves the first minutes unseparated.
- Synthesis clips carry up to ~0.7 s of padding silence at their edges. Strip leading and trailing silence from every generated clip (conservative threshold around -45 dB, keep ~50 ms) BEFORE fitting: padding inflates the apparent clip length, forces unnecessary speed-ups, and can push borderline turns into content-losing trims. After stripping, re-audit which turns would be trimmed.
- Judge a turn's fit against the room up to the NEXT turn's start, not against its own subtitle window — subtitle windows end at the last fragment while the placement room includes the following pause. Most apparent "heavy speed" flags disappear under the correct room; the real defect classes are only trims (clip still too long at the speed cap) and end-position errors versus the ORIGINAL speech.
- The original can contain audible speech with NO subtitle at all (arrival announcements, an approaching villain's laugh, taunts over a reaction shot) — OCR never sees it and the segment list looks complete. Detect it by scanning the separated original vocal stem for speech-energy blocks that fall outside every turn; identify the speaker from frames plus pitch, and patch the gap with a short line in that character's locked voice, level-matched to a neighbouring turn. A steady, unbroken energy block with no syllabic dips is a laugh or a sustained shout, not a sentence.
- When a turn's dubbed speech must end at a precise point (it badly overruns the original speech end, or ends long before it), give the track builder a per-turn target-end override: fit speed = clip duration / (target end - start). If one off-screen reaction line needs a speed above the global cap to keep all its words, raise the cap for that single turn instead of trimming its tail.
- A single-word or two-word synthesis input can fail repeatedly with an "empty/truncated audio" backend error while longer lines with the same voice succeed. Pad the shout minimally and naturally (a leading spoken particle), then trim the particle off the finished clip so the placed audio is exactly the scripted word.
- Hinglish synthesis runs slower than the original language — a literal translation typically needs 1.5-1.7x the original's time. Write translations tight the first time, and when a tester flags an early-ending turn, lengthen the text (or slow-fit) rather than leaving silence under a still-speaking mouth; when it flags an overrun, shorten the text before raising speed.

- A source page can carry official multi-language subtitle tracks even when the video it serves is paywalled or only a short preview. Always list and download the subtitle tracks first; they may belong to the full-length cut while the served video is minutes long. Verify the cue span against the file you will actually dub before building segments from them.
- Some streaming mirrors expose a REST endpoint that returns base64-encoded player page URLs, and the player page carries its config as base64 of XOR-obfuscated JSON (the key sits in the page's own script) containing a direct HLS master playlist. When a direct ffmpeg HLS download stalls behind a proxy, fetch the playlist's segments with parallel curl (skip-existing, so it resumes), then concatenate and remux.
- Synthesis output is deterministic per voice plus text: regenerating the same line returns the identical take with the identical pitch. Never re-roll hoping for a different performance; change the text or fix the take in post.
- A locked voice can drift far above its design pitch on shouted or panicked lines (a 136 Hz male voice measured 186-273 Hz on shouts and read as female). After fitting, F0-verify EVERY turn of expressive characters, not just samples; fix drift with a duration-preserving pitch shift (rubberband) down to the design target — the voice identity stays, the gender read is corrected.
- On very long files (around 90 minutes), never hold several full-length float32 stereo arrays in memory at once — that gets the mixer killed on an 8 GB machine. Stream the mix in blocks, and take measurement slices through a memory map.
- Long detached jobs die when the runtime or VM restarts mid-job. Give every long job a skip-guard (skip any chunk whose output already exists) so a relaunch resumes where it stopped; confirm liveness from fresh log lines and output files, not from the launch having succeeded.
- Extracted frame folders can be time-shifted against the segment times on long files (observed about +11 s). Verify frame timing against burned-in captions or an independent subtitle track before trusting frame-based lip-sync or attribution checks near boundaries.
- The original mix itself may bury dialogue under effects: measure the original vocal stem's dominance before judging the dub (climax turns can sit at -5 to -20 dB in the original). When the +3 dB target is unreachable by dialogue boosts alone (peak headroom), combine moderate dialogue boosts with dipping the bed a few dB inside the worst windows; a few turns near 0 dB against the loudest effects are acceptable when they sit far above the original balance.
- Cross-check suspected missed speech against an independent subtitle source for the same file (for example a fansub track): if a suspect block overlaps no cue in either track and shows no syllabic structure, it is non-verbal (score vocalise, groans, effects), not a missed line.

## Keep improving this skill

- Append each new lesson, problem and fix from a dubbing job to this skill right after the job, written as method only with no episode names, show names or user details.

- Try to raise quality on every new episode through better fitting, better emotion in the lines and better mixes, while keeping every approved character voice identical.

- Read this skill and the series voice map before starting any new dubbing job so earlier lessons and locked voices carry over.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.
