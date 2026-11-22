# Dice Drop mobile web rollout

Dice Drop is in beta. The game ships through GitHub Actions to GitHub Pages and saves progress on the device. The proposed rollout adds two global leaderboards, optional Google sign-in, and one ad opportunity between worlds. Players should be able to finish a level and save their win even when a network service is unavailable.

## Release sequence

1. **Protect existing saves.** Pick the public game domain before rolling out accounts. If the origin changes, offer a tested save transfer while the old site is still available. Give rankings, sign-in, and ads independent release switches.
2. **Pilot both boards with beta testers.** Use a small Render API backed by Neon, the selected managed Postgres provider. Give each tester an anonymous player ID. Save wins locally first and queue them for submission. The API keeps each player’s best stars per level, so retries and lower-scoring replays cannot inflate or reduce a total.
3. **Link accounts when ready.** Offer Google sign-in for players who want to recover ranked progress on another device. Verify the Google ID token on Render and link its stable subject identifier to the existing player. Show a public nickname. When linking two progress records, keep the better result for each level.
4. **Open global rankings.** **Most Stars** sums each player’s best star count per level. **Highest Level** counts the highest campaign level cleared. Tied players share a rank. The proposed launch policy preserves existing beta saves for campaign play and counts eligible clears submitted from the start of public rankings. Validate level order and star bounds, rate-limit submissions, and keep database credentials on Render. These checks still rely on the client to report plausible results.
5. **Pilot between-world ads.** Save the clear and show the world-complete celebration. When the player taps **Enter Next World** for the first time, request one interstitial. Revisits skip the ad. If an ad is unavailable or fails to load, continue into the next world. Start with test ads and confirm provider approval and applicable consent requirements before enabling live ads.

## Mobile release checks

- On Android Chrome and iPhone Safari, close the tab after a win, reopen it, and confirm the clear survives. Repeat offline and verify that reconnecting updates the boards exactly once.
- Link a guest profile, sign in on a second device, and confirm the nickname, per-level bests, and ranks. Test a database restore before public launch.
- During a world transition, dismiss the ad, simulate no fill, and background the phone. Each path should open the next world once and restore audio according to the player’s settings. Test switching ads off independently.
