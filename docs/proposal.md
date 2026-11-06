# Proposal: Strength and Fueling Coach Agent

Status: Draft v0.1 (Week 1)
Author: Liz Arcila-Osejo

## 1. Problem and User

I am a busy professional who wants to get strong and lean with training that fits a real schedule and reflects research on women's physiology (for example, the work of Stacy Sims and similar sources). General plans ignore my calendar, my sleep, and what I actually did last week. Chat assistants can answer questions, but they do not remember my training, plan around my schedule, or check themselves against my data.

The only user is me. The project is also a vehicle to practice building production-minded LLM agents: tool use, memory, retrieval, evaluation, and monitoring.

## 2. The One Task

Each week, draft next week's training and fueling plan from my real schedule, my logged sessions, and my chosen source material, and adjust it when I log what I actually did.

## 3. Success Criteria

Targets are starting points. Revise after Week 4 once there is a baseline.

1. **Plan fit:** At least 80% of planned sessions are feasible within my calendar's free slots (measured by me marking plans "doable" or "not doable").
2. **Grounding:** Every training or fueling recommendation cites a source from my curated library. Target: 100% cited, with spot checks showing citations actually support the claim.
3. **Adaptation:** When I log a missed or hard session, the next plan changes in a sensible way. Measured on a scenario test set (see section 7), target of at least 90% of scenarios passing.
4. **Sustainability:** I log at least 4 of 7 days for 8 consecutive weeks. If I stop logging, the design is too heavy.

## 4. Non-Goals

- No medical advice, diagnosis, or treatment. Anything beyond my sources, or any pain, injury, or concerning symptom, results in a "check with a professional" message.
- No second project in the meantime.
- No model training or fine-tuning. The system uses an existing LLM, grounded in curated sources and my own data.
- No multi-user features, accounts, or app polish.
- No automatic purchases, messages to others, or actions outside my own calendar and logs.

## 5. Data and Sources

**My data (kept local, never committed):**
- Session log: exercise, sets, reps, load, perceived effort
- Daily check-in: sleep, energy, soreness, optional notes
- Optional cycle phase tracking, only if I choose to enable it
- Calendar availability (read only)

**Knowledge sources (curated library):**
- Books, articles, and notes on female-oriented training and fueling that I choose myself
- Each document has metadata: author, date, topic, and a short summary

**Repo rule:** The repo contains only synthetic sample data. Real logs live in a gitignored local folder.

## 6. System Design (initial sketch)

- **Orchestrator:** An LLM with a small set of tools, in a simple loop (plan, act, observe)
- **Tools:** `log_session`, `log_checkin`, `get_free_slots` (calendar), `search_sources` (retrieval over the library), `save_plan`
- **Memory:** A structured store (SQLite) for logs and plans, not free-text chat history
- **Retrieval:** Chunked source documents with citations carried through to the output
- **Schedule:** A weekly run (Sunday) that drafts the next plan, plus on-demand updates after logging
- **Guardrails:** Progression caps (limits on weekly load increases), deload rules, and a safety check that routes concerning input to "see a professional"
- **Local-first:** Real data stays on my machine. Model choice (hosted vs. local) is a documented decision; revisit if privacy needs change.

## 7. Evaluation Plan

Built in Week 4, expanded in Week 8.

- **Scenario test set:** 10 to 20 hand-written scenarios at first, growing to 30+. Examples: missed two sessions, poor sleep week, travel week, a fully booked calendar, a report of joint pain (expected: defer to a professional), a request outside scope.
- **Checks per scenario:** Is the plan feasible? Does it respect progression caps? Are citations present and relevant? Did it avoid out-of-scope advice?
- **Methods:** Rule-based checks for hard constraints, and LLM-as-judge with a rubric for softer qualities, with periodic manual review to validate the judge.
- **Tracking:** Results logged per version so changes in prompts, retrieval, or tools can be compared.
- **Real-world signal:** My weekly "doable / not doable" ratings and logging adherence.

## 8. Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Unsafe or overly aggressive training advice | Progression caps, source-only recommendations, professional-referral rule, I remain the final check |
| Hallucinated or misattributed citations | Citation spot checks, require retrieved passage for every claim |
| Overbuilding and never shipping | One-hour weekly build block, non-goals list, milestone gates |
| Privacy leakage of personal health data | Gitignored real data, local storage, synthetic data in repo |
| Logging fatigue leads to abandonment | Keep check-ins under 1 minute, accept free-text input |
| Source library biased or low quality | Curate deliberately, record source metadata, review quarterly |

## 9. Milestones

Follows the 12-week study roadmap.

- **Weeks 1-2:** Proposal done, repo set up, simplest end-to-end version (one LLM call drafts a plan)
- **Weeks 3-4:** Structured inputs and outputs, logging, first scenario test set and baseline
- **Weeks 5-6:** Prompt improvements compared on the test set, retrieval over source library with citations
- **Weeks 7-8:** Calendar and logging tools, expanded evaluation and failure analysis
- **Weeks 9-10:** Reliable scheduled runs, logging and quality monitoring
- **Weeks 11-12:** Cost and latency tuning, guardrail review, write-up as a short case study

## 10. Open Questions

- Hosted model or local model for the final version?
- Integrate WHOOP data
- Which calendar and which logging interface (CLI, chat, or a simple form)?
- Do I include cycle phase tracking, and what are the privacy implications?
- What is my real baseline for "doable" plans?

## 11. Decision Log

Add dated entries as the design changes.

- 2026-11-06: Chose the strength and fueling coach as the single agent project.
