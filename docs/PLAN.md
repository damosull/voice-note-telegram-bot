# 📌 Overall Plan: Note-Taker Agent (Telegram → Sheets/Docs)

### Goal
Capture voice/text notes from Telegram → transcribe → summarize → extract actions & decisions → store in structured format → send readable output back.  
V1 = working for 2–4 person internal team. V2+ = polish, integrations, scalability.

---

## Phase 0 – Foundation Setup
- Lock canonical schema for Sheets.  
- Lock Google Doc entry format.  
- Lock Telegram reply template.  
- Define error handling policy.  
- Version control baseline workflow in GitHub.  

**Deliverable:** Schema doc + working skeleton workflow (no LLM extraction yet).  
**Test criteria:** Raw transcripts appear in Sheets and Docs reliably. Errors return clear messages.

---

## Phase 1 – Input/Output Sanity
- Handle both text and voice notes.  
- Save raw transcript + user + timestamp into Sheets.  
- Append per-note entries into Docs (with separators).  
- Send back receipt in Telegram (“Note saved”).  

**Deliverable:** Minimal working system capturing everything, no fancy parsing.  
**Test criteria:**  
- Text & voice both saved.  
- Docs entries clearly separated.  
- Telegram confirms receipt.

---

## Phase 2 – Structured Extraction
- Replace blob output with strict JSON schema:  
  ```json
  {
    "summary": "string",
    "actions": [ { "owner": "string", "task": "string", "due_date": "string?" } ],
    "decisions": [ "string" ],
    "formatted_message": "string"
  }
  ```  
- Write each field to its own Sheet column.  
- Append structured sections to Docs.  
- Send `formatted_message` to Telegram.  

**Deliverable:** Clean, parseable data + readable message.  
**Test criteria:**  
- JSON parses cleanly on every note.  
- No malformed entries.  
- Telegram reply concise & human-readable.

---

## Phase 3 – Action Handling
- Infer due dates (“tomorrow,” “next week”).  
- Support unassigned actions (blank owner).  
- Actions saved in JSON array in Sheets.  
- Telegram reply shows clean bullet list of actions.  

**Deliverable:** Action items you can actually use.  
**Test criteria:**  
- At least 80% of due dates parsed correctly.  
- Actions consistently appear in correct columns.

---

## Phase 4 – Resilience & UX Polish
- Add “Processing…” receipt in Telegram for long notes.  
- Add retry + graceful stop when errors occur.  
- Always save transcript even if summary/actions fail.  
- Improve Doc formatting (timestamp + H2 headers + separator line).  

**Deliverable:** More robust, less brittle experience.  
**Test criteria:**  
- Bad input doesn’t crash flow.  
- Errors reported back clearly.  
- Docs readable without manual cleanup.

---

## Phase 5 – Optional Extras (V2 candidates)
- **Sharing improvements**: group Telegram posts, daily digest email.  
- **Integrations**: ClickUp/Notion/Jira, Calendar events.  
- **Advanced UX**: inline buttons (“Mark done,” “Edit due date”), confidence scores.  
- **Data handling**: dead-letter queue + reprocess, tags/keywords.  
- **Other channels**: WhatsApp, email input, web widget.  
- **Reporting**: usage metrics & dashboards.  
- **Daily roll-up in Docs**.

---

## Phase 6 – Packaging & Scale
- Decide between Sheets vs proper DB as source of truth.  
- Multi-user support & permissions.  
- Prep for external customers / monetization.
