# KickOff — Watch Party Host

A **party host tool** for watching sports together — for friends who gather **in person** *and* friends who join **remotely** from home.

## Run it

It's a self-contained prototype. No build step:

```bash
open index.html        # macOS
# or just double-click index.html, or serve it:
python3 -m http.server 8000   # then visit http://localhost:8000
```

State persists in your browser via `localStorage`, so your parties and history stick around between sessions.

## The flow

1. **Host creates a party** — picks a match from the schedule.
2. **Host pays a per-game fee** ($7.99 demo) — this unlocks the full app for that game.
3. **Host shares the invite link** — once paid, the link is live.
4. **Friends open the link** — they say whether they're coming **🏠 in person**, watching **📺 remotely**, or **can't make it**, and (if in person) pick what food/drinks they'll bring.
5. **Attendees use the app free** — bracket, food coordination, and a live "vibe" feed shared by in-person *and* remote guests, before / during / after the game.
6. **History is kept** — every past party (attendance, predictions, reactions) stays browsable in the History tab.

## Screens

| Screen | What it does |
|--------|--------------|
| **Parties / Create / Pay** | Host setup + per-game payment gate that unlocks the invite link |
| **Invite** | Guest-facing RSVP — in person / remote / can't, plus food |
| **⚽ Party** | Dashboard: in-person vs remote counts, food, invite link |
| **👥 Crew** | Guest list with RSVP type (in person / remote / out) |
| **🍕 Food** | Who's bringing what + group order & split-pay shortcuts |
| **🏆 Bracket** | Score prediction pool for everyone, in person or remote |
| **🔥 Vibe** | Live reactions shared by all attendees in real time |
| **🕘 History** | Every past party, kept forever |

## Notes for the real build

- Invite link should be a real web URL so guests can RSVP without installing the app.
- Match schedule needs a live sports API feed.
- Payment needs a real processor (Stripe) gating link generation per game.
- Live vibe / sync needs a realtime backend (e.g. websockets) so in-person and remote guests stay in sync.
- History needs server-side persistence and per-user accounts.
