# HotLead - AI Lead Qualification Voice Agent

A voice AI agent that calls leads to verify identity and confirm interest, outputting structured qualification data.

**Team: HotLead**

---

## Market Opportunity

The lead qualification and sales automation market is massive and growing rapidly:

**Lead Management Market:**
- **$19.12 billion** in 2024, projected to reach **$44.23 billion** by 2035 (CAGR 7.92%)
- Lead Qualification segment alone: **$4.0 billion** in 2024

**CRM Lead Management Software:**
- **$10.48 billion** in 2024, projected to reach **$27 billion** by 2032 (CAGR 12.56%)

**Auto Dealership CRM Market:**
- **$6.13 billion** in 2024, projected to reach **$9.58 billion** by 2029 (CAGR 9%)

**The Problem is Real:**
- Sales reps spend **72% of their time** on unqualified leads ([source](https://www.seleqt.ai/blog/why-sales-teams-waste-time-on-unqualified-leads))
- **50-80% of leads** are unqualified ([source](https://clickback.com/press/unqualified-leads-wastes-sales-time/))
- Each failed call wastes **5+ minutes**

HotLead addresses this massive productivity gap by automating the qualification process, allowing sales teams to focus only on leads that matter.

---

## Demo Video

**[15-Second Demo Video](https://youtu.be/RzyaJbxirRg)**

---

## What is this?

Sales teams spend hours calling leads, but **around 80% have no real interest**. This wastes time and increases cost per qualified lead (CPQL).

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
- SmallWebRTC transport for local browser testing
- Pipeline architecture: STT → LLM → TTS with context aggregation
- Tool/function calling for structured output capture
- VAD (Voice Activity Detection) with Silero for turn detection
- Fast iteration: test in browser, deploy to Pipecat Cloud

### NVIDIA Nemotron (Open Weights Models)
- **STT**: Nemotron Speech Streaming (0.6B) - real-time transcription
- **LLM**: Nemotron 3 Super 120B - conversation and tool calling

**What worked well:**
- Followed complex multi-step prompts accurately
- Natural conversational tone appropriate for phone calls
- Reliable tool/function calling for capturing structured lead data
- Good at understanding context and maintaining conversation flow

**What could improve:**
- Initial latency on first response (expected with 120B model)
- Would benefit from voice-specific fine-tuning for sales scenarios
- More examples of phone/voice use cases in documentation

### Cekura (Testing & Evaluation)

**What we tested:**
- Identity verification flow ("Is this the right person?")
- Interest confirmation handling (interested vs not interested vs uncertain)
- Requirement collection accuracy
- Edge cases: wrong number, opt-out requests, uncertain responses

**Testing approach:**
- Created test scenarios simulating different lead types
- Evaluated transcript quality and conversation flow
- Measured tool calling accuracy (did AI capture correct intent?)

**Performance improvement:**
- Initial accuracy: ~60% correct intent classification
- After prompt tuning: ~85% accuracy
- Key improvements:
  - Added explicit guardrails for uncertain responses
  - Fixed prompt to avoid pushy sales language
  - Improved tool calling reliability for structured output

**Self-improvement loop:**
- Used Cekura to identify failing scenarios
- Updated system prompt based on failures
- Re-tested to verify improvements
- Iterated 3-4 times during hackathon

**Bugs found:**
- Minor: Some test runs showed duplicate transcript entries (may be our implementation issue)

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

### NVIDIA Nemotron Feedback

**What the models did well:**
- The 120B model handled complex prompts with multiple instructions and guardrails well
- Tool calling was surprisingly reliable - consistently invoked correct functions
- Natural language generation felt appropriate for phone conversations
- Good at maintaining context across multi-turn conversations

**What could be better:**
- Latency: First token latency noticeable on cold start (~2-3 seconds)
- For real-time voice, would benefit from a smaller, faster variant
- More examples of voice/phone-specific use cases in documentation
- Better guidance on prompt engineering for voice agents vs text agents

### Cekura Feedback

**What worked well:**
- Easy to connect Pipecat agent via MCP integration with Claude Code
- Test scenarios covered real-world lead qualification flows
- Transcript evaluation caught subtle issues in prompt wording
- Good visibility into conversation flow and failure points

**What could improve:**
- More predefined scenarios for B2B/sales qualification contexts
- Would love metrics on conversation latency timing (STT→LLM→TTS pipeline)
- More guidance on building systematic self-improvement workflows

**Bugs/Issues found:**
- Minor: Some test runs showed duplicate transcript entries in output (may be on our side, not Cekura)
- Occasional delay in test result feedback

**Self-improvement experience:**
- The cycle of test → identify failures → update prompt → re-test was effective
- Would have benefited from more structured guidance on what to tune first
