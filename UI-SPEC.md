# KickOff — UI Design Spec

A watch party host tool for friends watching sports together — **in person** and **remotely**. Mobile-first (primary target: 430px wide phone). Two roles: **Host** (pays per game) and **Guest** (free).

---

## Design System / Global

**Layout**
- Mobile-first, max content width ~430px, single column
- Persistent **bottom tab bar** on top-level screens (Parties, History)
- **Top bar** on every screen: back/title left, status/avatar right
- Floating **+ button** (FAB) on Parties & History to create a party

**Color tokens**
- Background `#0d1117` / Panel `#161b22` / Panel-alt `#1c232c`
- Border `#2a323c`
- Text `#e6edf3` / Dimmed text `#8b949e`
- Accent (brand) `#f0883e` (orange)
- Green (in person / paid / success) `#2ea043`
- Blue (remote) `#388bfd`
- Yellow (pending / awaiting payment) `#d29922`
- Red (can't attend / error) `#da3633`
- Purple (accent secondary) `#a371f7`

**Typography**
- System sans-serif. Bold weights for names/stats. Uppercase letter-spaced section headers (12-15px).

**Reusable components**
- Card (rounded 16px, bordered panel)
- Match card (gradient hero: flag/emoji + VS + team names + kickoff time)
- Status pill (green/yellow/red/blue/purple variants)
- Avatar (circle, first initial)
- Segmented control (e.g. In person / Remote / Can't)
- List row (avatar + name + sub-text + trailing element)
- Stat block (big number + small label)
- Link box (dashed border, shareable URL)
- Bottom sheet / modal
- Primary / ghost / colored buttons

---

## Roles & Access Logic

| State | Effect on UI |
|-------|--------------|
| Party created, **unpaid** | Invite link locked. Pill: "Pay to share". Lives in "Awaiting payment". |
| Host **pays** per-game fee | Link unlocks. Full app enabled for that game. Pill: "Live access / Unlocked". |
| Guest opens link | Sees invite, RSVPs free, downloads app free, full feature access while host's game is paid. |
| Game ends | Party moves to **History**, stays browsable forever (read-only summary). |

---

## SCREEN 1 — Parties (Home)
**Purpose:** Host's landing screen listing their watch parties.
- Top bar: brand "KickOff" + tagline "Host. Invite. Watch together." + user avatar
- Section "Awaiting payment" (if drafts exist): unpaid party cards, yellow "Pay to share" pill -> resume payment
- Section "Your watch parties": paid parties; card shows team emojis, "Live access" pill, "Team A vs Team B", kickoff time, count going
- Empty state: soccer icon + "No parties yet. Tap + to host one."
- FAB + -> Create Party
- Bottom tabs: Parties / History

## SCREEN 2 — Create Party (Host, Step 1: Pick)
**Purpose:** Host selects the match.
- Top bar: "< Host a party"
- Match hero card (live preview of selection)
- Dropdown: pick match (team names + competition round)
- Fee card: "Per-game host fee - $X.XX" + explainer
- Primary button: "Continue to payment"
- Match data: team A/B name, flag/emoji, competition/round, kickoff date-time (timezone-aware)

## SCREEN 3 — Payment (Host, Step 2)
**Purpose:** Per-game payment gate. Unlocks invite link.
- Top bar: "< Host a party"
- Confirmation card: card icon + match name + kickoff time
- Payment form: card number, expiry, CVC
- Divider + "Total due today: $X.XX"
- Primary button (green): "Pay $X.XX & unlock invite"
- Fine print / demo notice
- Real build: Stripe; link generation gated on successful charge

## SCREEN 4 — Success / Share (Host, Step 3)
**Purpose:** Confirm payment, surface link.
- Success card: check icon + "You're all set!" + "Full app unlocked for [match]."
- Share card: dashed link box + "Copy link" (blue) + "Preview what guests see" (ghost) + explainer
- Primary button: "Open party ->"

## SCREEN 5 — Invite (Guest-facing RSVP)
**Purpose:** What a guest sees opening the link. Free, no login to RSVP.
- Top bar: "< Invite preview" + "FREE FOR GUESTS" badge
- Match hero card with "[Host] invited you to a watch party"
- RSVP form:
  - Text input: "Your name"
  - Segmented control: In person / Remote / Can't
  - If In person: input "What will you bring? (food / drinks)"
  - If Remote: info "You'll join the live vibe chat & bracket from home - synced with everyone."
  - Primary button: "Join the party" (or "Send response" if Can't)
- Confirmation state: celebration + status + download prompt -> back

## SCREEN 6 — Party Dashboard (Tab: Party)
**Purpose:** Home for a single party; entry to 5-tab experience.
- Top bar: "< [Team A] v [Team B]" + "Unlocked" pill
- Match hero card
- Stats card (4 blocks): in-person, remote, predictions, bringing food
- Invite link card: link box + Copy + Preview + explainer
- Bottom tab bar (5): Party / Crew / Food / Bracket / Vibe

## SCREEN 7 — Crew (Tab)
**Purpose:** Guest list with attendance type.
- Crew list: avatar + name (+ "(you)") + food sub-text + RSVP pill (In person green / Remote blue / Can't red)
- Add-someone card: name input + segmented control + "Add to crew"

## SCREEN 8 — Food (Tab)
**Purpose:** Who brings what (in-person only) + ordering/splitting.
- "Who's bringing what": row per in-person guest with editable "Bringing..." field
- Empty state: "No in-person guests yet."
- Group order: DoorDash (red) + Uber Eats (green) buttons
- Split cost: Venmo / PayPal / Cash App
- Real build: affiliate/deep-link integrations; payment handles

## SCREEN 9 — Bracket (Tab)
**Purpose:** Group score-prediction pool (in person + remote).
- Match hero card: "Predict the final score"
- Lock card: current pick status + score input ("2-1") + "Lock it in"
- Pool: row per predictor = avatar + name + (remote/in person) + predicted score
- Rules: "Exact score = 3 pts, correct result = 1 pt. Auto-scored after full time."
- Real build: auto-scoring via live result feed

## SCREEN 10 — Vibe (Tab)
**Purpose:** Live reaction feed shared by all attendees in real time.
- Banner: "LIVE vibe - everyone in person and remote sees this in real time."
- Feed: reverse-chronological; item = avatar + name + emoji + optional text bubble
- Empty state: "No reactions yet. Start the hype."
- Sticky composer: emoji picker row + text input "Hot take..." + Send
- Real build: realtime backend (websockets)

## SCREEN 11 — History (Bottom tab)
**Purpose:** Permanent record of past parties.
- Top bar: "History" + count
- Card per past party: team emojis + "Team A vs Team B" + kickoff date + chevron -> read-only party
- Divider + stats row: attended / predictions / reactions
- Empty state: "Your past parties will live here forever."

---

## Core User Flows

**Host:** Parties -> + -> Create (pick match) -> Payment -> Success/Share -> copy & send link -> Open party -> manage Crew/Food/Bracket/Vibe -> game ends -> History.

**Guest:** Receives link -> Invite screen -> RSVP (in person / remote / can't) + food -> download app (free) -> Party tabs (Bracket, Vibe, Food) before/during/after -> History.

---

## Data Model (for backend later)

```
User       { id, name, avatar }
Party      { id, hostId, matchId, paid (bool), link, createdAt }
Match      { id, teamA, teamB, flagA, flagB, competition, kickoffAt }
Guest      { id, partyId, userId/name, rsvp: in|remote|out, food }
Prediction { id, partyId, guestId, score }      // "2-1"
Vibe       { id, partyId, userId/name, emoji, text, createdAt }
Payment    { id, partyId, hostId, amount, status, createdAt }
```
