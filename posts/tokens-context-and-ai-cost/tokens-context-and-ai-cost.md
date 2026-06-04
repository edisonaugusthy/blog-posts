# What you should know about tokens, context, and AI cost

Most of the use AI coding tools in a very normal way.

We paste an error, ask for a fix, paste a file, ask again, run a command, paste the output, and keep going. After some time we get a message telling that `you are out of tokens/messages`

Most of the time, the reason is tokens.

## What is a token?

A token is a small piece of text the model reads or writes.

It can be a word, part of a word, a symbol, or spacing depending on the language and context. The model does not see text exactly like we do. It breaks everything into tokens first.

[https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-the](more about counting tokens here)

So when you send a message, you are sending input tokens. When the model replies, it creates output tokens. If your coding agent reads files, terminal logs, docs, diffs, and old chat history, that can also become input tokens.

That is why a small question can become expensive when it carries a lot of hidden text behind it.

## What is a context window?

The context window is how much text the model can keep in front of it at one time.

Think of it like a desk. If the desk only has the bug, the related file, and the test error, the model can focus. If the desk has old logs, full diffs, generated files, long docs, and ten previous attempts, it can still work, but now it has to search through all of that.

A bigger context window helps, but it does not fix messy context.

Some models can hold a lot now. 200K tokens is already common in many coding workflows. Some newer models can go near 1M tokens. That sounds huge, and it is huge. But it does not mean you should always use it.

Roughly speaking, 1M tokens can be hundreds of pages of text. It can be a big part of a codebase, many docs, or long chat history. But the model still has to read through that text. More context can mean more cost, more waiting, and more chances for the important thing to get buried.

That is the part people miss. A large context window is like a large backpack. It is useful when you need it, but you do not want to carry bricks every day just because the bag can hold them.

Here is a rough way to think about it:

| Context size | What it can feel like in real life |
| ---: | --- |
| 32K tokens | A few files, a long bug report, or a small feature discussion |
| 128K tokens | Many files, long logs, or a decent chunk of project docs |
| 200K tokens | A large debugging session with files, logs, and history |
| 1M tokens | Hundreds of pages, big docs, or a large slice of a codebase |

This is not exact. Different languages, code, spacing, and tokenizers change the count. But it gives you the idea.

When the context is full or messy, a few things can happen:

- The reply can take longer.
- The cost can go up.
- The model can miss the main point.
- It can keep using old information.
- It can start fixing the wrong problem.
- You may need to compact the chat or start fresh.

So the goal is not to give the model everything. The goal is to give it the right things.

## Why cost grows so fast

AI agents are powerful because they can read files and run commands. But that also means they can send a lot of text back into the conversation.

These things usually waste tokens:

- huge terminal logs
- full `git diff` output
- lock files
- generated files
- long `AGENTS.md` or `CLAUDE.md` files
- repeated docs
- old chat history
- pasting a full file when one function is enough

Two people can ask the same question and pay very different cost. One sends a clean error and one file path. The other sends the whole repo, full logs, and old attempts. The second one usually pays more and may get a worse answer.

## Input tokens and output tokens are not the same price

This is another thing many people do not notice.

Input tokens are what you send to the model. Output tokens are what the model writes back. In many models, output tokens cost much more than input tokens.

That matters a lot for coding agents because they do not just answer with a small paragraph. They think, call tools, explain, write code, run commands, and sometimes produce long patches. Reasoning tokens can also be counted as output tokens in many pricing systems.

Here is a simple pricing snapshot checked on June 4, 2026. These prices can change, so always check the official page before making a serious budget.

| Provider / model | Input per 1M tokens | Output per 1M tokens | What to notice |
| --- | ---: | ---: | --- |
| OpenAI GPT-5.4 | $2.50 | $15.00 | Output is 6x input |
| OpenAI GPT-5.4 mini | $0.75 | $4.50 | Still 6x input |
| OpenAI GPT-5.3 Codex | $1.75 | $14.00 | Output is 8x input |
| Claude Sonnet 4.6 | $3.00 | $15.00 | Output is 5x input |
| Claude Haiku 4.5 | $1.00 | $5.00 | Cheaper, but same 5x pattern |
| Gemini 2.5 Pro, <= 200K prompt | $1.25 | $10.00 | Output is 8x input |
| Gemini 2.5 Flash | $0.15 | $1.25 | Good for cheaper simple work |
| Gemini 2.5 Flash-Lite | $0.10 | $0.40 | Very cheap for light tasks |

This is why "make the answer shorter" is not just about readability. It can save real money.

It is also why output-heavy work can surprise you. If the agent writes long explanations, repeats full files, or prints large patches again and again, the expensive side of the bill grows fast.

For Codex users, there is also a credit-based view. Current Codex pricing maps credits to input, cached input, and output tokens. For example, GPT-5.3 Codex costs much more credits for output tokens than input tokens. So the same idea still applies: long answers and output-heavy tasks cost more.

## A small cost example

Let us say one coding task uses:

- 80,000 input tokens from files, logs, and chat history
- 20,000 output tokens from reasoning, tool use, and code changes

On a model where input is $3 per 1M and output is $15 per 1M:

| Part | Math | Cost |
| --- | --- | ---: |
| Input | 80,000 x $3 / 1,000,000 | $0.24 |
| Output | 20,000 x $15 / 1,000,000 | $0.30 |
| Total | One task | $0.54 |

The output is only one fourth of the tokens here, but it costs more than the input.

Now imagine running many agents, long sessions, CI fixes, reviews, and retries. That small number can grow quietly.

## How to handle a full context window

When the chat starts getting heavy, do not keep adding more text forever.

A better flow is:

1. Finish the current small step.
2. Ask the agent to summarize what changed, what failed, and what is next.
3. Save that summary in a file if the task is long.
4. Compact the chat if your tool supports it.
5. Start fresh when the old chat is no longer useful.

For coding, I like this rule:

Keep long-term knowledge in files. Keep chat history temporary.

But even those files should be short. Your `AGENTS.md` or `CLAUDE.md` should point the agent to the right place, not paste every detail into every session.

Bad:

```md
Here are 900 lines of backend notes, frontend rules, deployment steps,
old bugs, API docs, and every command we use.
```

Better:

```md
For backend rules, read docs/backend.md.
For release steps, read docs/release.md.
Run npm test after changing shared code.
Do not edit dist/.
```

That is cheaper and usually works better.

## Simple ways to save tokens and money

The biggest win is to stop sending noisy output.

Instead of sending 2,000 lines of test output, send the failing test name, error message, and file path.

Instead of asking the agent to read the whole repo, say where the bug probably is.

Instead of pasting a full file, point to the file and ask it to inspect the needed function.

Good habits:

- Keep instruction files short.
- Point to files instead of pasting files.
- Ask for focused searches.
- Avoid generated files and build output.
- Summarize long logs.
- Compact after a finished phase.
- Start a new session for a new task.
- Use cheaper or faster models for simple edits when your tool supports it.
- Use higher reasoning only for hard tasks.

Prompt caching can also help. Many providers can reuse stable prompt parts, which can make repeated instructions cheaper or faster. But caching works best when the repeated part stays stable. If every prompt starts with a different giant blob, it will not help much.

## Saving money in Claude Code

For Claude Code, the main file is `CLAUDE.md`.

Useful things to check:

- project `CLAUDE.md`
- global `~/.claude/CLAUDE.md`
- `.claude/settings.json`
- `/compact`
- `/clear`
- `/config`

Keep `CLAUDE.md` short. Put important commands and project rules there. Link to deeper docs instead of copying them.

Use `/compact` when the task is still active but the chat is getting full. Use `/clear` when the old chat is not useful anymore.

Also be careful with `@` file references. If you add a huge file to context, you pay for that huge file.

## Saving money in Codex

For Codex, the main project instruction file is `AGENTS.md`. Codex config usually lives in `~/.codex/config.toml`.

Useful values:

- `AGENTS.md`
- `model`
- `model_reasoning_effort`
- `project_doc_max_bytes`
- `model_instructions_file`

The one I would watch closely is `project_doc_max_bytes`. It controls how much project instruction text Codex can load from agent docs. If your `AGENTS.md` is large, reducing this can stop every session from starting with too much text.

Example:

```toml
model_reasoning_effort = "medium"
project_doc_max_bytes = 20000
```

Do not copy values blindly. A small project and a large monorepo need different limits.

## Saving money in OpenCode

OpenCode also uses `AGENTS.md`.

Useful places:

- project `AGENTS.md`
- global `~/.config/opencode/AGENTS.md`
- `opencode.json`
- `.opencode/skills/<name>/SKILL.md`
- `.opencode/agents/`

Keep global rules short. Keep project rules focused on commands, conventions, and where to look.

If you have long reusable instructions, move them into skills instead of loading them every time. If you have different jobs, use separate agents instead of one giant instruction file.

## Every setup is different

There is no perfect number.

Each device, repo, model, and tool can behave differently. A small library, a frontend app, a backend service, and a large monorepo all need different settings.

Start small. Watch what the agent keeps reading. Remove repeated logs and repeated docs. Increase limits only when the agent is clearly missing useful context.

You are not trying to starve the model. You are trying to feed it cleanly.

## I made a tool for this

I made a tool called `token-optimizer` to help reduce AI token usage for coding agents.

The idea is simple. A lot of token waste comes from noisy command output, huge logs, repeated file reads, and verbose tool responses. `token-optimizer` filters and compresses that output before it gets sent back into the agent conversation.

There are already online tools for some parts of this. You can find token counters, pricing calculators, cost graphs, and model comparison pages. They are useful when you want to estimate something manually.

But I wanted something that works closer to the real coding flow. Not just a calculator after the bill comes. Something that can sit in the workflow and reduce noisy output before it burns context.

That is why `token-optimizer` combines a few ideas:

- command output filtering
- log compaction
- repeated output reduction
- agent-friendly summaries
- setup for different coding agents

Install:

```bash
npx token-optimizer install
```

Or:

```bash
curl -fsSL https://raw.githubusercontent.com/edisonaugusthy/token-optimizer/main/install.sh | bash
```

Read more:

- [token-optimizer on npm](https://www.npmjs.com/package/token-optimizer)
- [token-optimizer on GitHub](https://github.com/edisonaugusthy/token-optimizer)
- [OpenAI: What are tokens?](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them)
- [OpenAI: Prompt caching](https://platform.openai.com/docs/guides/prompt-caching)
- [Claude Code usage and context](https://support.claude.com/en/articles/14552983-models-usage-and-limits-in-claude-code)
- [OpenCode instructions](https://opencode.school/lessons/instructions/)
- [AGENTS.md guide](https://agents.md/)

Tokens are not something to be scared of. You just need to be aware of them.

Clean context usually means better answers, lower cost, and fewer strange turns in the middle of a task.
