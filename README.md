# War Room Wager

A live, multi-device points-wagering quiz round for college quiz competitions.
Host runs the dashboard on a projector/laptop; teams join on their own phones using a 6-digit code.

## How it works

1. **Lobby** — Host clicks *Join as Host* → gets a 6-digit code. Teams enter a name + the code to join. Host sees the live roster.
2. **Wager window (20s)** — Host clicks *Start Quiz*. The broad topic appears for everyone; the question stays hidden. Teams have 20 seconds to wager 1–10 points (two rows: 1–5, 6–10). Each number can only be used once per team for the whole game — used numbers show struck-through. If time runs out, the server auto-locks each team's lowest unused number.
3. **Answer window (60s)** — Host clicks *Broadcast Question*. The full question text appears to teams, and a 60-second timer starts. Teams type and submit a written answer. If time runs out, unanswered teams get `[No Answer Submitted]` automatically. Teams never see other teams' answers or the correct answer.
4. **Host evaluation** — The host dashboard shows every team's wager and typed answer live, next to the hidden correct-answer benchmark. The host clicks ✓ Correct or ✗ Wrong per team; correct adds their wagered points to their score. Host then clicks *Advance to Next Topic*.
5. **Pause/Resume/End** — Host can pause at any point (freezes timers and shows an overlay to teams), resume, or end the quiz early to show final standings.

10 MBA-level, long-format clue questions are preloaded in `server.js` (edit the `QUESTIONS` array to change them).

## Run locally

```bash
npm install
npm start
```

Visit `http://localhost:3000`. Open a second tab/incognito window to test host + team simultaneously.

## Deploy — GitHub + Render

### 1. Push to GitHub

```bash
cd war-room-wager
git init
git add .
git commit -m "War Room Wager quiz app"
git branch -M main
git remote add origin https://github.com/<your-username>/war-room-wager.git
git push -u origin main
```

(Create the empty repo on GitHub first, e.g. at `github.com/new`, then run the commands above.)

### 2. Deploy on Render

1. Go to [render.com](https://render.com) → **New** → **Web Service**.
2. Connect your GitHub account and select the `war-room-wager` repo.
3. Configure:
   - **Environment**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Instance Type**: Free is fine for a demo/event.
4. Click **Create Web Service**. Render will build and deploy automatically.
5. Once live, you'll get a URL like `https://war-room-wager.onrender.com` — that's your quiz link.

Every time you push to `main`, Render redeploys automatically.

### Notes

- This app keeps game state **in memory on the server** — one live game at a time, which is exactly what you need for a single-event competition. If the server restarts (e.g. Render's free tier spins down after inactivity), any in-progress game resets — start a fresh game right before the round begins.
- No database, no sign-in required for teams — they just need the link and the code.
- To change the number of questions, the timer lengths, or the question bank, edit the `QUESTIONS`, `WAGER_SECONDS`, and `ANSWER_SECONDS` constants at the top of `server.js`.
