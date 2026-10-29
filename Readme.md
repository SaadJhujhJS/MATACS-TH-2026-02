# AI Proposal Triage Design

- **Name:** Saad Ali
- **Reference:** MATACS-TH-2026-02
- **Time actually spent:** 2.5 hours

## How I used AI tools

I used Claude to help me understand the assessment, talk through the design, check my scoring arithmetic and draft the documents. I reviewed everything, checked the calculations on the sample proposals myself, and edited the text so it reflects my own reasoning.

## Files

- [diagram/process_flow.png](diagram/process_flow.png): process flow from upload to ranked output
- [WRITEUP.md](WRITEUP.md): answers to the five questions
- [PROMPTS.md](PROMPTS.md): extraction and explanation prompts
- [ranking.json](ranking.json): expected ranking of proposals A to E
- [prototype/triage.py](prototype/triage.py): optional script that runs extraction, gate and scoring on the five samples. By default it runs in offline mode, using saved example extractions (`mock_extractions.json`) in place of the AI so the gate and scoring can be run without an API key. `python triage.py --live` calls the AI instead and needs `ANTHROPIC_API_KEY`.