# Voice Notes Workflow Plan (Damien & Reece)

## Overview
A Telegram-based workflow for capturing voice notes and text messages, transcribing them, structuring into summaries + actions, and storing in Google Sheets (for searchability) and Google Docs (for human-friendly reading).  

The workflow is designed for **small, testable phases**, with clear checkpoints at each step.  

---

## Key Decisions

- **Sources:** Telegram **voice notes** + **plain text** only (ignore video, files, stickers, etc.).
- **Response style:** Bot replies with **summary + actions** during testing. Later can switch to 👍/👎 replies with links.
- **Action ownership:**  
  - Default = **sender**.  
  - If the note addresses “you …” → assign to the **other person** (Damien ↔ Reece).  
- **Storage:**  
  - **Primary:** Google Sheets (tab: `VoiceNotes`).  
  - **Secondary:** Google Doc (title: `Voice Notes Log`).  
- **Tags:**  
  - **AI-generated** + optional **manual hashtags** (e.g., `#client`, `#ops`).  
  - Manual + AI tags merged, deduped, lowercased.  
- **Long note policy:**  
  - If transcript length ≥ 3 minutes → **concise mode**: max 3 bullets, all actions preserved, critical topics (clients, deadlines, money) must appear.  
  - Reply marked with “(Condensed for length)”.  
- **Dates & context awareness:**  
  - Include current date/time (`$now`) in prompt.  
  - Timezone: **Europe/Dublin**.  
  - Explicit dates normalised to `YYYY-MM-DD`.  
  - Ambiguous dates → `due=null` + note `"ambiguous date phrase: …"`.  
- **Reply toggle:**  
  - Env var `REPLY_MODE=verbose|silent`.  
  - `verbose`: summary in Telegram.  
  - `silent`: 👍 Saved + link.  
- **Privacy & retention:**  
  - Store transcripts indefinitely.  
  - No audio retention (unless added later).  
- **Multi-user ready:** store `sender` + `chat_id` for future expansion.

---

## Formatter JSON Contract

```json
{
  "summary": ["concise bullet 1", "…"],
  "action_items": [
    { "owner": "Damien|Reece|Unassigned", "task": "…", "due": "YYYY-MM-DD|null", "note": "optional" }
  ],
  "tags": ["client", "pricing", "…"],
  "decisions": ["optional decision …"]
}
```

---

## Google Sheets Schema

- `timestamp`
- `sender`
- `source` (voice|text)
- `transcript`
- `summary_bullets`
- `actions_json`
- `tags`
- `decisions`
- `chat_id`
- `debug_json` (temporary for testing)

---

## Google Doc Entry Format

```
----
[2025-09-03 14:12] Damien (voice)

Summary:
• …
• …

Actions:
• (Owner: Damien) … [due: 2025-09-05]

Tags: client, ops
```

---

## Phases

### Phase 1 — Capture & Routing
- **IF node:** `message.voice OR message.text`.  
- True → process; False → exit.  

**Tests:**
- Voice note → flows through.  
- Text → flows through.  
- Photo/sticker → ignored.  

---

### Phase 2 — Transcribe (voice path)
- Voice branch: `Telegram Get File → Download → OpenAI Transcribe`.  
- Text branch: `Set transcript = message.text`.  

**Tests:**
- 30s voice note → transcript created.  
- <3s voice note → one-liner “Too short to summarise”.  
- Text → passes through as transcript.  

---

### Phase 3 — Format
- **OpenAI Chat (JSON mode):**  
  - Strict schema.  
  - Ownership rules.  
  - Long note threshold (concise mode).  
  - Manual hashtags merged with AI tags.  
  - Token cap = 1000.  
- **Validate JSON node:** fallback if parse fails.  

**Tests:**
- “You …” → assigns to other person.  
- `#client #pricing` → tags merged.  
- >3 min note → “(Condensed for length)” + 3 bullets.  
- Always valid JSON or fallback.  

---

### Phase 4 — Storage
- **Sheets:** append row with schema.  
- **Docs:** append clean block.  
- Debug JSON stored in Sheets during testing.  

**Tests:**
- Row appears in Sheets.  
- Doc shows formatted section.  
- Debug JSON visible.  

---

### Phase 5 — Reply UX
- **IF node checks `REPLY_MODE`:**  
  - `verbose`: reply with summary + actions.  
  - `silent`: 👍 Saved + link.  

**Tests:**
- Switch env var → behaviour changes.  
- Long reply doesn’t exceed Telegram length.  
- JSON parse error → fallback warning reply.  

---

### Phase 6 — Optional Extras (scaffold only)
- **Message links:** attempt to add Telegram message URL into Sheets row.  
- **Cron digest (disabled):** daily summary → Doc + Telegram.  

---

## Error Handling
- If STT fails → reply: “⚠️ Couldn’t transcribe, stored transcript only”.  
- If JSON parse fails → fallback: summary = transcript, no actions.  
- All errors logged in Sheets.  

---

## Test Reminders
- Try `#tags` in messages.  
- Try long-winded (≥3 min) notes.  
- Try “you do X” phrasing.  
- Check fallback handling.  

---

## Defaults
- Manual tags: enabled.  
- Long note threshold: 3 minutes.  
- Actions: no cap.  
- Sheet name: `VoiceNotes (Damien & Reece)` with tab `VoiceNotes`.  
- Doc name: `Voice Notes Log (Damien & Reece)`.  
- Timezone: Europe/Dublin.  

---
