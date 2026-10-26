# NCAA Basketball Pool Rankings

## Goal
Build a ranked list of Division I NCAA basketball teams to win the user's annual draft pool.

## Pool Rules
1. **Win condition:** The participant with the most collective total wins across their drafted teams is champion.
2. **Draft:** ~15 participants each pick 12 teams in a snake draft (~180 of ~360 D1 teams get drafted).
3. **Eligible teams:** Any Division I program.
4. **Duration counted:** Full regular season + all playoffs (conference tournaments + NCAA Tournament + NIT).

## Deliverable
A single-file, printable HTML page (`rankings.html`) with:
- A straight numbered list of all ranked D1 teams
- Tier dividers: **Elite / Strong / Mid / Weak**
- Conference next to each team
- Per-team factor scores displayed inline (user asked for "Option B" full detail)
- Click-to-strikethrough during live draft
- Algorithm legend at the bottom

User prints the page; strike-through is on-screen only (not needed on paper).

## Algorithm v2 (current)

Composite score (0–10 scale per factor, output ×10 as /100):

| Factor | Weight | Notes |
|---|---|---|
| Projected Wins (2026-27) | 20% | Forward-looking — anchored to KenPom/Torvik preseason |
| Returning Players | 17% | Minutes/production returning from prior season |
| Hidden Gem Potential | 15% | See GEM explanation below |
| Transfer Portal Class | 10% | **NEW in v2** — 2026 portal haul rank |
| Coaching Stability | 10% | New 2026 hires penalized |
| Senior Starters | 8% | Experience in close games |
| Strength of Schedule | 8% | Quality of competition |
| Recruiting Class (HS) | 7% | Reduced from v1 since portal matters more now |
| Injuries | 5% | 10 = fully healthy |

**Tiebreaker:** Projected 2026-27 win total.

**Score formula:**
```
score = p*0.20 + r*0.17 + g*0.15 + t*0.10 + h*0.10 + s*0.08 + q*0.08 + e*0.07 + i*0.05
```

Field keys in the JS data array (`T`):
- `n` name, `c` conference
- `p` projWins, `r` returning, `g` gem, `t` transferPortal, `h` coaching, `s` seniors, `q` sos, `e` recruiting, `i` injuries
- `xp` projected 2026-27 win total (shown + used as tiebreaker)
- `xw` actual 2025-26 win total (shown for context)

**Tier thresholds** (composite ×10):
- Elite ≥ 65
- Strong 50–64
- Mid 35–49
- Weak < 35

## Key Concepts

### ★GEM (Hidden Gem)
Teams likely to pile up 20+ wins but ranked outside the national top 25, usually in weaker conferences. These are the highest-leverage late-round draft picks because other drafters chase name recognition.

Scoring:
- 0 → top-25 team (no gem value)
- 5–6 → decent mid-major, 18–20 wins
- 7–8 → strong mid-major, 22–24 wins in a weak conference
- 9–10 → elite mid-major, analytics darling, 25+ projected wins

Teams scoring ≥8 display a **★GEM** tag.

**Draft strategy:** Let other players chase blue bloods early; stack 2–3 GEMs in rounds 6–10.

### ♦PORTAL (Transfer Portal Class)
Modern college basketball transfer portal moves more impactful talent than HS recruiting. Teams scoring ≥8 display a **♦PORTAL** tag. 2026 leaders: Tennessee, Louisville, Texas, Indiana, Xavier, Miami, BYU, USC.

## Data Sources
- **Analytics:** KenPom and Bart Torvik 2026-27 preseason rankings
- **Transfer portal:** CBS Sports, ESPN, On3 (2026 cycle)
- **Prior season:** 2025-26 final records (Michigan won title 37-3 over UConn)
- **Coaching changes:** 2026 carousel — new hires at UNC (Michael Malone), Villanova (Kevin Willard), Miami FL (Jai Lucas), Utah (Alex Jensen), Florida State (Luke Loucks), plus firings at Kansas State, Georgia Tech, NC State, Minnesota, Iowa

## File Structure
- `rankings.html` — single-file app (HTML + CSS + JS data + render logic)
- `CLAUDE.md` — this file
- `.git/` — version control (pushed to `github.com/jjtharrisonjames/09-Basketballpool`)

## Design Decisions (locked in with user)
- **Format:** straight numbered list with horizontal tier dividers (not grouped by tier)
- **Detail level:** full per-factor scores shown inline per team ("Option B")
- **Conference:** displayed next to each team
- **Use case:** printed on paper for the draft
- **Draft tracking:** strike-through only, no need to log who drafted each team
- **Weights at bottom:** yes, with full algorithm legend including GEM/PORTAL explainers

## When Updating

If asked to adjust rankings, factors, or data:
1. Edit the `T` array in `rankings.html` (JS section)
2. Verify score formula and weights match the legend table
3. Open the file in a browser to spot-check sorting
4. Commit with a clear message describing the algorithm change

If adding a new factor:
1. Update weights to sum to 100%
2. Add the factor to the weights table in the legend
3. Update the field keys comment in the JS
4. Update the score formula in the `score()` function
5. Update the per-team detail row (`row2`) to show the new field

## User Preferences
- Prefers concise answers; dislikes long preambles.
- Will ask for Option A (cleaner view) if the full-detail Option B gets too noisy.
- Uses `! <command>` to run shell commands in the prompt.
- Repo lives at `/Users/lindsayharrison/Documents/projects/09-Basketballpool`.
