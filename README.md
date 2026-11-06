# Strength Training Agent
 
A personal agent that drafts and adjusts my weekly strength and fueling plan from my real schedule, my logged sessions, and a curated library of sources on training for women.
 
**Status:** Early stage. Proposal written, implementation not started.
 
## Why this exists
 
General plans ignore my calendar, my sleep, and what I actually did last week. This project is a system, not a smarter chat: it remembers my training, plans around my real schedule, cites its sources, and is tested against scenarios and my own data.
 
It is also a learning project for building production-minded LLM agents: tool use, memory, retrieval, evaluation, and monitoring.
 
## What it will do (v1)
 
Each week, draft next week's training and fueling plan from:
 
- My logged sessions and daily check-ins (sleep, energy, soreness)
- My calendar availability
- A curated set of sources I chose myself
When I log a missed or hard session, the next plan adjusts. Every recommendation cites a source.
 
## What it will not do
 
- Give medical advice. Pain, injury, or concerning symptoms get a "check with a professional" response.
- Train or fine-tune a model.
- Support multiple users.
See [PROPOSAL.md](PROPOSAL.md) for the full scope, success criteria, evaluation plan, and risks.
 
## Roadmap
 
| Weeks | Focus |
|---|---|
| 1-2 | Proposal, repo setup, simplest end-to-end version |
| 3-4 | Structured inputs and outputs, logging, first scenario test set |
| 5-6 | Prompt comparisons, retrieval with citations |
| 7-8 | Calendar and logging tools, expanded evaluation |
| 9-10 | Scheduled runs, monitoring |
| 11-12 | Cost and latency tuning, guardrails, case study write-up |
 
Possible later extension: wearable data (for example WHOOP) as an additional input.
 
## Repo layout (planned)
 
```
.
├── README.md
├── PROPOSAL.md
├── data/          # real logs, gitignored, never committed
├── sample_data/   # synthetic examples for testing and demos
├── sources/       # curated source library (metadata only if licensing requires)
├── src/           # agent code
└── evals/         # scenario test set and evaluation scripts
```
 
## Privacy
 
Real training and health data stay local and are gitignored. This repo contains only synthetic sample data.
 
## Getting started
 
Setup instructions will be added once the first working version exists.
 
## Author
 
Liz Arcila-Osejo
