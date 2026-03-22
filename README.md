# slop-humanizer

A Claude skill that strips AI-generated writing patterns and rewrites text to sound like a specific human wrote it.

Synthesized from the best techniques across 8 humanizer repos:
[stop-slop](https://github.com/hardikpandya/stop-slop) · [blader/humanizer](https://github.com/blader/humanizer) · [abnahid/claude-humanizer](https://github.com/abnahid/claude-humanizer) · [OrbitWebTools/Humanize-AI](https://github.com/OrbitWebTools/Humanize-AI) · [Khizer-Data/AI-Text-Humanizer](https://github.com/Khizer-Data/AI-Text-Humanizer) · [vardhin/Humanizer](https://github.com/vardhin/Humanizer) · [DadaNanjesha/AI-Text-Humanizer-App](https://github.com/DadaNanjesha/AI-Text-Humanizer-App) · [xszcs546/ai-text-humanizer](https://github.com/xszcs546/ai-text-humanizer)

---

## What it does

- Runs a two-pass rewrite: humanize → audit → humanize again
- Targets 25+ documented AI writing patterns (from Wikipedia's Signs of AI Writing)
- Kills banned phrases, structural clichés, copula avoidance, sycophancy, and more
- Maximizes perplexity and burstiness — the two signals AI detectors actually measure
- Supports casual, professional, or default output modes

---

## How to add it as a Claude skill

### Option 1: Claude Code (recommended)

```bash
# Clone into your Claude skills directory
git clone https://github.com/mbodkebiz/slop-humanizer.git ~/.claude/skills/slop-humanizer
```

Then invoke it in any Claude Code session:

```
/slop-humanizer
```

Paste your text and Claude will humanize it.

### Option 2: Claude Projects (no setup needed)

1. Open [claude.ai](https://claude.ai) and go to a Project
2. Click **Project instructions**
3. Copy and paste the full contents of [`SKILL.md`](./SKILL.md) into the instructions box
4. Every conversation in that project will now humanize text on request

### Option 3: System prompt / API

Copy the contents of `SKILL.md` into your system prompt when calling the Claude API:

```python
import anthropic

with open("SKILL.md", "r") as f:
    skill = f.read()

client = anthropic.Anthropic()
message = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=1024,
    system=skill,
    messages=[
        {"role": "user", "content": "Humanize this: [your text here]"}
    ]
)
print(message.content)
```

### Option 4: Cowork (desktop app)

1. Download [`SKILL.md`](./SKILL.md)
2. Drop it into your `.skills/skills/` folder in your Cowork workspace
3. Claude will automatically pick it up

---

## Usage

Once installed, just say:

> "Humanize this:" and paste your text

Or specify a tone:

> "Humanize this in a casual tone:" / "...professional tone:"

---

## What it fixes

| Category | Examples |
|----------|---------|
| Banned phrases | "Here's the thing:", "At its core", "It's worth noting" |
| AI vocabulary | pivotal, tapestry, delve, garner, underscore, moreover |
| Copula avoidance | "serves as" → is, "boasts" → has |
| Structural clichés | Binary contrasts, negative listings, dramatic fragmentation |
| Sycophancy | "Great question!", "I hope this helps!", "Certainly!" |
| Style issues | Em dashes, title case headings, decorative emojis, synonym cycling |
| False agency | "the data tells us", "the market rewards" |
| Gerund analyses | "symbolizing the tension between..." |

---

## License

MIT
