# 📌 Phase 0 Spec – Foundation Contract

## Sheets Schema (canonical store)
Columns:  
- `timestamp` (UTC ISO 8601)  
- `sender_name` (Telegram first name)  
- `chat_id`  
- `note_type` (“voice” or “text”)  
- `raw_transcript` (string)  
- `summary` (string, may be blank at this stage)  
- `actions_json` (array, may be blank)  
- `decisions` (string, may be blank)  
- `formatted_message` (string, may be blank for now)  
- `status` (success | error)  
- `error_message` (if status=error)

---

## Google Doc Format (per note entry)
```
[Timestamp UTC] – [Sender]

Summary:
- <summary text>

Actions:
- <action 1>
- <action 2>

Decisions:
- <decision 1>

---
```

---

## Telegram Reply Template (v1)
```
Note saved.
Transcript captured and logged.
```
(Phase 2 will replace this with the full formatted message.)

---

## Error Handling Rules
- **Transcription fails** → Telegram reply:  
  `Error: could not transcribe note. Please try again.`  
  Stop workflow.  

- **Extraction fails** → save raw transcript to Sheets/Docs.  
  Telegram reply:  
  `Note saved, but summary/actions could not be extracted.`  

- **System errors** (API down, quota) → generic error reply.  
  Log details in `error_message` column.

---

## Versioning
- Export workflow JSON after every change.  
- Commit to GitHub with message: `phaseX_stepY_description.json`.  
- Store spec docs (`PLAN.md`, `PHASE0.md`) alongside workflow JSONs.  

---

✅ Phase 0 defines the ground rules. Once locked, we move to Phase 1 knowing structure, formatting, and error paths are frozen.  
