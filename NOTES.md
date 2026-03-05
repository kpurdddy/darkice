# Dark Ice -- Build Notes

## Alpha 1.3.0 (2026-03-04)
- Fix: Multiplayer freeze after goals. conn.send() was executing inside the React state
  updater. WebRTC buffer overflow caused the updater to crash, permanently halting the
  host game loop. Joiner froze simultaneously (depends entirely on host STATE_SYNC).
- Fix: Moved conn.send() out of the updater into a dedicated throttled useEffect.
  Network sync now runs at ~20fps (50ms minimum interval) with try/catch so network
  errors cannot crash game logic.
- Fix: period_break "Tap to continue" used dispatch instead of dispatchAndSend. Joiner
  tapping first had no effect -- host's next sync overwrote the local change.
- Fix: fulltime "Play Again" had the same dispatch bug as period_break. Same fix applied.
- Known issues: CPU skaters have no offensive AI. Pause-to-draw not yet built.

## Alpha 1.2.0 (2026-03-04)
- Fix: Keeper slide rate during shot animation reduced from 0.25 to 0.10 per tick. Keeper now looks like she's diving to intercept rather than receiving a lateral pass.
- Fix: Keeper X movement during shot animation removed. Keeper stays in crease on X axis -- no longer drifts toward end boards during save.
- Fix: Keeper position factor replaced with hard thresholds. 15+ units off aim point: 0.20 factor (cross-crease shot, keeper is out of play). 8-15 units: 0.30. Under 8 units: max(0.50, 1.0 - dist * 0.05). Cross-crease passes setting up shots now pay off when keeper is out of position.
- Cosmetic: stale comment "Dark rink surface" corrected to "White ice surface"
- Known issues: CPU skaters static (no offensive AI). Pause-to-draw system not yet built.
- Version updated to Alpha 1.2.0

## Alpha 1.1.0 (2026-03-04)
- Fix: Tap priority -- teammate tap now checked before goal zone, so tapping a teammate standing near the net passes to them instead of shooting at the keeper
- Fix: Scoring math -- distFactor at 5 units increased from 0.50 to 0.75; save chance multiplier reduced to 0.55; defender proximity radius reduced to 2 units (from 3) for 3v3 density
- Fix: Save animation -- puck now flies toward where player aimed; keeper slides to intercept during animation (not magnetic redirect to keeper position)
- Fix: Check brackets widened -- 85% at 1.0 unit, 55% at 2.0 units (was 80%/50% at tighter thresholds)
- Fix: Save rebounds have velocity -- puck gets gameRNG() * 0.8 velocity after save, creating rebound chances
- Fix: Loose puck pickup requires d < 4 units -- prevents any player on the rink from claiming the puck when looseTimer expires
- Fix: White ice -- rink surface changed from dark (#1a1a2e) to cool blue-white (#e8eef5); markings opacity increased for visibility on white
- Fix: Carrier indicator -- small black puck disc rendered at stick-side edge of carrier's token, replacing soccer-style gold ring
- Version updated to Alpha 1.1.0

## Alpha 1.0.0 (2026-03-03)
- Foundation build -- 3v3 hockey forked from Soccer Mamas engine
- Rink with boards (reflection physics), three periods, period-flip attack direction
- Puck velocity physics: loose puck slides and decelerates (PUCK_DECELERATION), board bounces with energy loss
- Checking system (replaces soccer tackle): same proximity dice roll, same dispossession cooldown
- Two-player local WiFi via PeerJS with darkice- namespace (room codes, host authority model)
- Period flip: home attacks right in periods 1 and 3, left in period 2 (odd/even check)
- Faceoff at center ice (50, 50) after goals and period starts
- Role labels: C, LW, RW, LD, RD, G on all tokens
- Hockey stick rendering: line from player token edge toward puck position
- Goalie clamped to crease (GK_HOME/AWAY X bounds)
- Seeded PRNG (gameRNG) inherited from Soccer Mamas -- zero Math.random() calls
- Known issues: scoring too hard, checks don't connect reliably, CPU skaters static
