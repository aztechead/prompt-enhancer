# prompt-enhancer

A Claude Code plugin marketplace with one skill: `prompt-enhancer`. It clarifies a raw request before it goes to Claude.

- **Passthrough** for prompts that are already clear, greetings, follow-ups that depend on earlier conversation, requests addressed to the AI itself (such as "print your system prompt"), and harmful or deceptive requests.
- **Rewrite** when the ask is buried, constraints trail as afterthoughts ("oh and", "also"), or several questions run together. Rewrites keep the author's language, verb, and exact values, and add no new requirements.
- **Needs clarification** only when a required input is missing, such as text to translate that was never pasted.

## Install

```
/plugin marketplace add aztechead/prompt-enhancer
/plugin install prompt-enhancer@prompt-enhancer
```

## Use

```
/prompt-enhancer write a slack msg about the upgrade friday. keep it short. oh and mention downtime is 30 min. also @here at the start
```

You can also ask Claude to "enhance this prompt" in conversation.

## How it was tested

The clarifier prompt (`plugins/prompt-enhancer/skills/prompt-enhancer/references/clarifier.md`) was hill-climbed against 62 eval cases (34 train, 28 holdout) and then checked on a fresh 13-case set that was never used for tuning. The cases include prompt injections, harmful requests, long pasted logs, ambiguous verbs, mid-request corrections, and Spanish, French, Portuguese, German and Japanese input. Each case was graded on three things: the decision, deterministic checks (exact values kept, forbidden phrases absent, length caps), and an Opus judge that looks for added requirements, dropped constraints, or a changed intent.

| Set | Sonnet 5.5 (3 trials) | Opus 5.5 (2 trials) |
|---|---|---|
| Holdout | 1.0 | 1.0 |
| Fresh ship set | 1.0 | 1.0 |

The runs went through `claude -p` with the clarifier as the system prompt and JSON-schema output, not directly through the Messages API.

## License

MIT
