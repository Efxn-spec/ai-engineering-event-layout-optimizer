# EventLayout-RL: AI Event Layout Optimizer

> Reads a hall's 2D floor-plan image and uses reinforcement learning to arrange the stage, seats and tables into a safe, compact layout, explaining why it is usable. For event organizers planning seminars and conferences of 50–200 people.

## Problem Statement

Event organizers must decide by hand where the stage, seats and tables go in a hall. The layout must be safe, compact, give a clear view of the stage and be practical, and these goals change with every room. This project reads the floor-plan image, lets an RL agent arrange the items until the layout passes the checks, and explains the result.

**How it works:** floor-plan image → grid map → user checks the map → RL agent places items → safety and compactness check (try again if it fails) → final layout + reasons

## Evidence of the Problem

- The seating arrangement changes how fast people can evacuate, and the best arrangement depends on where the exits are (Delcea et al., 2019, *IEEE Access*, <https://doi.org/10.1109/ACCESS.2019.2909637>).
- Seating people compactly under spacing rules is hard enough that a venue needed an optimisation model to plan it (Blom et al., 2022, *INFORMS Journal on Applied Analytics*, <https://doi.org/10.1287/inte.2021.1104>).

## Initial Related Work / Existing Approaches

- Event diagramming software (e.g., Cvent / Social Tables): easy to draw with, but the organizer still places every item by hand, with no automatic safety check.
- Seat-planning optimisation tools (Bortolete et al., 2022): only place seats; they ignore exits, the stage and equipment.
- RL for furniture layout (Di & Yu, 2021): designed for home furniture, with no safety rules or explanation.
- Generative AI image tools (Huang et al., 2023, T2I-CompBench): weak at exact positions and counts, and no safety check.

## Engineering Gap / Limitation

No existing approach takes a floor-plan image and automatically produces a complete event layout that is both safe and compact, and explains why it is usable.

## Project Objectives

- O1: To develop a floor-plan reader that converts a 2D floor-plan image into a grid map (walls, pillars, exits, power points).
- O2: To develop an RL agent that places the stage, seats and tables until the layout is safe, compact, has a clear view of the stage and is practical.
- O3: To evaluate layout validity, constraint satisfaction and computation time against a simple rule-based method, and show the reasons behind each layout.

## Target Users

Event organizers (e.g., university faculty and student society event committees) planning seminars or conferences of 50–200 people in indoor halls.

## Data Sources

- Fire Safety Risk Assessment: Small and Medium Places of Assembly (UK Government): <https://www.gov.uk/government/publications/fire-safety-risk-assessment-small-and-medium-places-of-assembly>
- CubiCasa5K floor-plan dataset: <https://github.com/CubiCasa/CubiCasa5k>
- Self-made sample floor-plan images (synthetic), stored in `data/`

## Weekly Progress

| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + [docs](docs/week1_project_scope.md) | ✅ |
| W2 | Requirements + evaluation criteria + V1 architecture | [Requirements](docs/week2_requirements.md) · [Evaluation](docs/week2_evaluation_criteria.md) · [Architecture](docs/week2_architecture_decisions.md) | ✅ |
| W3 | V1 data pipeline (image pathway, practice data) | [Notebook](notebooks/week3_pipeline.ipynb) · [Data inventory](docs/week3_data_inventory.md) · [Pipeline decisions](docs/week3_pipeline_decisions.md) · [Prepared data](data/) | ✅ |
