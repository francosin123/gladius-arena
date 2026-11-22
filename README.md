# Gladius Arena

Physics-driven gladiator duels in a caged stone arena, right in the browser.

**Play:** https://francosin123.github.io/gladius-arena/

Open the link, pick a name and a discipline, and fight. Everyone who opens the same link lands in the same arena.
Add `#yourroom` to the URL to make a private arena for your friends.

## How it plays
- **Active ragdolls.** Every limb is a physics body. Motors steer the limbs toward a pose, so blows knock you off balance and big hits knock you down.
- **Steer the blade with the mouse.** Weapons have mass and momentum. Wind up, then sweep through. LMB reaches or thrusts, RMB raises the shield. Raise it just before a blow lands to **parry** and stagger the attacker.
- **Damage depends on impact speed and where the hit lands.** Heads take the most. Helmets, the manica and greaves soak up part of a blow.
- **Three disciplines:** Murmillo (gladius and scutum), Hoplomachus (spear and parma), Thraex (sica and parmula).
- **Modes:** practice against a trainer when you are alone, a duel (first to 5) with two players, and a free-for-all (first to 10) with three or more. Fallen gladiators respawn.
- **Phones:** left thumb moves, drag on the right to steer the blade, plus STRIKE, SHIELD and DODGE buttons. Play in landscape.

## Controls (desktop)
Mouse steers the weapon · LMB thrust/reach · RMB shield · WASD move · Shift run · Space dodge · Q/E turn · F lock-on · Esc menu

## Tech
- A single `index.html` file. Three.js 0.160 renders it, cannon-es 0.20 runs the physics, and Trystero 0.26 handles peer-to-peer WebRTC, using Nostr relays for signalling. All of these are pinned on jsDelivr.
- Each client simulates its own gladiator and streams pose snapshots at 30 Hz. Remote fighters are interpolated about 110 ms behind and become kinematic bodies, so your blade really collides with them.
- One peer per room is the **host** (the earliest joiner). The host decides HP, kills, respawns and rounds. Attackers report hits to the host, which applies them and broadcasts the result. If the host leaves, the next peer takes over.
- The protocol is version-tagged (in the app id and in every message), so clients on different versions ignore each other.
- The arena is the free **Arena** pack from [devassets.com](http://devassets.com/) (see their [usage guidelines](http://devassets.com/guidelines/)). It was converted from FBX/Unity into one `arena/arena.glb` (4 MB), with resized textures and the 607 seats baked as an instanced chair plus seat positions. Gladiators, effects and sounds are generated in code.

`test.html` opens two bot-driven clients side by side in iframes for multiplayer testing (serve the folder with `python3 -m http.server`).
