# Requirements Specification — EventLayout-RL

**Version:** 1.0

> **In one sentence:** an event organizer uploads a 2D floor-plan image of a hall; the system reads it, places the stage, seats and tables, checks that the layout is safe, and explains why it is usable.
>
> Words such as *grid map*, *valid layout* and *RL agent* are explained in [Key Terms](#key-terms) at the end.

## Section 0 — Project Framing & Application Scope

|  |  |
|---|---|
| **Problem Evidence** | 1. The best seating arrangement for a fast evacuation changes with where the exits are, so every room needs its own check (Delcea et al., 2019, *IEEE Access*, <https://doi.org/10.1109/ACCESS.2019.2909637>).<br>2. Seating people compactly under spacing rules was hard enough that a venue needed an optimisation model to plan it (Blom et al., 2022, *INFORMS Journal on Applied Analytics*, <https://doi.org/10.1287/inte.2021.1104>). |
| **Existing Approaches** | 1. **Event diagramming software** (Cvent / Social Tables): lets organizers draw a layout on a floor plan, but every item is placed by hand.<br>2. **Seat-planning optimisation** (Bortolete et al., 2022): places seats automatically under spacing rules, but not the stage, tables or exit paths.<br>3. **RL for furniture layout** (Di & Yu, 2021): an agent learns to arrange furniture in a room, but for homes, with no safety rules and no reasons.<br>4. **Generative AI image tools** (Huang et al., 2023, T2I-CompBench): can draw a layout picture, but often get positions and counts wrong and do not check safety. |
| **Project / Engineering Gap** | No existing tool takes a floor-plan image and produces a complete event layout (stage, seats, tables) that is **checked** for safety and compactness and **explains** the result. This is a practical gap, not a claim of scientific novelty. |
| **Objective O1** | To develop a floor-plan reader that converts a 2D floor-plan image into a grid map of walls, pillars, exits and power points, which the user can check and correct. |
| **Objective O2** | To develop an RL-based layout planner that places the stage, seating blocks, registration table and AV table until the layout passes the safety checks and is compact, has a clear stage view and is practical. |
| **Objective O3** | To evaluate the layouts (validity, constraint satisfaction, generation time) against a simple rule-based method, and show the reason behind each result. |
| **Intended Users** | Event organizers, e.g., university faculty staff and student society committees, planning seminars or conferences of 50–200 people. |
| **Supported Tasks** | 1. Upload a floor-plan image and enter the number of attendees.<br>2. Check and correct the detected walls, pillars, exits and power points.<br>3. Generate a layout with the stage, seating blocks, registration table and AV table.<br>4. Read the pass/fail result and the reason for each of the four layout checks. |
| **Information / Knowledge Domain** | Single-room indoor event halls drawn as colour-coded 2D floor plans (black = walls, red = pillars, green = exits, blue = power points); exit and aisle rules from public fire-safety guidance; common event layout practice. |
| **Operating Context** | Used on a laptop during event planning, days or weeks before the event. The layout is an **advisory draft**: the organizer and the venue still check it before the event. |
| **Important Constraints** | No real venue floor plans with permission to use, so self-made (synthetic) rooms are used for training and testing. One semester and free computing (laptop / Google Colab). The image must follow the colour code. The room is treated as a grid of 1 m squares. No paid AI services. |
| **Explicit Out-of-Scope Tasks** | Certifying that a layout meets fire-safety law (only a qualified officer can do this). Multi-floor, outdoor or irregular venues. Photos or hand sketches of rooms. Events outside 50–200 people. Booking, ticketing, catering, decoration and 3D views. |
| **Evaluation Focus** | Can the system turn a floor-plan image into a complete layout that passes every safety check, in rooms it has never seen, quickly and with correct reasons, and does it do better than a simple rule-based method? |

## Section 1 — Functional Requirements

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
|---|---|---|
| **FR1** | Convert an uploaded 2D floor-plan image into a grid map that shows walls, pillars, exits and power points. | Silently skip parts of the image it cannot read; it must warn the user instead. |
| **FR2** | Let the user view and correct the grid map, and enter the number of attendees (50–200), before a layout is generated. | Generate a layout from a map the user has not confirmed. |
| **FR3** | Generate a complete layout that places the stage, enough seating for the attendees, a registration table and an AV table in the free floor space. | Place any item on a wall, pillar or exit, or outside the room. |
| **FR4** | Check every layout against the safety rules (a clear path from every seat to an exit, minimum aisle width, no blocked exit) and present only layouts that pass. If no layout passes, tell the user. | Present a layout that fails any safety rule as usable. |
| **FR5** | Show the final layout with a pass/fail result and a short reason for each check: safety, compactness, stage view and practicality. | Claim that the layout is officially approved for fire safety. |

## Section 2 — Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| NFR1 | Performance | Layout generation shall be fast enough for interactive planning. | Time each run, from confirmed map to final layout, on the test rooms using a normal laptop. | p90 generation time | < 30 s |
| NFR2 | Correctness (map reading) | The floor-plan reader shall label the room correctly. | Compare the detected grid map with a hand-made answer map for at least 20 test images. | % of grid squares labelled correctly | ≥ 95% |
| NFR3 | Reliability | The system shall find a valid layout for rooms it did not see during training. | Run the system on at least 50 unseen test rooms (different sizes, pillar and exit positions) where a valid layout is possible. | Layout validity rate (% of rooms with a valid layout) | ≥ 90% |
| NFR4 | Layout quality | Valid layouts shall meet the compactness, stage-view and practicality goals. | Score each valid layout with the goal checks; run the rule-based method on the same rooms. | Average constraint satisfaction rate (% of goal checks passed) | ≥ 80%, and not lower than the rule-based method |

## Section 3 — Responsible AI Requirements

These come from the three risks identified in Week 1.

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| RAI1 | Safety | The system shall never present a layout that breaks a safety rule as usable. | Re-check every final layout with a separate test script, including at least 20 tricky rooms (narrow exits, many pillars, room too small), and inspect 20 layouts by hand. | Number of unsafe layouts presented as usable | 0 |
| RAI2 | Wrong image reading (human oversight) | The system shall make the user check and confirm the map before any layout is generated. | Run at least 20 test sessions, including images with deliberate reading errors, and try to generate a layout without confirming the map. | Number of layouts generated from an unconfirmed map | 0 |
| RAI3 | Over-trust (transparency) | Every final layout shall show a reason for each of the four checks that matches the real check result, plus a notice that it is an advisory draft. | Compare the shown reasons with the checker's results for every test layout. | Reason presence rate; reason–result match rate | 100%; 100% |

## Key Terms

**The four layout checks**

| Check | Type | Passes when… |
|---|---|---|
| Safety | Must pass (hard rule) | Every seat has a clear path to an exit, aisles are at least the minimum width, and no exit is blocked. |
| Compactness | Goal | Seats are grouped together near the stage, with little wasted floor space. |
| Stage view | Goal | No pillar blocks the view from a seating block to the stage. |
| Practicality | Goal | The AV table is near a power point and the registration table is near the entrance. |

**Other terms**

| Term | Meaning |
|---|---|
| Grid map | The room drawn as a grid of 1 m × 1 m squares. Each square is marked as floor, wall, pillar, exit or power point. |
| RL agent | A program that learns by trial and error (reinforcement learning). It practises on many self-made rooms, gets a higher score for safe and compact layouts, and learns where to place items. |
| Valid layout | A layout where all requested items are placed and every safety rule passes. |
| Constraint satisfaction rate | The share of goal checks (compactness, stage view, practicality) that a layout passes. |
| Rule-based method | A simple fixed-step method (e.g., stage at the front wall, then fill rows of seats), used only for comparison. |
| Unseen test rooms | Rooms the agent never practised on, used to test whether it really learned. |
| p90 time | 90% of runs finish within this time. |
