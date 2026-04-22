## Plan: Flip Game Improvement Roadmap

Improve short-session engagement and replayability first (quick wins), then add fairness and depth features that preserve the current minimalist style.

**Steps**
1. Phase 1: Engagement Quick Wins (can run in parallel)
1. Add persistent high score using localStorage and show it on menu/death overlay (parallel with step 2).
2. Add pause/resume state and visible pause control; ensure timers and spawn logic halt while paused (parallel with step 1).
3. Add lightweight sound effects for flip/coin/death via Web Audio API with mute toggle (parallel with step 1).
4. Add a score sharing action (copy-to-clipboard text) on the death overlay (depends on step 1 for high-score context).
5. Phase 2: Fairness and Readability (depends on phase 1)
6. Cap late-game speed ramp and spawn density so difficulty stays hard-but-fair rather than impossible.
7. Add optional hitbox debug overlay and tune collision margins using real gameplay checks.
8. Improve onboarding hints with short contextual prompts for first coin and first dodge.
9. Phase 3: Gameplay Depth (depends on phase 2)
10. Add one new obstacle archetype (for example double-side or moving spike) with gradual introduction.
11. Add rare power-up prototype (shield or slow-motion) and constrain spawn rates to keep core loop intact.
12. Phase 4: Replayability and Identity (depends on phase 3)
13. Add milestone unlocks (cosmetic player variants/themes) tied to score thresholds.
14. Add post-run stats panel (spikes dodged, coins collected, best run, run duration).

**Implementation Kickoff (Phase 1)**
1. Slice A: High score persistence (localStorage) and UI display on menu/death card.
- Acceptance: best score survives reload; updates only when current score exceeds previous best.
2. Slice B: Pause/resume with explicit state handling.
- Acceptance: no spikes/coins/time progression while paused; resume returns to same frame state.
3. Slice C: Sound effects and mute toggle.
- Acceptance: flip/coin/death cues fire exactly once per event; mute persists for current session.
4. Slice D: Share score action.
- Acceptance: clipboard text includes score and best score; fallback message shown if clipboard API unavailable.
5. Regression gate before merge.
- Acceptance: Space/tap flip unchanged, level/theme transitions unchanged, score math unchanged (spike +1, coin +3).

**Relevant files**
- `c:/Code Projects/flip/index.html` — core game loop and systems to extend: `getSpeed`, `getSpawnInterval`, `spawnSpike`, `checkCollisions`, `die`, `startGame`, overlay UI wiring.

**Verification**
1. Manual run test on desktop: verify controls, pause/resume, audio toggle, score persistence, and share text behavior.
2. Manual run test on mobile/touch viewport: confirm tap flip reliability and overlay button usability.
3. Fairness checks: survive test runs to late-game and confirm obstacle cadence remains reactable.
4. Regression checks: ensure existing theme transitions, level badge behavior, and score increments still match current mechanics.

**Decisions**
- In scope: ideas prioritized for a single-file web game with minimal dependencies.
- Out of scope for now: backend leaderboard, account systems, monetization.
- Preserve current visual language and fast restart loop as core identity.

**Approval**
- Approved by user on 2026-04-22 to execute all phases in sequence.

**Execution Order**
1. Implement Phase 1 slices A-D, then run Phase 1 verification gate.
2. Implement Phase 2 balancing and onboarding, then run fairness-focused verification.
3. Implement Phase 3 obstacle and power-up additions, then run collision and spawn regression checks.
4. Implement Phase 4 cosmetics and post-run stats, then run full regression and mobile usability pass.

**Handoff Notes**
- Execute in one branch with small commits per slice to reduce rollback risk.
- Prefer feature flags for debug hitboxes and new obstacle/power-up toggles during tuning.
- Keep all new state centralized near existing globals in index.html to avoid scattered side effects.