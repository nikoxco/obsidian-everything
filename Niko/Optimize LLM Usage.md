---
notion-id: 33293272-91a2-800f-8e83-d972c778d44f
base: "[[Do.base]]"
Pile:
Recurring: false
Pin: false
Date Added: 2026-03-29T09:38:00
Hide: Show
---
The $0 OpenClaw setup that we should talk about

Every week I see the same post. "Is $200/month normal?" "My API bill is $47 this week." "I'm on haiku and still spending $22 a day."

And every time, the top answer is "switch to sonnet." which is fine advice. but nobody ever asks the real question: do you need to pay anything at all?

I've been running an openclaw agent for free for the last 3 weeks. not "$5 a month" free. not "free trial" free. actually free. zero dollars. And it handles about 70% of what I used to pay claude to do.

Here's the setup. no fluff.

# Path 1: free cloud models (no hardware needed)

This is the one most people should start with because it requires nothing except an openclaw install you already have.

**OpenRouter free tier.** Sign up at [openrouter.ai](http://openrouter.ai/). No credit card. They offer 30+ free models, including Llama 3.3 70B, Nemotron Ultra 253B, MiniMax M2.5, and Devstral. Some of these are genuinely good. Nemotron Ultra has 262K context. These aren't toy models.

config:

json

```plain text
{
  "env": {
    "OPENROUTER_API_KEY": "sk-or-..."
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "openrouter/nvidia/nemotron-ultra-253b:free"
      }
    }
  }
}
```

If you don't want to pick a specific model, OpenRouter has a free router that auto-selects from whatever's available:

```plain text
"primary": "openrouter/openrouter/free"
```

**Gemini free tier.** google gives you 15 requests per minute on Gemini Flash for free. that's more than enough for casual daily use. get an API key from ai.google.dev and run `openclaw onboard`, pick Google. It's a built-in provider so the setup is straightforward.

**Groq.** fast. very fast. free tier has rate limits but for basic agent tasks it works. sign up, get API key, done.

The catch with all cloud free tiers: rate limits. you will hit them. Your agent will pause, wait, retry. For light to moderate daily use (10-20 interactions) this is barely noticeable. For "always-on agent doing 100 tasks a day" it won't cut it. But let's be honest, if you just installed OpenCLaw this week, you are not running 100 tasks a day.

# Path 2: local models via Ollama (truly $0, forever)

This is the setup where your API bill is literally zero because nothing leaves your machine. no API key. no account. no rate limits. No data going anywhere.

Ollama became an official OpenClaw provider in March 2026 so this is now a first-class setup, not a hack.

**Step 1:** install Ollama.

bash

```plain text
curl -fsSL <https://ollama.com/install.sh> | sh
```

**Step 2:** pull a model.

bash

```plain text
# if you have 20GB+ VRAM (RTX 3090, 4090, M4 Pro/Max)
ollama pull qwen3.5:27b

# if you have 16GB VRAM
ollama pull qwen3.5:35b-a3b

# if you have 8GB VRAM (most laptops)
ollama pull qwen3.5:9b
```

Qwen3.5 27B is the current sweet spot for openclaw. it handles tool calling well enough for daily agent tasks and the 35b-a3b mixture-of-experts variant runs at 112 tokens/second on an RTX 3090 because it only activates 3B parameters at a time.

**Step 3:** run onboarding and pick Ollama.

bash

```plain text
openclaw onboard
```

Select Ollama from the provider list. it auto-discovers your local models. done.

or the simplest manual setup (auto-discovery, no manual model config needed):

bash

```plain text
export OLLAMA_API_KEY="ollama-local"
```

That's it. OpenClaw discovers your models from `http://127.0.0.1:11434` automatically and sets all costs to 0.

If you need manual config (ollama on a different host or you want to force specific settings):

json

```plain text
{
  "models": {
    "providers": {
      "ollama": {
        "baseUrl": "<http://localhost:11434>",
        "apiKey": "ollama-local",
        "api": "ollama",
        "models": [
          {
            "id": "qwen3.5:27b",
            "name": "Qwen3.5 27B",
            "reasoning": false,
            "contextWindow": 131072,
            "maxTokens": 8192
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/qwen3.5:27b"
      }
    }
  }
}
```

Important stuff that will save you hours of debugging:

- Use the native Ollama API URL (`http://localhost:11434`), NOT the OpenAI compatible one (`http://localhost:11434/v1`). the /v1 path breaks tool calling and your agent will output raw JSON as plain text. I wasted an entire evening figuring that out.
- Set `"reasoning": false` in the model config. when reasoning is enabled, openclaw sends prompts as "developer" role which ollama doesn't support, and tool calling breaks silently.
- Set `"api": "ollama"` explicitly to guarantee native tool-calling behavior.

# Path 3: the hybrid (what I actually recommend)

pure free has limits. local models struggle with complex multi-step reasoning. free cloud tiers have rate limits. so here's what I actually run:

- **Default model:** Ollama/Qwen3.5 27B (local, free). handles file reads, calendar checks, simple summaries, web searches, reminders. about 70% of daily tasks.
- **Fallback:** OpenRouter free tier (Nemotron Ultra or Llama 3.3 70B). catches anything the local model fumbles.
- **Emergency escalation:** Sonnet. only for genuinely complex stuff. maybe 5 times a week.

with this setup my last month's API spend was $2.40. two dollars and forty cents. The sonnet calls were the only ones that cost anything.

config for the hybrid approach:

json

```plain text
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/qwen3.5:27b",
        "fallbacks": [
          "openrouter/nvidia/nemotron-ultra-253b:free",
          "anthropic/claude-sonnet-4-6"
        ]
      }
    }
  }
}
```

OpenCLAW handles the cascading automatically. if local fails or returns garbage, it tries the next model in the list. if that hits a rate limit, it goes to the next one. you don't have to manage this manually.

# What works on free models

This surprised me.... local and free cloud models handle more than I expected:

- reading and summarizing files. solid.
- calendar management, reminders, basic scheduling. fine.
- web searches and summarizing results. good enough.
- simple code edits, config changes, boilerplate. works.
- quick lookups ("what's the syntax for X"). instant and free.
- reformatting text, cleaning up notes, drafting short messages. no issues.

# What doesn't work (be honest with yourself)

- Complex multi-step debugging. local models lose the thread after step 3. use sonnet for this.
- Long nuanced conversations with lots of context. free models forget things faster.
- Anything where precision matters more than speed. legal, financial, medical. pay for the good model.
- Heavy tool chaining. five tools in sequence, each dependent on the last. sonnet or opus territory.

The mental model is simple: if you would answer the question without thinking hard, a free model can handle it. If you'd need to actually sit down and reason through it, pay for reasoning.

# Stuff nobody will tell you out loud

**Heartbeats cost money too.** OpenClaw runs a health check every 30-60 minutes. if your primary model is Claude Opus, every heartbeat costs you tokens. on local models, heartbeats are free. On Opus, someone calculated it's roughly $30-50/month just in heartbeats. That's the "I'm not even using my agent and my bill is growing" problem.

**Sub-agents inherit your primary model.** When your agent spawns a sub-agent for parallel work, that sub-agent uses whatever model you have set as primary. if primary is opus, every sub-agent runs on opus. with the latest update you can set model fallbacks that help with this.

**Cron jobs create sessions that never clean up.** Every cron job creates a session record. over weeks, these accumulate and bloat your context. recent updates added session TTL to help with this. update if you haven't.

**Free models + no skills = the right starting point.** Don't add clawhub skills to a free model setup. skills inject instructions into your context window. on an 8K-32K context local model, skills eat half your available context before you even say hello. learn what your agent can do stock first. add skills later when you move to a cloud model with bigger context.

# The real question

Most people who ask "how do I reduce my openclaw costs" are actually asking the wrong question. The right question is "which of my tasks actually need a $15/million-token model and which ones don't?"

The answer, for almost everyone I've helped, is that 60-80% of what they ask their agent to do could be handled by a model that costs nothing.

Start free. move tasks up to paid models only when free genuinely can't handle them. not when it feels slightly slower. not when the formatting isn't perfect. when it actually fails.

The people spending $200/month on OpenClaw aren't getting 40x more value than I'm getting at $2.40. They're getting maybe 1.3x more value and paying for the convenience of not thinking about it.

Think about it. Your wallet will thank you.

\-----------

Running this on a Mac Mini M4 with 16GB RAM if anyone's wondering about hardware. Ollama + Qwen3.5 9B runs fine on it. not blazing fast but fast enough that I don't notice the difference for basic tasks.