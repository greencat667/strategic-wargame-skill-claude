# Strategic Wargame

A skill for [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) and [Cowork](https://claude.ai) that runs a multi-round strategic simulation — distinct actors take actions under fog of war, and a neutral Control agent adjudicates what actually happens, round by round.

Inspired by [Snow Globe](https://github.com/IQTLabs/snowglobe) (IQT Labs), an LLM-powered qualitative wargaming platform.

## What it does

You describe a strategic situation — a planning fight, a negotiation, a regulatory process, a competitive standoff. The skill:

1. Researches real-world context (WebSearch/WebFetch) to ground the simulation in actual facts, precedents, and actor positions
2. Maps the strategic landscape and generates 4–6 players (organisations, governments, community groups, companies) with explicit objectives, resources, constraints, and — for institutional players — a **pressure threshold**: the specific condition under which they're forced to stop managing a tension and actually resolve it
3. Runs each round under fog of war: players submit actions (intent, method, resources committed, and a private calculation hidden from other players) without seeing what anyone else is doing
4. Has a Control agent adjudicate all actions together — determining what actually happened, including at least one genuinely unintended consequence per round — and classifies each player's position as advancing / holding / losing ground / compromised
5. Produces a final synthesis: outcome assessment, turning points, an alliance/opposition map, and concrete strategic lessons

Output includes an interactive HTML report with a position timeline (the centrepiece — who's gaining and losing at a glance), a separate dark-dashboard timeline visualisation, and a resumable JSON state file. A branded Word report is available on request, pointed at whatever template your organisation supplies.

## Why fog of war and private calculations matter

Players don't know what other players are doing when they choose an action — they can only react to the *previous* round's public adjudication. Each action also carries a private calculation (what they're hoping others will do, what they're trying to avoid) that's hidden from other players but visible to Control and the reader. This is what produces genuine strategic surprise rather than a scripted outcome: the Control agent has to reconcile actions that were chosen without coordination, and the collisions between them are where the interesting findings come from.

The skill also includes an explicit methodological framing that gets passed to every subagent generating a player's action: this is a qualitative simulation for scenario planning, not advocacy for or against any real organisation. Without it, subagents asked to act as real-world organisations sometimes refuse or hedge, treating the task as taking a side rather than modelling one.

## Installation

**Ask Claude to set it up for you.** If you're using Claude Code or Claude Cowork, you can just say something like *"install the strategic-wargame skill from github.com/greencat667/strategic-wargame-skill-claude"* and Claude will clone the repo and put it in the right place — you don't need to do this by hand.

Or do it yourself: copy the `strategic-wargame/` folder into your project's `.claude/skills/` directory:

```bash
git clone https://github.com/YOUR_USERNAME/strategic-wargame-skill-claude.git
cp -r strategic-wargame-skill-claude/strategic-wargame/ your-project/.claude/skills/strategic-wargame/
```

Claude will pick it up automatically from `available_skills` next time you start a session.

## A note on what this is

This is a qualitative simulation, not a prediction engine. It's useful for stress-testing a strategy before committing resources, surfacing blind spots about how other actors are likely to respond, and identifying which moments in a process matter most. It is not useful for precise probability estimates or as a substitute for actual stakeholder engagement.

## Repository structure

```
strategic-wargame-skill-claude/
├── strategic-wargame/
│   └── SKILL.md    # Copy this folder to .claude/skills/
├── README.md
├── CONTRIBUTING.md
└── LICENSE
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) — note this repo isn't actively maintained, so response times on issues and PRs will be slow to nonexistent.

## License

MIT — see [LICENSE](LICENSE).
