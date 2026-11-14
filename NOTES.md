Unofficial That Sky Mod update for Sky 0.35.2 (Android, Canvas / Sky Modloader).

- **Fix: every TSM feature that talks to the game's server works again** (Wing Light collect/drop/refresh, quests,
  collectibles, shop, social, Eden, daily quests). TSM sent each request with the Host header twice, and the server
  rejects that with "400 Bad Request", so all of them failed (for example "Wing Light collect failed").
- **Fix: Auto Burn Plants and Auto Burn Dyes** (Progression) turn on again. Their three game addresses were from an
  older Sky version; they now point at the same code in Sky 0.35.2.
- **Auto Wax Tools run** (Progression → Wax) now turns on **Auto Burn Candles**, and **Auto Burn Plants** when
  "Include plants" is on, while it runs. When the run finishes or is canceled, both go back to how you had them.

That Sky Mod is made by XeTrinityz and the TSM team. Unofficial build; see the README for credits.
