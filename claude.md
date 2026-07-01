# Clinical Transcript Navigator — Project Context

## What This Is
An AI-powered Streamlit web app that extracts structured clinical 
facts from unstructured patient call transcripts, evaluates them 
against clinical SOPs using a deterministic rule engine, and 
delivers prioritized routing recommendations with confidence 
scoring for Carrum Health's Care Team.

## Architecture
Two-stage pipeline:
1. Claude Opus (claude-opus-4-20250514) extracts 13 clinical flags 
   from the transcript — each with value, source_quote, confidence
2. Deterministic Python SOP rule engine evaluates all 8 rules
3. Google Gemini evaluates output across 4 dimensions — 
   SOP accuracy 40%, extraction completeness 30%, 
   next step actionability 20%, human review 10%

## Project Structure
clinical-transcript-navigator/
├── app.py                  ← Streamlit UI — main entry point
├── extractor.py            ← Claude Opus API call + JSON parsing
├── evaluator.py            ← Google Gemini evaluation layer
├── sop_engine.py           ← Deterministic SOP rule engine
├── schema.py               ← Pydantic v2 models
├── validator.py            ← Post-extraction validation
├── prompts/
│   ├── v1_system.txt       ← Extraction system prompt v1
│   ├── v2_system.txt       ← v2 — ambiguity rules added
│   └── v3_system.txt       ← v3 — contradiction handling added
├── sop_rules.json          ← 8 SOP rules as versioned JSON
├── sample_transcripts/     ← Test transcripts by category
├── docs/                   ← Strategic brief and architecture notes
├── .env                    ← API keys (never committed)
├── .env.example            ← Key placeholders (committed)
├── requirements.txt
├── CHANGELOG.md            ← Prompt iteration log
└── README.md

## Key Design Decisions
- SOPs stored in sop_rules.json — not hardcoded — so Clinical Ops 
  can update rules without touching Python code
- SOP engine uses get_flag() helper to safely read nested flag values
- Null over inference — vague answers return null, never false
- Three flag states: true (confirmed), false (denied), null (unknown)
- SOP rules only fire on confirmed false — never on null
- Incomplete transcripts return overall_status: 
  "Pending — Callback Required" with empty recommendations array
- Two AI models — Claude Opus extracts, Gemini evaluates — 
  genuine cross-model adversarial review

## API Keys Needed
- ANTHROPIC_API_KEY — for Claude Opus extraction
- GEMINI_API_KEY — for Google Gemini evaluation

## How to Run
pip install -r requirements.txt
streamlit run app.py

## Coding Conventions
- All boolean SOP conditions use == False, never falsy check
- get_flag(flags, key) used for all flag value access
- JSON parse failures always return safe fallback state with 
  requires_human_review: true
- Prompts loaded from file, never hardcoded in Python
- Every extraction logs prompt_version and model_version

## Current Status
Migrating from previous repo. Core pipeline working. 
Adding new features — see Tier 1, 2, 3 priority list.

## Sample Transcripts Available
Clean joint: Raymond T (all 4 joint rules + GEN-001)
Clean bariatric: Donna M (all 3 bariatric rules + GEN-001)
Edge case incomplete: Daniel M (Pending — Callback Required)
Edge case inaudible: Patricia W ([inaudible] gaps)
Edge case contradictions: Margaret L (smoking conflict)
