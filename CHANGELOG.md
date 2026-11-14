# Changelog

Unofficial That Sky Mod (TSM) builds for Sky: Children of the Light on Android (Canvas / Sky Modloader).
All credits for the mod go to the original TSM makers: **XeTrinityz and the TSM team**
([XeTrinityz/ThatSkyMod-Android](https://github.com/XeTrinityz/ThatSkyMod-Android)). See the README for full credits.

This list covers every change made in the source repository (That-Sky-Mod-Temp-Updates, private),
grouped by version. "Behind the scenes" items don't change how the mod works in the game.

---

## 0.26.4 — Sky 0.35.2 (2026-11-14)

- **New: Change Server** (Teleport → Dynamic). A log showed that Reload Level keeps the same game server (no new
  connection between three reloads), and that a new server is only assigned after the shared connection drops
  (in a private place). Change Server goes Home (TTitleGate, like the Home button), waits 4 s, changes back to the
  same level and teleports you to where you stood. Runs in its own thread, so it works with the menu closed.
- **New: Apartment Hopping** (Teleport → Dynamic, off at start). When you enter an apartment without joining a
  friend, the game sends you to your own apartment ("boot self", bl 0x1A7BB6C at 0x1DF9CD0). The switch skips that
  call (original word checked first, put back when off), so Change Server can take you through other players'
  apartments.
- **Skip Spirit Tree Prerequisites** (Unlocks): new description. The switch only changes what the game lets you
  select; buying a node out of order or above your tier is refused by the server, and the game then shows
  "game data corrupted" (restart the game to clear it). Reported in game on 2026-11-14.

## 0.26.3 — Sky 0.35.2 (2026-11-14)

- **Fix: Relog didn't do anything** (Wing Light "Relog now", Settings → Account). The account fields were 0.33.7
  values (0x1430 / 0x143C / 0x1440) and wrote into other fields of the account object in 0.35.2. The game's own
  logout (0xDFC110) and account switch (0xE020A0) show the 0.35.2 layout: loading state 0x14B4 (4 bytes; 0 starts
  the login again, 0x1C = loaded), login type 0x14C0 (4 bytes; logged out is now 14, was 12), "should restart"
  byte 0x14C4. TSM now uses those (4-byte writes for the two state fields). Also fixes account switching and new
  local accounts. Login types 12 and 13 (added after 0.33.7) show as "Other".

## 0.26.2 — Sky 0.35.2 (2026-11-14)

- **Fix: TSM's server requests all failed with "400 Bad Request".** `ApiClient` put `Host` and `User-Agent` in the
  request headers and `HttpClient` added its own copies, so every request carried two `Host` headers; the server
  rejects that ("Invalid :authority", checked against the live server). Defaults are now only added when the caller
  hasn't set them, so the game's own User-Agent is the one sent. This fixes Wing Light collect / drop / refresh and
  every other feature that uses the account API (quests, collectibles, shop, social, Eden, daily quests).
- **Fix: Auto Burn Plants / Auto Burn Dyes wouldn't turn on.** Their addresses (0x15BAF90 / 0x15BB004 / 0x15BB07C,
  from TheVoid) were never moved to Sky 0.35.2, so the instruction check found nothing and the switch stayed off.
  The same `ldr s0, [xN, #0x248]` instructions are at 0x15BB0C0 / 0x15BB134 / 0x15BB1AC in 0.35.2 (+0x130).
- **Auto Wax Tools run turns on the burn switches.** Starting a run switches on Auto Burn Candles (and Auto Burn
  Plants with "Include plants"), so they show as on; the run's own copies of those patches then find the code
  already patched and add nothing. Finishing or canceling puts both switches back to their earlier state.

## 0.26.1 — Sky 0.35.2 (2026-11-14)

- **New: Skip Spirit Tree Prerequisites** (Unlocks → Temporary Unlocks), ported from Offset Lab 0.37.7 (confirmed
  working in game on a seasonal spirit). It changes the game's "can this node be bought", "is the node before it
  owned" and seasonal tier checks (13 code words, each checked against Sky 0.35.2 first; if any doesn't match,
  nothing changes) and sets the game's tier byte 0x34B4600. Not saved: off every time the game starts. Purchases
  still go to the server.

## 0.26.0 — Sky 0.35.2 (2026-11-13)

- **Rainbow Trails** (Player) works again: it gives you the game's `trail_rainbow` spell on your screen (10 hours;
  turning it off removes it), as Offset Lab does. The old switch had no game address since Sky 0.35.1.
- **Bubble Trails** (Player) works again as a substitute: turning it on places the game's bubble zone
  (HubbaBubbaZone, size 5) where you stand, as Offset Lab does; it stays until you change level.
- **Fix:** Disable Joinable (Social) wrote to address 0, the start of the game library, because its game address
  wasn't found for 0.35.1 / 0.35.2. It now skips that write; its request to the server is unchanged.
- (0.25.10 was built but not released; its fixes are in 0.26.0.)

## 0.25.9 — Sky 0.35.2 (2026-11-13)

- **Unlock All Relationship Abilities** (Unlocks) now also unlocks the friendship abilities for **strangers**:
  the player menu's "is this player my friend" check answers yes, so a player you haven't befriended gets the
  same abilities a friend does. Ported from Offset Lab (one instruction at 0x1D97D6C, checked before it's
  patched and put back when the toggle is turned off).

## 0.25.8 — Sky 0.35.2 (2026-11-13)

- **Fix:** "Shout Cry" and "Shout Heart" (Player → Expressions) played each other's reaction. The game's shout
  types are 1 Heart, 2 Shock, 3 Touched (cry), 4 Light; the two buttons now send 3 and 1. Found while checking the
  same types in Offset Lab (type 0, Default, crashes the game and is never sent).

## 0.25.7 — Sky 0.35.2 (2026-11-01)

- Updates now come from this public repository (That-Sky-Mod-RE): tap the mod's update button in Canvas
  to get new versions. Install 0.25.7 by hand once; later versions arrive through Canvas.

**Behind the scenes**
- Mod's update link changed to `books4jy-art/That-Sky-Mod-RE` releases.
- Releases are published by a workflow in this repository when `libTSM.so` and `VERSION` change.
- The release workflow in the source repository was removed, so releases come from one place only.
- Source README: new "Publishing an update" steps.

## 0.25.6 — Sky 0.35.2 (2026-11-01)

**Official game names**
- Spells and spirits are shown by their real in-game names instead of internal codes
  (e.g. `rabbit_form` → Rabbit Form, `teleport_forest_elder` → Elder of Forest).
- Used in the spell list, the Forms / Outfit Spells sections and the Spirit Shops lists.
- Names for 181 spells and 387 spirits, taken from the game's own text in English and Korean.
- Oreo Form has no Korean name in the game yet; 오레오 형태 is used.

**Korean menu** (Settings → Language & Translations → **한국어 (Korean)**)
- Built-in Korean translation of about 1,150 menu texts.
- Realm, teleport, season and remaining menu labels translated, including the spell type filter.
- Terms revised three times, ending on the game's own official Korean:
  1. Sky Planner terms (영혼 for spirits, spirit names, 메너티).
  2. Community glossary terms (날개빛, 지식의 도서관, 제스처, 영혼의 문).
  3. The game's own text, where it differs: 다크 드래곤 (Krill), 하늘양초 (Ascended Candle),
     여명의 섬 (Isle of Dawn), 안식처 (Home), 어둠의 식물, 어둠의 파편, 일일퀘스트, 시즌양초, 시즌패스,
     의상 주문, 은둔자의 언덕, 새들의 정원 마을 and others (53 texts).
- Korean letters are drawn with a Korean font from the phone (Noto CJK, Samsung, Nanum or Droid),
  because the loader's own font has none. Only used while Korean is selected.

**Updates**
- First version that could update through Canvas (from the source repository; replaced by 0.25.7).
- Before this, the mod's update link pointed at Catyro's repository.

**Behind the scenes**
- Source and README updated for Sky 0.35.2; `release/libTSM.so` replaced with the 0.35.2 build.
- Build re-embeds the mod config whenever it changes (version bumps).
- Local build copies are no longer tracked.

## 0.25.5 — Sky 0.35.2 (2026-10-31)

**Game update**
- All 94 game addresses ported from Sky 0.35.1 to **0.35.2**.

**TSM login**
- TSM login restored, so features that need it can work again: auto / semi winged light,
  auto / semi Eden, daily quests and events.
- Every TSM server request is written to the log, to find what still fails.
- Progression features whose game address isn't found are skipped instead of breaking.

**Spells tab**
- Bunny, Oreo, Chibi quick forms fixed: exact spell names are matched first, and Bunny no longer picks
  the Crabbit door spell. Wider match for Oreo.
- Spell Sharing section removed.
- The long form spell list between Form Duration and Outfit Spells is hidden.
- Spell list sorted by name, with a type filter: All / Forms / Outfits / Glow / Trails / Other.
- When the TSM server's spell list isn't available, the game's own spell list is used.

**Semi Candle Run**
- Only candles that give wax are listed (strict check). Several filter experiments in between —
  either flag, an "Include Extra Candles" toggle, filters A–D, adding candles from groups not loaded
  yet — were tried and removed; Filter A (strict) is kept.

**Music**
- Sheet playback plays chord notes together.

**Known issues**
- Semi Eden may not deposit or collect winged light.
- Reload Daily Quests may fail.

## 0.25.4 — Sky 0.35.1 (2026-10-31)

**Game update**
- First update of the mod for **Sky 0.35.1** (build 423272), with game addresses re-found.
- Restored: teleports (candles, dark plants, dyes, winged light), camera (Free Zoom, FPV, Drone),
  World group, relationship abilities, Anti-AFK, Never Drown, Ethereal Avatar, Broken Neck,
  social / ESP lists.

**Auto Complete Quests**
- Finishes open daily quests, including waiting / timed quests like "watch the tree".
- Keeps completing new quests while on, and retries on every level change.

**Spells tab**
- Works without TSM login: the spell list is read from the game itself. Placeables are no longer
  behind the login screen.
- **Forms** and **Outfit Spells** sections: bunny, oreo, chibi, crab and outfit spells with a chosen
  duration (10–600 s), live countdown, early end and automatic expiry; one spell per section.
- Quick Bunny / Oreo / Crab / Chibi buttons; spell tracking clears when you leave the session.
- Both sections sit inside the scrolling spell list, so the other spells can still be reached.
- Star pin spells are left out of these sections. Some forms are visible to other players;
  outfit spells only to you.

**Not working in this version**
- Bubble / Rainbow Trails (no longer in the game), Pause UI Animations.
- Features that need the TSM login (restored in 0.25.5).

**Behind the scenes**
- README with install steps, feature status and build instructions.
- Update notes with the full address table; mod config set to 0.25.4.
- An extra, duplicate Forms module was merged into the existing form spells feature.
