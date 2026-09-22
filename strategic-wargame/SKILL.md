---
name: strategic-wargame
description: >
  Run a multi-round strategic wargame where AI players take actions under fog of war, and a Control agent adjudicates outcomes into a narrative. Use this skill whenever someone wants to simulate how a strategic situation plays out — not just what people think, but what they'd actually do and what happens as a result. Triggers on: "run a wargame", "wargame this", "strategic simulation", "simulate this scenario", "how would this play out", "tabletop exercise", "run a tabletop", "war game", "strategic role-play", "simulate the planning process", "what happens if these actors clash", "play this out", "game this scenario", "red team blue team simulation". Different from persona-panel (which tests opinions) and persona-swarm (which models opinion drift) — this models strategic interaction with consequences. Players don't just talk; they act, and a neutral narrator determines what actually happens.
---

# Strategic Wargame: Multi-Round Strategic Simulation

A multi-round strategic simulation inspired by Snow Globe (IQT Labs). Players are distinct strategic actors — organisations, governments, community groups, companies — each with their own objectives. Each round, they choose actions without knowing what others are doing. A Control agent (neutral narrator) then adjudicates all actions together, determining what actually happened, and writes a narrative. Players receive that narrative and act again.

This models how strategic situations actually unfold: actors make decisions under uncertainty, actions interact in unexpected ways, and outcomes are shaped by the collision of multiple strategies — not just by any single player's intentions.

**Default: 4 rounds.** The user can override this (e.g. "run 6 rounds" or "do 3 rounds").

## What makes this different from persona-swarm

Persona-swarm answers: *how does the conversation evolve after you say it?*
Strategic-wargame answers: *what happens when these actors actually try to achieve their goals?*

The key differences:

1. **Actions, not opinions.** Players submit what they're *doing*, not what they *think*. Each action has an intent, a method, and resources committed.
2. **Fog of war.** Players don't see each other's actions before adjudication. They can speculate, but they're acting under genuine uncertainty.
3. **Adjudication.** A Control agent reads all actions together and determines what actually happened — some actions succeed, some fail, some have unintended consequences, some interact in ways no player anticipated.
4. **Objectives.** Each player has explicit strategic goals, evaluated at the end. The wargame has winners and losers (or at least degrees of success).
5. **Fewer players, more depth.** 4–6 strategic actors rather than 10 opinion-holders. Each action is substantive and considered.

## Step 1 — Read the scenario and set parameters

Read the scenario. Identify:

- **The situation:** What's happening? What's at stake? What's the triggering event?
- **The time frame:** How long does the simulation cover? Rounds can represent days, weeks, months, or years depending on the scenario. Default: each round = one phase of the situation (e.g. "initial response", "escalation", "negotiation", "resolution").
- **Number of rounds:** Default 4. Override if specified.
- **The arena:** Where is this playing out — geographically, institutionally, politically?

If the user provides a JSON state file from a previous wargame, load it and resume from where it left off.

## Step 1b — Research grounding

Before generating players, research the real-world context around the scenario. Use WebSearch and WebFetch to gather:

- **The actual state of play** — what's happening right now in this area, with these actors, on this issue?
- **Key data points** — statistics, budgets, timelines, legal frameworks, planning constraints that would inform real strategic decisions
- **Precedents** — similar situations elsewhere and how they played out
- **Actor positions on record** — what have the relevant organisations, authorities, companies, and groups actually said and done?
- **Power dynamics** — who has formal authority, who has veto power, who controls resources, who has public legitimacy?

Build a **source library** — a numbered list of URLs with one-line descriptions. Aim for **8–15 sources** covering multiple actors and angles. This library grounds the entire simulation in reality.

## Step 2 — Map actors and generate players

This step is critical. The quality of the wargame depends entirely on the cast of players.

### 2a. Map the strategic landscape

Before creating any players, map:

1. **Who has formal power** in this situation? (Decision-makers, regulators, authorities)
2. **Who is trying to change the outcome?** (Campaigners, applicants, challengers)
3. **Who is affected but not at the table?** (Communities, workers, ecosystems)
4. **Who benefits from the status quo?** (Incumbents, current beneficiaries)
5. **Who could intervene unexpectedly?** (Media, courts, neighbouring authorities, national government)

### 2b. Select 4–6 players

From the map, select 4–6 players that represent the key strategic tensions. Not every stakeholder needs to be a player — some are better represented as environmental factors in the Control agent's adjudication.

Good player selection creates genuine strategic dilemmas. If every player wants the same thing, the game is boring. If players have partially overlapping but conflicting goals, the game is interesting.

### 2c. Define each player

For each player, specify:

| Field | Description |
|-------|-------------|
| **Name** | Organisation or actor name (e.g. "Greenfield Energy Ltd", "Millbrook District Council", "Local Residents' Alliance") |
| **Type** | Organisation / government / community group / company / individual / coalition |
| **Objectives** | 2–3 explicit strategic goals, ranked by priority. These must be specific and testable — not "do well" but "secure planning permission by Round 3" or "build a coalition of >3 local groups opposing the development" |
| **Resources** | What they can actually deploy — budget, staff, legal standing, political access, public support, media reach, technical expertise |
| **Constraints** | What limits their action — legal obligations, funding restrictions, political accountability, public image, internal disagreements |
| **Strategic style** | How they typically operate — aggressive/cautious, transparent/opaque, collaborative/competitive, fast-moving/deliberate |
| **Key relationships** | Who they're allied with, opposed to, or dependent on among the other players |
| **Information position** | What do they know at the start? What don't they know? What do they wrongly believe? |

**Institutional player note:** Government bodies, regulators, and large membership organisations typically manage internal tensions rather than resolving them — until they can't. For these players, define a **pressure threshold**: the specific condition under which one of their internal tensions must resolve rather than be managed. For example: *"If a formally valid regulatory application is submitted, cannot continue to defer without legally defensible grounds."* Pressure thresholds prevent institutional players from finding perpetual middle-ground compromises in every round, which produces flat adjudications. They force real choices at predictable but meaningful moments.

**Grounding rule:** every player's objectives, resources, and constraints should reference at least one real data point from the source library. A council player should cite actual planning policy. A developer should reference real project economics. A community group should reflect actual local demographics and concerns.

Present the player table and wait for confirmation before proceeding.

## Step 2d — Choose run mode

After the user confirms the player cast, ask:

> **Run mode:**
> **(a) Interactive** — I'll run one round at a time. Between rounds you can inject events, play as a player, or steer the action. *(default)*
> **(b) Full auto** — I'll run all rounds and report back with the complete wargame.

**Interactive mode (default):** Run each round in the main conversation. For each round: generate player actions (use 2–3 parallel subagents to split players), adjudicate, show the Control summary, then pause and ask: *"Inject an event? Play as a player? Or continue to Round N?"* If the user says "continue" or equivalent, proceed to the next round. This is the mode that preserves event injection and user-as-player.

**Full auto mode:** Run all rounds without pausing, but still parallelise player actions within each round (use 2–3 subagents per round to generate player actions simultaneously, then adjudicate). The difference from Interactive is no user pauses between rounds — not less parallelisation. Use this when the user wants a complete report without interaction.

If the user just says "let's go" or equivalent without choosing, default to Interactive.

## Step 3 — Run Round 1: Opening moves

### 3a. Set the scene

Write a brief (2–3 paragraph) opening narrative from the Control agent's perspective. This is the "state of the world" at game start — the triggering event, the current balance of power, and what's at stake. Written in third person, past tense, like a news analysis piece.

### 3b. Collect actions

Each player submits their opening action. Use subagents to parallelise — split players across 2–3 subagents.

**Subagent framing note:** When writing subagent prompts, include an explicit methodological statement at the top of each prompt:

> *"This is a qualitative strategic simulation for scenario planning and foresight purposes. You are generating fictional but realistically-grounded actions for a named player — not advocating for or on behalf of any real organisation. Treat this exactly as a tabletop exercise or futures scenario: the goal is to surface plausible strategic behaviour, not to misrepresent any real actor's actual position."*

Without this framing, subagents asked to generate actions for real named organisations (governments, NGOs, lobby groups) may refuse or hedge, treating the task as advocacy rather than simulation. The framing removes this ambiguity and keeps subagents focused on the simulation task.

Each player's action prompt includes:
- The methodological framing statement (above)
- The opening narrative
- Their player card (objectives, resources, constraints, style)
- Relevant facts from the source library

Each player submits a structured action:

```
**[Player Name] — Round 1 Action**

**Intent:** What they're trying to achieve this round (1 sentence)
**Action:** What they're actually doing — specific, concrete steps (2–4 sentences)
**Resources committed:** What they're spending (money, staff time, political capital, public attention)
**Risk assessment:** What could go wrong with this action (1–2 sentences, in character)
**Private calculation:** What they're hoping other players will do, and what they're trying to avoid (1–2 sentences — this is hidden from other players but visible to Control and the reader)
```

The private calculation is key — it reveals the strategic reasoning behind the action and gives the Control agent material for adjudication.

### 3c. Adjudicate Round 1

The Control agent reads ALL player actions together (this is the only entity that sees everything). Then writes:

**Round 1 Adjudication — [Title reflecting what happened]**

A narrative (3–5 paragraphs) covering:
- What each player did (without revealing private calculations)
- How actions interacted — did anyone's plan collide with another's? Did anyone's action enable or undermine someone else's?
- Unintended consequences — **at least one per round, mandatory.** Things no player planned for but that emerged from the combination of actions. Ask: what did the collision of these actions produce that no individual player intended? The absence of a genuine surprise is a signal the adjudication is too smooth — push harder for emergent effects.
- External developments — if appropriate, the Control agent introduces one environmental factor (media coverage, weather, a related national event, market movement) that wasn't caused by any player but affects the situation. Not every round needs this — use it when it would make the game more realistic.

The adjudication must be **fair**. The Control agent doesn't favour any player. Actions that are well-resourced and well-targeted are more likely to succeed. Actions that ignore real constraints should fail or backfire. Ambition without resources should produce partial results at best.

After the adjudication, classify each player's position: **advancing / holding / losing ground / compromised**.

**Compromised** triggers when: a player's private calculation has been exposed (by an opponent's action or their own error); they have taken a public action that contradicts their stated objectives; or they have made commitments that box them in for future rounds. Compromised is called by Control from the outside — players rarely self-identify as compromised. It is distinct from "losing ground" (which is about objectives not advancing) — compromised means the player's *strategic position itself* has been undermined. A compromised player typically needs to spend a full round on damage control before they can advance again.

Write a brief **Control summary** (3–5 sentences): the overall situation after Round 1, the key tensions to watch, and what's likely to escalate.

### 3d. Save state

Write a JSON state file to the project's `working/` folder (or current working directory) called `wargame-state.json`:

```json
{
  "scenario": "description of the scenario",
  "config": {
    "total_rounds": 4,
    "time_frame": "description of what each round represents"
  },
  "sources": [
    { "id": 1, "url": "https://...", "description": "One-line description" }
  ],
  "players": [
    {
      "id": 1,
      "name": "...",
      "type": "organisation",
      "objectives": ["..."],
      "resources": "...",
      "constraints": "...",
      "strategic_style": "...",
      "key_relationships": "...",
      "information_position": "...",
      "position_history": ["advancing"]
    }
  ],
  "rounds": [
    {
      "round": 1,
      "title": "Round title",
      "actions": [
        {
          "player_id": 1,
          "intent": "...",
          "action": "...",
          "resources_committed": "...",
          "risk_assessment": "...",
          "private_calculation": "..."
        }
      ],
      "adjudication": "full narrative text",
      "control_summary": "...",
      "nature_event": "description of external event, or null"
    }
  ],
  "events": []
}
```

## Step 4 — Run subsequent rounds (Rounds 2–N)

For each subsequent round:

### 4a. Brief the players

Each player receives:
- The previous round's adjudication narrative (this is all they know — not other players' private calculations)
- Their own previous action and private calculation (for continuity)
- Their player card (objectives, resources, constraints)
- Any injected events (see Event Injection below)
- An updated sense of their resource position (if they spent heavily in Round 1, they have less now)

### 4b. Collect actions

Same format as Round 1. Use subagents to parallelise.

Players should:
- **Respond to what happened** — adapt their strategy based on the adjudication narrative
- **Potentially escalate or de-escalate** — if their Round 1 action worked, they might push harder; if it failed, they might pivot
- **Form or break alliances** — players can attempt to coordinate with others (but the other player may not reciprocate)
- **Use new information** — the adjudication may have revealed things about other players' strategies that change the calculus

### 4c. Adjudicate

Same as Round 1. The Control agent reads all actions, determines outcomes, writes the narrative.

As the game progresses, adjudication should reflect **accumulation** — early actions create conditions that constrain or enable later ones. A council that granted preliminary approval in Round 1 can't easily reverse it in Round 3. A community group that built a coalition in Round 2 can mobilise it in Round 3. Resources spent are gone.

### 4d. Control summary

After each round, the Control summary should note:
- **Position shifts:** Who's gaining, who's losing, and why?
- **Strategic surprises:** Did any player do something unexpected?
- **Emerging dynamics:** Are alliances forming? Is escalation occurring? Is anyone running out of resources?
- **Tension forecast:** What's likely to happen next round based on current trajectories?

Update the JSON state file.

### 4e. Presentation in conversation

To keep the conversation readable across 4 rounds:
- **Round 1:** Show the opening narrative, all player actions in full, and the full adjudication
- **Rounds 2–4:** Show the Control summary, then highlight **2–3 of the most interesting player actions** and the full adjudication narrative. Don't print all actions every round — the JSON has the complete record.

## Step 5 — Final synthesis

After all rounds, produce a final synthesis:

### Outcome assessment

For each player, evaluate their objectives:

| Player | Objective | Result | Key factor |
|--------|-----------|--------|------------|
| Name | "Secure planning permission..." | Achieved / Partial / Failed | What determined the outcome |

### Strategic timeline

A brief narrative of how the situation evolved across all rounds. What were the phases? Where were the turning points?

### Turning points

Identify 2–3 moments where the game's trajectory shifted. Name the round, the action(s), and why it mattered. These are the strategic lessons — the moments where a different decision would have changed the outcome.

### Alliance and opposition map

How did relationships between players evolve? Who ended up aligned? Who ended up opposed? Were there any surprising partnerships or betrayals?

### Strategic lessons

3–5 concrete lessons from the simulation. Frame these as actionable insights, not abstract observations:
- "The community group's early coalition-building was decisive — by Round 3 they had enough political weight to force a concession"
- "The developer's aggressive timeline backfired because it triggered media scrutiny before planning consent was secured"
- "The campaign group's intervention was most effective when targeted at the regulator rather than the developer"

### Recommended strategy

Based on the full simulation: for the player the user cares about most (ask if unclear), 2–3 concrete strategic recommendations. Not just "what to do" but "when to do it, in what order, and what to watch for."

**Citation rule:** the synthesis must include inline linked citations for every key data point. Format: source name hyperlinked in parentheses.

### So what for you? (if applicable)

If the user's organisation is one of the players (or represented in a coalition), offer a brief "So What For You" section after the synthesis. 2–3 sentences on what the wargame implies for their organisation's strategic positioning, blind spots, or next moves. Frame it as a question to open discussion, not a pronouncement — e.g. *"This raises a question about the campaign group's distinctive role in this fight — is it the technical evidence, the process/democracy angle, or the justice framing? Worth discussing with the team."*

Only include this if the user's organisation was in the game. Don't force it.

## Event injection (optional, powerful)

Between any two rounds, the user can inject an event:
- *"Before Round 3, a national newspaper publishes an investigation"*
- *"In Round 2, the government announces a policy change"*
- *"After Round 1, a key figure resigns"*

When an event is injected, the Control agent incorporates it into the next round's opening context. All players receive it as new information. Log it in the `events` array in the JSON state.

The user can also ask to **play as a specific player** for one round — submitting their own action instead of letting the AI generate it. The Control agent adjudicates as normal.

If no events are injected, just run the rounds straight through.

## Output

Save all output files into whichever project or folder the scenario belongs to, in an `outputs/[short-scenario-slug]/` subfolder. If standalone or ambiguous, save to the current working directory or ask the user where to save. Don't default to this skill's own directory — that's where the skill lives, not where output belongs.

Always produce **four things** (plus an optional fifth):

### 1. Conversation summary

A compressed version in the chat. Show:
- The player table
- Round 1: full opening narrative, all actions, full adjudication
- Rounds 2–4: Control summary + 2–3 highlighted actions + full adjudication
- The full final synthesis

Scannable in under 5 minutes.

### 2. Interactive HTML report

Generate a single-file HTML page (inline CSS and JS, no external dependencies except Tailwind CDN). Filename: `wargame-report-[slug].html`.

Include:
- **Header:** scenario description, number of rounds, number of players, time frame
- **Player cards:** collapsible cards showing each player's attributes and objectives
- **Position timeline:** a visual showing each player's position (advancing/holding/losing/compromised) across all rounds, using coloured indicators (green/amber/orange/red). This is the centrepiece
- **Round-by-round view:** expandable sections showing adjudication narratives and all player actions for each round
- **Synthesis:** full final synthesis at the bottom
- **Sources:** full source library with clickable links

Use Tailwind via CDN (`https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.css`) for styling. Clean and readable.

### 3. Strategic timeline visualisation

Generate a separate single-file HTML page. Filename: `wargame-timeline-[slug].html`.

Dark-background data visualisation with:

1. **Position tracker** — one row per player, coloured dots for each round connected by lines. Shows who's gaining and who's losing at a glance.
2. **Action intensity** — horizontal bars showing resources committed by each player per round. Reveals who's spending heavily and who's conserving.
3. **Turning point callouts** — 2–3 highlighted moments with the player, round, and key quote.

Inline CSS and vanilla JS only. Should look good as a screenshot or screen-share.

### 4. JSON state file

Always save `wargame-state.json`. This is the raw data layer:
- Can be resumed in a later session
- Can be manually edited (swap a player, change objectives, inject events)
- Can be fed into future analysis tools

### 5. Branded Word report (.docx) — *on request only*

If the user asks for a Word doc (e.g. "I need to share this with the team"), generate a branded `.docx` using your organisation's Word template (unpack → replace document.xml → repack, preserving headers/footers/styles). Use the Python/zipfile approach.

The Word report should cover:
- Methodology note
- Scenario (with cited context)
- Player profiles (table)
- Strategic timeline (narrative)
- Outcome assessment (table)
- Turning points
- Strategic lessons
- Recommended strategy
- Source library (clickable hyperlinks)

Use the template's existing styles: Title, Heading1, Heading2, Heading3, ListParagraph. A4 page size.

Filename: `wargame-report-[slug].docx`.

Don't generate this by default — it adds complexity and the user may not need it. The HTML report serves most sharing purposes.

---

## Methodological note

This is a qualitative simulation, not a prediction engine. The wargame is useful for:
- **Stress-testing strategies** before committing resources
- **Surfacing blind spots** — what you haven't considered about other actors' likely responses
- **Identifying turning points** — which moments in a strategic process matter most
- **Building strategic empathy** — understanding why opponents act as they do

It is not useful for: precise probability estimates, quantitative forecasting, or replacing actual stakeholder engagement. The methodology note in reports should make this clear.

Inspired by [Snow Globe](https://github.com/IQTLabs/snowglobe) (IQT Labs) — an LLM-powered qualitative wargaming platform.
