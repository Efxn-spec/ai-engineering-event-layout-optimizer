# Evaluation Criteria — EventLayout-RL

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
|---|---|---|---|---|---|---|
| O1 / FR1 / NFR2 | Floor-Plan Reading Accuracy | Domain-specific capability | Compare the detected grid map with a hand-made answer map for at least 20 test images. | % of grid squares labelled correctly | ≥ 95% | Organizers start from a correct map and only need a few corrections. |
| O2 / FR3 / FR4 / NFR3 | Layout Validity | Generation capability | Run the system on at least 50 unseen test rooms where a valid layout is possible. | Layout validity rate | ≥ 90% | Organizers get a usable first draft for most halls instead of planning from scratch. |
| O2 / RAI1 | Safety Rule Compliance | Domain-specific capability | Re-check every final layout with a separate test script; inspect 20 layouts by hand, including tricky rooms. | Number of unsafe layouts shown as usable | 0 | Attendees are never put at risk by a layout the tool calls safe. |
| O3 / NFR4 | Layout Quality vs Rule-Based Method | Generation capability | Score the valid layouts from both methods on the same test rooms. | Average constraint satisfaction rate | ≥ 80%, and not lower than the rule-based method | Shows whether the RL agent is worth using over a simpler method. |
| O3 / FR5 / RAI3 | Reason Correctness | Instruction-following capability | Compare the shown reasons with the checker's results for every test layout. | Reason presence rate; reason–result match rate | 100%; 100% | Organizers understand why a layout is usable and can spot problems themselves. |
| O3 / NFR1 | Generation Time | Cost and latency | Time each run, from confirmed map to final layout, on a normal laptop. | p90 generation time | < 30 s | Organizers can try several layouts in one planning session. |

*The four layout checks, "valid layout" and the other terms are explained in [Key Terms](week2_requirements.md#key-terms).*
