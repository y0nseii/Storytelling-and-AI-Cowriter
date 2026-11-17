# Loomwright 🧵
**A co-writer that remembers your universe and never contradicts itself.**

Loomwright is an AI storytelling assistant that helps authors build a world, branch into parallel universes, write in-character dialogue, and play through an interactive narrative, while keeping every fact consistent.



## The problem
AI writing tools forget characters, break the rules of the world, and contradict themselves. Writers end up re-explaining their story every time they open a new chat.

## Our solution
Loomwright gives the AI a memory it can't lose:
- A structured **World Bible** (characters, locations, rules, quest) sent with every request
- A **running summary** of the story so far
- **Game state** (inventory, relationships, flags) tracked in Python code, not by the AI
- A **consistency checker** that flags contradictions against the World Bible

## Features
| Feature | What it does |
|---|---|
| World Bible generator | Turns a genre and premise into structured JSON: characters, rules, locations, quest |
| Interactive scenes | Writes a scene and offers 3 choices that continue the story |
| State tracker | Inventory, relationships, and flags update after every choice |
| "What if…" multiverse | Branches the story into a parallel universe that still obeys the world's core rules |
| Dialogue studio | Writes short exchanges in a character's own voice, using their goals and secrets as subtext |
| Story map | A graph of scenes, with one column per universe |
| Consistency checker | A second AI call that compares the latest scene to the World Bible |

## How it works
1. The user picks a genre and premise.
2. Claude returns a **World Bible** as JSON.
3. For each scene, the backend sends Claude: World Bible + story summary + current state + the player's choice.
4. Claude replies in JSON: `scene_text`, `choices`, `state_changes`, `summary`.
5. Our Python code (`apply_state`) applies the state changes. **The AI never owns the state.**
6. The frontend renders the scene, choices, state sidebar, and story map.

```
Player choice
     │
     ▼
 Frontend (HTML/JS) ──► FastAPI (main.py) ──► Claude API
     ▲                        │                    │
     │                        ▼                    ▼
     └──── scene + new state ◄── apply_state() ◄── JSON reply
```

## Tech stack
- **Language:** Python 3.10+
- **Backend:** FastAPI, Uvicorn, Pydantic
- **AI:** Claude API via the `anthropic` Python SDK (Sonnet for scenes and world-building, Haiku for quick checks)
- **Frontend:** HTML, CSS, and vanilla JavaScript (no build step), with an SVG story map
- **Config:** `python-dotenv` for the API key



