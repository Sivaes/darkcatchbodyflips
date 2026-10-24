# Dark Flip

Body flips out of a dark catch in skate., on ReSkate.

skate. only starts a body flip once a grab (LT / RT) has registered for part of the air, and a dark catch refuses the grab press. The heelflip / kickflip / impossible glitch works because the grab goes in just before the dark catch takes over. Dark Flip's goal is to let the grab register during any dark catch, with no grab pose, so every flip trick can body flip dark.

| Version | What it is |
|---|---|
| **0.1.0** | **Probe build.** It changes nothing in the game. It records every air so we can find the exact check that refuses the grab. |

## Install

1. Download the zip from the latest run (Actions) or release.
2. Close the game. Replace `ReSkate.dll` and `ReSkateLauncher.exe` in your ReSkate folder (next to `Skate.exe`) with the two in the zip.
3. Start ReSkate as usual. The probe is on from the start.

## Console (`~`)

- `darkflip status`: the probe, how many airs it has recorded, and whether the state names were found.
- `darkflip mark <text>`: tags the next airs in the logs (for example `darkflip mark normal`).
- `darkflip probe on|off`
- `darkflip names`: searches the game's memory again for the state names (only needed if `status` says none were found).

## Test card (0.1.0)

Play with Dark Catch Assist and Legacy Dark Catches set the way you normally play, and No Bail off. Pick one spot with enough height for a body flip (a ledge or gap you'd normally body flip off), and use it for all three sets.

1. Load in, ollie once, then wait about 10 seconds. The probe searches for the game's state names after the first air, and `ReSkate.log` says when it's done.
2. **Set 1: normal body flips.** Type `darkflip mark normal`. Do 5 body flips: any flip trick caught normally, then LT/RT plus the left stick.
3. **Set 2: the glitch.** Type `darkflip mark glitch`. Do 5 dark catch body flips with the heelflip/impossible timing (grab pressed just before the dark catch). Failed attempts are fine, just keep going until about 5 work.
4. **Set 3: the failure.** Type `darkflip mark fail`. Do 5 attempts with kickflip, tre or 360 inward caught dark, then LT/RT plus the stick (the ones that don't body flip).
5. Close the game and send these files from the `logs` folder next to `Skate.exe`:
   - `ReSkate.log`
   - `dark-flip-steps-<date>-<time>.csv`
   - `dark-flip-states-<date>-<time>.csv`
   - `dark-flip-names-<date>-<time>.txt`

The CSVs can get large. Zip them if Discord won't take them.

## What the probe writes

- **dark-flip-steps**: a row per physics step, from 20 steps before takeoff to 30 after landing. Each row has the pad as the game read it, the physics state, the board's and the skeleton's up axes and spins (`dark` = 1 while standing on the board's underside), the skater context's flag words (+0x1380 to +0x143c), and the core's trick state and its animation output whenever they changed.
- **dark-flip-states**: every animation game state of the skater that changed on a recorded step (`Bool.Intent...`, `Float.Anim...`), by name when the search found it.
- **dark-flip-names**: every state name the search found, with its page and place.
- **ReSkate.log**: one line per air, with its marker, how long it was dark, when LT/RT were first pressed, whether a grab was pressed while dark, how far the body turned and how it ended.

## Build

Actions → **Build Dark Flip** → Run workflow. It builds on ReSkate `v2.1.2` (the tag can be changed), applies the patch embedded in the workflow, and uploads `ReSkate_DarkFlip-<version>-ReSkate<version>` with just the two files. Tick **release** to also publish a GitHub release. The Spotify helper is left out of the build.

`dark-flip.patch` is the same patch as a plain file, for reading or for applying to a ReSkate source tree by hand (`git apply --ignore-whitespace dark-flip.patch`).
