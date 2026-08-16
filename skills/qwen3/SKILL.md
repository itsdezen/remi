---
name: qwen3
description: Use for trivial, cheap, or high-volume sub-tasks (classification, extraction, short summarization, simple text formatting/rewriting, boilerplate drafts, quick lookups) that don't need the main cloud agent's intelligence — offload to the local Ollama qwen3:8b model to save tokens/cost. Coding, complex reasoning, multi-step agentic work, and anything user-facing where quality matters stay on the main agent or go through herdr.
---

# qwen3 (local, via Ollama)

Local model, reached directly over Ollama's HTTP API on `localhost:11434` — no `herdr` involvement. Use `ollama run qwen3:8b "<prompt>"` for a one-off manual check.

## What it is
`qwen3:8b` — Qwen3, 8.2B dense params, Apache 2.0, Q4_K_M quantized (~5.2GB on disk), 40K context window, supports tool calling and a hybrid thinking/non-thinking mode. Installed locally via Ollama (`ollama list` to confirm). Source: [Ollama library](https://ollama.com/library/qwen3), [Qwen3 announcement](https://qwenlm.github.io/blog/qwen3/).

## Measured on this machine (Apple M4, 16GB RAM) — read before using
- **Non-thinking (`think: false`), short prompt**: ~19 tok/s once loaded, well under 2s total. This is the mode to use for the offload use case.
- **Cold load penalty**: ~13s the first call after the model has been idle (Ollama unloads after ~5min by default). Expect one slow call per burst of activity, not per request.
- **Thinking mode (`think: true`)**: measured **89 seconds and 1125 tokens of reasoning to answer "what is 17×24"** on this hardware. Qwen's own docs say thinking mode is for complex problems and non-thinking for simple ones — on this machine thinking mode is slow enough that it's rarely worth it even for moderately hard questions. Default to `think: false`; only try `think: true` for something genuinely non-trivial, and expect to wait a minute or more.

## When to offload here (saves cloud tokens)
Short, low-stakes, easily-verified-or-low-risk-if-wrong tasks:
- Classification / sentiment / routing labels
- Extraction (pull a field, a date, a name out of text)
- Short summarization of small inputs, well under the 40K context
- Simple text formatting, rewriting, or boilerplate drafts (a commit message draft, a template fill-in)
- Quick lookups / restating something already in context

## When to use the main agent instead
- Code editing, multi-file reasoning, or agentic tool use — that's the main cloud agent's job, or a `herdr`-spawned coding agent for parallel work. qwen3 has a "tools" capability flag, but real actions should go through a channel proven reliable.
- Complex reasoning, math, or planning — thinking mode exists for this but is impractically slow on this hardware (see above); use the cloud agent.
- Anything user-facing where getting it wrong matters, or anything needing the 1M-token context / frontier judgment of Opus/Sonnet/Sol/Terra.
- Long inputs — 40K context is generous for a local model but tiny next to cloud models, so keep inputs small and let the cloud agent handle large files.

## Invocation

Check it's up and warm:
```bash
ollama list                              # confirm qwen3:8b is pulled
curl -s http://localhost:11434/api/version
```

One-shot, non-thinking (the default pattern for offloaded tasks):
```bash
curl -s http://localhost:11434/api/generate -d '{
  "model": "qwen3:8b",
  "prompt": "<task text>",
  "stream": false,
  "think": false
}' | python3 -c "import json,sys; print(json.load(sys.stdin)['response'])"
```

Chat-style (multi-turn or system prompt):
```bash
curl -s http://localhost:11434/api/chat -d '{
  "model": "qwen3:8b",
  "messages": [
    {"role": "system", "content": "<instruction>"},
    {"role": "user", "content": "<task text>"}
  ],
  "stream": false,
  "think": false
}' | python3 -c "import json,sys; print(json.load(sys.stdin)['message']['content'])"
```

Manual sanity check from a terminal (not for scripted use — interactive):
```bash
ollama run qwen3:8b "<prompt>"
```

`stream: false` is deliberate — offloaded tasks are small enough that waiting for the full JSON response is simpler than handling SSE chunks.
