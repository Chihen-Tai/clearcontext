# clearcontext

> A lightweight system prompt that helps you save tokens, reduce wasted context, and get routed to the right AI tool — automatically.

Works with **Claude, ChatGPT, Gemini CLI, Codex, or any LLM** that accepts a system prompt.

---

## Features

### 💡 Topic Drift Detection
When you switch to an unrelated topic mid-conversation, the AI flags it and asks if you want to start fresh — so you're not burning tokens on dead context.

### 🤖 Agent Delegation Suggestions
When your task is better suited for a specialized tool (Codex for repo-level coding, Gemini CLI for live search/large docs), the AI tells you — so you use the right tool for the job.

### 🌐 Language Efficiency Prompt
If you're chatting in Chinese, the AI offers a one-time suggestion to respond in English (which uses 20–30% fewer tokens for equivalent content), making your context window go further.

---

## Example Interactions

**Topic drift:**
> [10 messages debugging a Rust async runtime]
> User: "hey what's a good recipe for 麻婆豆腐？"

> AI: Here's a classic 麻婆豆腐 recipe: ...
>
> 💡 **話題偏移偵測到。** 這個問題跟之前的對話關聯不大，開新對話可以省下不必要的 context 消耗。要清除重來嗎？

---

**Agent delegation:**
> User: "Can you refactor my entire repo and fix all the linting errors?"

> AI: Here's how I'd approach it...
>
> 🤖 **更適合的工具：** 這個任務用 Codex 來做會更有效率，也能省下這裡的 token。要我說明怎麼切換嗎？

---

**Language switch:**
> User: "幫我解釋一下 transformer 的 attention mechanism"

> AI: [explains attention mechanism]
>
> 🌐 **省額度提示：** 您目前用中文對話。英文回覆通常少 20–30% 的 token，可以讓對話撐更久、省更多額度。要切換成英文嗎？（您可以繼續用中文問，我用英文回答就好）

---

## Usage

### Claude.ai / Any chat UI
Paste the contents of `system-prompt.txt` into the custom instructions or system prompt box.

### Gemini CLI
```bash
# Append to your Gemini CLI system prompt config
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

---

## Design Principles

- **Answer first, nudge second** — suggestions always come after the actual response
- **One nudge at a time** — never stacks multiple alerts
- **Never repeats** — each nudge fires at most once per trigger
- **Respects your choice** — if you decline, it drops it permanently for that session

---

## Files

| File | Description |
|------|-------------|
| `system-prompt.txt` | The prompt — paste this into your tool |
| `README.md` | This file |

---

## License

MIT — use it, fork it, share it.
