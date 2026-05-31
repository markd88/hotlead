# HotLead - AI Lead Qualification Voice Agent

A voice AI agent that calls leads to verify identity and confirm interest, outputting structured qualification data.

**Team: HotLead** | Built for [YC Voice Agents Hackathon 2026](https://github.com/pipecat-ai/yc-voice-agents-hackathon)

---

## Demo Video

**[60-Second Demo Video - Link Coming Soon]**

---

## What is this?

Sales teams spend hours calling leads, but **80% have no real interest**. This wastes time and increases cost per qualified lead (CPQL).

HotLead is an AI voice agent that:
- Calls leads automatically
- Verifies identity ("Is this John?")
- Confirms interest level
- Collects requirements if interested
- Outputs structured data: **Tag + Intent Score + Transcript**

### Output Example

```json
{
  "tag": "Hot Lead",
  "intent_score": 9,
  "summary": "Interested in weekly cleaning, prefers weekends",
  "transcript": [...],
  "timestamp": "2026-05-30T10:30:00"
}
```

### Tags

| Tag | Intent Score |
|-----|--------------|
| **Hot Lead** | 9-10 |
| **Interested** | 6-8 |
| **Not Sure** | 3-5 |
| **Not Interested** | 1-2 |
| **Wrong Number** | 1 |
| **Opt Out** | 1 |

---

## Tools Used

### Pipecat (Voice Agent Framework)
- SmallWebRTC transport for local testing
- Pipeline: STT → LLM → TTS
- Tool/function calling for structured output

### NVIDIA Nemotron (Open Weights Models)
- **STT**: Nemotron Speech Streaming
- **LLM**: Nemotron 3 Super 120B

**What worked well:**
- Followed complex prompts accurately
- Natural conversational tone
- Reliable tool calling

**What could improve:**
- Initial latency on cold start
- Would benefit from voice-specific fine-tuning

### Cekura (Testing & Evaluation)
- Ran test scenarios for different lead types
- Evaluated transcript quality
- Identified edge cases in conversation flow

**Improvements made:**
- Fixed prompt to avoid sales language
- Added guardrails for uncertain responses
- Improved tool calling reliability

---

## What We Built During the Hackathon

**New for this hackathon:**

1. **Lead qualification conversation flow**
   - Greeting → Identity → Interest → Requirements → Closing

2. **Structured output system**
   - 7 tag types (Hot Lead, Interested, Not Sure, etc.)
   - Intent scoring (1-10 scale)
   - Transcript logging

3. **Tool functions for data capture**
   - `confirm_identity()` - verify caller
   - `confirm_interest()` - capture interest level
   - `collect_requirements()` - gather needs
   - `end_call()` - save results

4. **Config-driven system**
   - Lead info from JSON
   - Result output to JSON

**From starter code:**
- NVIDIA STT/LLM service wrappers
- Base Pipecat infrastructure

---

## Feedback on Tools

### NVIDIA Nemotron
- Large model handled complex prompts well
- Tool calling reliable
- Latency noticeable on cold start (expected)
- More voice-specific examples in docs would help

### Cekura
- Easy Pipecat integration via MCP
- Good test coverage for B2B scenarios
- Found duplicate transcript issue (minor bug)
- More predefined sales scenarios would be useful

---

## Live Demo

**[Coming Soon - will deploy to Pipecat Cloud]**

---

## License

BSD 2-Clause License