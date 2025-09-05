# 📌 AI-Powered Note Taker - Simplified Plan

## Goal
Build an intelligent note-taking system that captures voice/text from Telegram, processes it with AI, and stores structured data in Google Sheets/Docs.

**Core Principle**: Use AI agents for intelligent processing, reliable nodes for infrastructure. Keep it simple.

---

## Architecture Overview

### **5-Node Workflow (Simple & Intelligent)**
1. **Telegram Trigger** - Capture messages
2. **AI Processing Agent** - Handle transcription, extraction, formatting
3. **Google Sheets Append** - Store structured data
4. **Google Docs Append** - Store formatted text
5. **Telegram Reply** - Send confirmation

### **Why This Works**
- ✅ **AI handles complexity** - transcription, action extraction, formatting
- ✅ **Reliable nodes handle infrastructure** - Telegram, Google Sheets, etc.
- ✅ **Simple to understand** - 5 nodes instead of 13
- ✅ **Easy to maintain** - clear separation of concerns
- ✅ **Actually works** - no complex JavaScript or regex

---

## Implementation Plan

### **Phase 1: Core AI-Powered System**
**Goal**: Working note-taker with AI processing

**Workflow**:
```
Telegram Message → AI Agent → Google Sheets + Docs → Telegram Reply
```

**AI Agent Prompt**:
```
You are a note-taking assistant. Process this input:

Input: {{ $json.message.text or $json.transcript }}

Extract and format:
- Raw transcript
- Actions (as JSON array)
- Formatted text for Google Docs

Return JSON:
{
  "raw_transcript": "full text",
  "actions_json": ["action1", "action2"],
  "formatted_doc_text": "formatted text with actions"
}
```

**Deliverable**: Working system that captures notes and extracts actions
**Test**: Send voice/text notes, verify actions are extracted and stored

### **Phase 2: Enhanced AI Processing**
**Goal**: More sophisticated AI processing

**Improvements**:
- Better action extraction
- Decision extraction
- Summary generation
- Due date detection

**Deliverable**: More intelligent note processing
**Test**: Complex notes with multiple actions and decisions

### **Phase 3: Advanced Features**
**Goal**: Production-ready system

**Features**:
- Error handling and retries
- Multiple chat support
- Action tracking
- Integration with task management

**Deliverable**: Robust, production-ready system
**Test**: Stress testing with various inputs

---

## Data Schema

### **Google Sheets Columns**
- `timestamp` (UTC ISO 8601)
- `sender_name` (Telegram first name)
- `chat_id`
- `note_type` ("voice" or "text")
- `raw_transcript` (string)
- `actions_json` (array)
- `decisions` (string)
- `summary` (string)
- `status` (success | error)
- `error_message` (if error)

### **Google Docs Format**
```
[Timestamp] - [Sender]

Summary:
- [AI-generated summary]

Actions:
- [action 1]
- [action 2]

Decisions:
- [decision 1]

---
```

---

## AI Agent Configuration

### **Model**: OpenAI GPT-4
### **Temperature**: 0.3 (consistent output)
### **Max Tokens**: 1000
### **System Prompt**:
```
You are a professional note-taking assistant. Extract actions, decisions, and summaries from notes. Be accurate and concise.
```

### **User Prompt Template**:
```
Process this note and extract structured data:

Note: {{ input_text }}

Extract:
1. Raw transcript (exact text)
2. Actions (array of action strings)
3. Decisions (array of decision strings)
4. Summary (brief summary)
5. Formatted text for Google Docs

Return as JSON with these exact keys:
- raw_transcript
- actions_json
- decisions
- summary
- formatted_doc_text
```

---

## Testing Strategy

### **Test Cases**
1. **Simple text note** - "I need to check the API"
2. **Complex voice note** - Multiple actions and decisions
3. **No actions** - Just informational note
4. **Error handling** - Invalid input, API failures

### **Success Criteria**
- ✅ Actions extracted correctly
- ✅ Data stored in Sheets/Docs
- ✅ Telegram reply sent
- ✅ Error handling works
- ✅ Fast response time (< 10 seconds)

---

## Future Extensions

### **Phase 4: Advanced AI Features**
- Action prioritization
- Due date extraction
- Assignee detection
- Follow-up reminders

### **Phase 5: Integrations**
- Calendar integration
- Task management tools
- Team collaboration
- Analytics dashboard

---

## Key Principles

1. **Keep it simple** - 5 nodes, not 13
2. **Let AI handle complexity** - Don't overengineer
3. **Use reliable infrastructure** - Telegram, Google Sheets
4. **Test early and often** - Validate each step
5. **Iterate quickly** - Build, test, improve

---

## Success Metrics

- **Functionality**: Actions extracted correctly 90%+ of the time
- **Performance**: Response time < 10 seconds
- **Reliability**: 99%+ uptime
- **Usability**: Easy to use, clear feedback

---

**This approach is simple, intelligent, and actually works. No overengineering, no complex JavaScript, just AI doing what it does best.**