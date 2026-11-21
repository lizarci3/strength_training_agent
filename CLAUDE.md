# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project state

Design-stage only: there is no code, build, lint, or test tooling yet. The repo currently holds three documents:

- `README.md`: overview, 12-week roadmap, planned repo layout
- `docs/proposal.md`: the source of truth for scope, success criteria, system design, guardrails, and the dated **Decision Log** (section 11)
- `evals/scenarios.md`: hand-written evaluation scenarios (S1-S9) drafted before implementation

When commands or setup are added (the `.gitignore` anticipates Python with `.venv/` and notebooks), update this file.

## What is being built

A single-user, local-first LLM agent that drafts the author's weekly strength-training and fueling plan from logged sessions, daily check-ins (sleep, energy, soreness), calendar free slots, and a curated source library on training for women. It re-plans when a missed or hard session is logged. Every recommendation must cite a source from the library. It is also a learning project for production-minded agent practices (tool use, memory, retrieval, evals, monitoring), so favor explicit, testable structure over cleverness.

## Planned architecture (from `docs/proposal.md` section 6)

- **Orchestrator:** existing LLM in a simple plan/act/observe loop. No training or fine-tuning.
- **Tools:** `log_session`, `log_checkin`, `get_free_slots` (calendar, read-only), `search_sources` (retrieval), `save_plan`.
- **Memory:** structured SQLite store for logs and plans, not free-text chat history.
- **Retrieval:** chunked source documents; citations must be carried through to the output and match a real retrieved passage.
- **Schedule:** weekly Sunday run drafting next week's plan, plus on-demand updates after logging.
- **Planned layout:** `src/` (agent), `evals/` (scenarios + eval scripts), `sources/` (library, metadata only if licensing requires), `sample_data/` (synthetic), `data/` (real, gitignored).

## Guardrails that code and evals must enforce

- **Progression caps:** limit weekly load increases. When a workout is rated easy, progress larger-muscle compound lifts (squat, deadlift, press, row) first and hold accessories steady. Very hard rating means hold or reduce. Missed sessions are never "made up" by piling volume onto the next week.
- **Four-tier symptom safety check:**
  1. Normal soreness: rest day or lighter session, no referral (do not over-refer).
  2. Mild/short-lived ache: reduce or swap the movement, monitor, refer if it persists.
  3. Sharp, persistent, or rest-time pain: stop loading the area, consult a professional, no diagnosis or treatment advice.
  4. Red flags (dizziness, chest tightness, fainting): stop and seek medical care, firmer language.
- **Scope:** no medical advice; no extreme calorie restriction; if sources do not cover a topic, say so rather than inventing a citation. No actions beyond the user's own calendar and logs.

## Evaluation approach

Scenarios in `evals/scenarios.md` each define situation, good/bad behavior, and a check type: **rule** (automatic hard-constraint checks such as slot fit, progression cap, citation present, equipment constraints, referral present/absent) vs. **judge** (LLM-as-judge with a rubric, periodically validated by manual review). Results are to be tracked per version so prompt/retrieval/tool changes can be compared. S6 (soreness vs. injury) and S7 (out-of-scope diet) are high-priority safety cases; S6a should be run as a template across multiple joints.

## Data privacy rule

Real training/health data lives only in `data/` (gitignored) and must never be committed. Anything committed (fixtures, demos, eval inputs) must be synthetic and go in `sample_data/`.

## Conventions

- Record design changes as dated entries in the Decision Log in `docs/proposal.md`.
- Respect the non-goals: no multi-user features, accounts, app polish, or second project before Week 12.
