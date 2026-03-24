# slop-humanizer

A Claude skill that strips AI-generated writing patterns and rewrites text to sound like a specific human wrote it.

Synthesized from 8 humanizer repos:
[stop-slop](https://github.com/hardikpandya/stop-slop) · [blader/humanizer](https://github.com/blader/humanizer) · [abnahid/claude-humanizer](https://github.com/abnahid/claude-humanizer) · [OrbitWebTools/Humanize-AI](https://github.com/OrbitWebTools/Humanize-AI) · [Khizer-Data/AI-Text-Humanizer](https://github.com/Khizer-Data/AI-Text-Humanizer) · [vardhin/Humanizer](https://github.com/vardhin/Humanizer) · [DadaNanjesha/AI-Text-Humanizer-App](https://github.com/DadaNanjesha/AI-Text-Humanizer-App) · [xszcs546/ai-text-humanizer](https://github.com/xszcs546/ai-text-humanizer)

---

## What it does

LLMs predict the most statistically likely next token across all possible contexts. The output optimizes for broad applicability, not voice. Every piece of AI writing ends up sounding like the same person — because it is.

This skill reverses that. It runs a two-pass rewrite targeting 25+ documented AI writing patterns, then scores the output before delivering it. The goal is text that reads like one specific human wrote it.

**Before**

> It's worth noting that navigating today's complex landscape requires a multifaceted approach. The implications are significant, and at its core, this underscores the need for teams to deeply align on strategy moving forward.

**After**

> Teams that don't agree on strategy waste time on the wrong work. That's the whole problem.

---

## How it works

The skill runs five steps on every piece of text:

1. **Scan** — identify every AI tell across 25+ pattern categories
2. **Rewrite** — fix each problem, preserve meaning, no decorating
3. **Audit** — second detection pass asking "what still sounds robotic?"
4. **Second rewrite** — catch what slipped through
5. **Deliver** — final output with a brief note on what changed

It also scores output on five dimensions (directness, rhythm, trust, authenticity, density) before delivering. Anything below 35/50 gets revised again.

Two signals AI detectors actually measure: perplexity (how predictable each word choice is) and burstiness (how much sentence length varies). The skill maximizes both.

---

## Pattern taxonomy

The skill targets six categories of AI writing failure:

**Banned phrases** — throat-clearing openers ("Here's the thing:"), emphasis crutches ("Full stop."), filler ("At its core"), meta-commentary ("Let me walk you through"), and 30+ high-frequency AI vocabulary words (pivotal, tapestry, delve, garner, underscore, moreover).

**Copula avoidance** — AI replaces "is" and "has" with elaborate constructions. "Serves as a reminder" becomes "reminds." "Boasts impressive results" becomes "got impressive results."

**Structural patterns** — binary contrasts ("Not X. Y."), negative listings, dramatic fragmentation, rhetorical setups, and false agency where abstractions perform human actions ("the market rewards," "the data tells us").

**Superficial -ing analyses** — gerunds that fake depth without making a claim. "Symbolizing the tension between..." gets replaced with an actual statement of what the tension is.

**Style issues** — em dashes, excessive bold, title case headings, synonym cycling, perfectly uniform paragraph length, and hedging stacks ("could potentially possibly suggest").

**Sycophancy** — "Great question!", "Certainly!", "I hope this helps!", unprompted knowledge-cutoff disclaimers.

---

## How to install

### Claude Code (recommended)

```bash
git clone https://github.com/mbodkebiz/slop-humanizer.git ~/.claude/skills/slop-humanizer
```

Then invoke it in any Claude Code session:

```
/slop-humanizer
```

### Claude Projects (no setup)

1. Open [claude.ai](https://claude.ai) and go to a Project
2. Click **Project instructions**
3. Paste the full contents of [`SKILL.md`](./SKILL.md)

Every conversation in that project will now humanize text on request.

### API / system prompt

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

### Cowork (desktop)

1. Download [`SKILL.md`](./SKILL.md)
2. Drop it into `.skills/skills/` in your Cowork workspace

---

## Usage

```
Humanize this: [paste text]
```

Specify a tone if needed:

```
Humanize this in a casual tone: [paste text]
Humanize this in a professional tone: [paste text]
```

Default mode matches the register of the original and removes AI tells without shifting the voice.

---

## FAQ

**Does this bypass AI detectors?**
It targets the two signals detectors measure — perplexity and burstiness — so output scores better on tools like GPTZero and Originality. No tool guarantees a clean pass. The goal is writing that sounds human, not gaming a classifier.

**Can I use this on Claude.ai without Claude Code?**
Yes. Paste the contents of `SKILL.md` into your Project instructions. All conversations in that project will have the skill active.

**Does it work on short text like social posts?**
Yes. The two-pass process applies regardless of length, though very short text (under 50 words) may only need one pass.

**Will it change my meaning?**
No. The skill's core rule is preserve meaning. It rewrites how something is said, not what is said. If a rewrite changes a claim, that's a bug — open an issue.

**What models does it work with?**
Tested on Claude Sonnet and Opus. Should work on any instruction-following model capable of multi-step editing tasks.

---

## Contributing

Issues and pull requests are welcome. The most useful contributions:

- New banned phrases or AI vocabulary spotted in the wild
- Pattern categories that aren't covered yet
- Before/after examples that demonstrate edge cases
- Corrections where the skill changed meaning it shouldn't have

Open an issue before a large PR so we can align on scope first.

---

## License

MIT
