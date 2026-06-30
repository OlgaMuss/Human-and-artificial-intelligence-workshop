# H&A Workshop App — Synthesis

## What was built
A real-time interactive workshop tool for 500+ participants, with two pages:
- **`workshop_host.html`** — presenter view (desktop): shows cards, live vote bars, comments, word cloud, session controls
- **`workshop_vote.html`** — participant view (mobile-first): vote on cards, submit comments

## How it works
- **Backend**: Firebase Firestore (project `human-and-ai-workshop`) for real-time sync
- **Hosting**: GitHub Pages on the public repo
- **No build step** — plain HTML/JS, open directly in a browser or via GitHub Pages

## Links (see also LINKS.md)
| What | URL |
|------|-----|
| Participant page | https://olgamuss.github.io/Human-and-artificial-intelligence-workshop/workshop_vote.html |
| Host page | https://olgamuss.github.io/Human-and-artificial-intelligence-workshop/workshop_host.html |
| Firebase console | https://console.firebase.google.com/project/human-and-ai-workshop/firestore |
| Public GitHub repo | https://github.com/OlgaMuss/Human-and-artificial-intelligence-workshop |
| GitHub Pages settings | https://github.com/OlgaMuss/Human-and-artificial-intelligence-workshop/settings/pages |

## Git repos
- **Private** (`everyone`): `https://github.com/OlgaMuss/everyone.git` — full workspace, all files
- **Public** (`Human-and-artificial-intelligence-workshop`): workshop files only, served by GitHub Pages

### To sync public repo after changes
```bash
cp "/path/to/workshop_host.html" /tmp/haiw/
cp "/path/to/workshop_vote.html" /tmp/haiw/
cd /tmp/haiw && git add -A && git commit -m "..." && git push
```
Or re-clone `/tmp/haiw` first if the folder is gone: `git clone https://github.com/OlgaMuss/Human-and-artificial-intelligence-workshop.git /tmp/haiw`

## Local file locations
```
Conferences & webinars/0 - H&A intelligencies workshop/
??? workshop_host.html          ? host/presenter page
??? workshop_vote.html          ? participant page
??? LINKS.md                    ? all links
??? SYNTHESIS.md                ? this file
??? Cards/
    ??? use_case_cards_short/   ? 11 card images used in the workshop
        ??? A8_AI_reduce_screen_time.png
        ??? B2_using_AI_to_help_with_homework.png
        ??? ... (11 total)
```

## Firestore data model
| Collection | Document ID | Contents |
|---|---|---|
| `session` | `main` | Active session state: `sessionId`, `activeCardId`, `todoCards[]`, `doneCards[]`, `revealVotes`, `sessionEnded` |
| `sessions` | `{sessionId}` | Archived past sessions |
| `votes` | `{sessionId}_{cardId}` | Vote counts: `highRisk`, `itDepends`, `beneficial` |
| `comments` | (auto) | `cardId`, `sessionId`, `text`, `ts` |

## Host page features
- **Start/End Session** toggle — starts fresh with first card active; shows overlay when no session
- **Sessions panel** — view and reload past sessions
- **Reveal Votes** — shows/hides vote bars from participants' perspective
- **Mark as Done ? Next** — moves active card to Done pile, activates next
- **Done pile** — click any done card to re-activate and review its votes/comments
- **Word cloud** — live, built from comment text, filters stop words
- **Resource cards** — top bar, click to zoom

## Participant page features
- Sees active card in real time
- Vote: High Risk / It Depends / Beneficial (one vote per session per card)
- Change vote option
- Submit a comment (one per card per session)
- Shows "Thank you" screen when session ends

## Cards in use (11 use-case cards)
```
A8  AI to reduce screen time
B2  Using AI to help with homework        ? starts as active card
B6  Using AI to learn a foreign language
B7  AI to choose who to go out with
C3  AI to interpret baby cries
C4  AI personalized curriculum
C5  AI bedtime story podcasts
D1  AI to practice difficult conversations
D4  AI companion to chat
D6  AI robot emotional bonding
D7  AI stuffed animals to calm babies
```

## Known issues resolved
- Sessions panel not opening ? was injected into wrong `<script>` tag; fixed
- Old session comments leaking ? filter now reads `sessionId` live from Firestore
- Card images missing on GitHub Pages ? images were gitignored, now force-added
- No-session overlay blocking Sessions button ? z-index raised; button added to overlay
- Start Session loading previous session ? now auto-activates first card; overlay shows when idle
