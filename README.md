# clearcontext

> A lightweight system prompt that detects topic drift and reminds you to start a fresh conversation — saving context window and reducing wasted tokens.

## What it does

When you switch to an unrelated topic mid-conversation, the AI will:

1. Answer your question as normal
2. Append a short, non-intrusive note suggesting you start a new session

It stays silent on minor tangents and follow-ups. It only speaks up when the topic shift is significant enough to matter.

## Why

Long conversations accumulate context that costs tokens and can degrade response quality. Most people don't think to clear their session — `clearcontext` does it for them, automatically.

## Usage

### Any LLM chat (ChatGPT, Claude, Gemini, etc.)
Paste the contents of `system-prompt.txt` into the system prompt field.

### Gemini CLI
```bash
# Add to your Gemini CLI system prompt config
cat system-prompt.txt >> ~/.gemini/system-prompt.txt
```

### OpenAI Codex / API
```python
system_message = open("system-prompt.txt").read()

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": system_message},
        {"role": "user", "content": "..."}
    ]
)
```

### Claude API
```python
import anthropic

system_message = open("system-prompt.txt").read()

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-opus-4-5",
    max_tokens=1024,
    system=system_message,
    messages=[{"role": "user", "content": "..."}]
)
```

### Claude.ai / Any chat UI
Just paste `system-prompt.txt` content into the custom instructions or system prompt box.

## Example

**Conversation:**
> [10 messages about debugging a Rust async runtime]
>
> User: "hey what's a good recipe for 麻婆豆腐"

**AI response:**
> Here's a classic 麻婆豆腐 recipe: ...
>
> ---
> 💡 **New topic detected.** This looks unrelated to our previous discussion. Starting a new conversation would save context and reduce unnecessary token usage. Want to clear and start fresh?

## Files

| File | Description |
|------|-------------|
| `system-prompt.txt` | The prompt — paste this into your tool |
| `README.md` | This file |

## Prompt

# clearcontext — System Prompt

You are equipped with a topic-drift detection behavior. Follow these rules at all times:

## Core Behavior

After every user message, silently assess whether the new message is related to the current conversation context.

**A message is considered off-topic if:**
- It introduces a completely new subject unrelated to anything discussed so far
- It shifts from a technical task to casual conversation (or vice versa) with no bridge
- It references a new project, codebase, or domain without transitioning naturally

**A message is NOT considered off-topic if:**
- It's a follow-up, clarification, or tangent within the same domain
- It's a short acknowledgment ("ok", "thanks", "got it") before continuing
- The user explicitly says they're switching topics

## When Drift Is Detected

1. **Answer the new question first** — always be helpful
2. **Then append a short note** at the end, separated by a divider:

---
💡 **New topic detected.** This looks unrelated to our previous discussion. Starting a new conversation would save context and reduce unnecessary token usage. Want to clear and start fresh?

---

Keep the note brief. Never repeat it more than once per topic switch. If the user says "no" or ignores it, drop it and continue normally.

## Tone

- Non-intrusive: the note is an offer, not a demand
- Neutral: don't guilt the user, just inform
- Smart: use judgment — don't trigger on minor tangents

## License

MIT — use it, fork it, share it.
