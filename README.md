# pyGOMAS_Team

**IDIAVAL, a team of coordinated BDI agents written in AgentSpeak (Jason-style ASL) that plays Capture The Flag in the pyGOMAS multi-agent environment.**

> **Context.** Built in June 2025 as a coursework project (the files are named AIN, the subject's code) at UPV, in the first phase of my public GitHub, when I was finishing my Computer Engineering degree and learning multi-agent systems and agent programming. The work itself dates from April and May 2025 (screenshots and files are dated then); the first commit here uploads it in June. It is kept as it was submitted; the code is not maintained.

![Early game: IDIAVAL agents deploying on the map](./imagenes/Captura%20de%20pantalla%202025-05-01%20221343.png)

## What it does

- Defines the allied team IDIAVAL: 1 Captain, 7 Soldiers, 1 Medic and 1 Field Operator (`pygomas_local.json`).
- The Captain registers as a service, collects the lists of soldiers, medics and field operators, and sends each soldier a position in an arc formation facing the flag.
- Soldiers shoot only when no ally is in the line of fire (anti friendly fire).
- The Medic creates health packs (`.cure`) and the Field Operator creates ammunition (`.reload`) when they see allies or after moving.
- Captain, Medic and Field Operator pick up packs and the flag when needed, guarded by a `goingForPack` belief so they do not chase every pack they see. Soldiers ignore packs on purpose, to keep them fighting.
- Whoever takes the flag returns to the base, which wins the match.

## How it works

Each role is an `.asl` file. The Axis agents in the same files keep the stock patrol-and-shoot behaviour of the original pyGOMAS code and serve as the opponent; only the `team(100)` (Allied) plans are the author's.

1. The Captain waits at the base, then after a pause sends everyone `go_to` orders toward the flag.
2. Two custom actions, defined in Python in `Captain.py` (class `CaptainBDI`, registered through `add_custom_actions`), compute the formation: `.calculatePos(num_agents, arc_degrees)` returns the angular step between soldiers, and `.squadForm(center, flag_pos, distance, angle_offset)` returns the target coordinates of one soldier on the arc. The ASL uses a 100 degree arc and a radius of 30 units (the values used on map 01 according to the report).
3. A recursive belief `issueOrders(Index)` walks the soldier list, sends one position per soldier and restarts the loop every few seconds, so the formation follows the Captain.
4. The strongest soldiers go first and absorb damage; the Medic and Field Operator are sent to the Captain's position so their packs reach the group.

The full design and the author's conclusions are in `memoria_AIN_IDIAVAL.pdf`, inside `equipo_IDIAVAL_pyGomas.zip` (in Spanish).

## Results

From `pygomas_stats.txt` in `AIN_pyGomas_P1.zip` (one match, the file's own figures): winner ALLIED, duration 0:03:10, 5 allied agents alive with average health 79.6, flag taken once and never lost. Allied shots: 525 enemy hits, 62 team hits, 86 misses, 673 total. Axis shots: 278 enemy hits, 42 team hits, 42 misses. Allied medic and field operator packs delivered: 0 in this log. This is a single recorded run, not a benchmark.

## Run it

Not re-run for this README. The steps below follow the repo's own guide.

1. Follow `AIN_iniciar_pygomas_guia.pdf` to install pyGOMAS (Python 3.7 environment), an XMPP server such as PyJabber (Python 3.10 or newer) and to start a match. The manual is `pyGOMAS Manual v2025.pdf`.
2. Unzip `equipo_IDIAVAL_pyGomas.zip`. It contains `Captain.py`, the four `.asl` files, `pygomas_local.json` and the report.
3. Run from that folder, with the XMPP server already running (the manager and service names come from `pygomas_local.json`; adjust `-jm`, `-sj` and `-m` to your setup):
   ```bash
   pygomas manager -jm m@localhost -m map_01 -sj s@localhost -np 20
   pygomas run -g pygomas_local.json
   pygomas render
   ```
   The Captain's rank is `Captain.CaptainBDI`, so `Captain.py` must sit next to the JSON file.
4. The configuration includes the Axis team (8 soldiers, medic, field operator, same `.asl` files) as the opponent. The commands above are the usual pyGOMAS ones described in the guide; I did not confirm them against the PDFs line by line.

## Repository layout

```
equipo_IDIAVAL_pyGomas.zip      final team: Captain.py, 4 x .asl, pygomas_local.json, memoria_AIN_IDIAVAL.pdf
AIN_pyGomas_P1.zip              earlier snapshot of the .asl files, config and pygomas_stats.txt
pygomas.zip                     a copy of the pygomas working directory (platform code, with its own .git and .eggs)
AIN_iniciar_pygomas_guia.pdf    setup guide
AIN_2025_pyGOMAS_P1_sesion_{2,3,4}.pdf   course session slides
pyGOMAS Manual v2025.pdf        platform manual
Jason_cheatsheet.pdf            AgentSpeak / Jason reference
imagenes/                       match screenshots (1 May 2025)
```

## Stack

pyGOMAS, SPADE and XMPP (PyJabber), AgentSpeak (BDI agents), Python 3.7, Pygame renderer.

## Gameplay screenshots

Chronological, from the repo's `imagenes/` folder.

![Mid-game progression](./imagenes/Captura%20de%20pantalla%202025-05-01%20221416.png)
![Tactical engagement](./imagenes/Captura%20de%20pantalla%202025-05-01%20221441.png)
![Advancing toward the objective](./imagenes/Captura%20de%20pantalla%202025-05-01%20221457.png)
![Later stages of the match](./imagenes/Captura%20de%20pantalla%202025-05-01%20221527.png)
![Final moments](./imagenes/Captura%20de%20pantalla%202025-05-01%20221629.png)
