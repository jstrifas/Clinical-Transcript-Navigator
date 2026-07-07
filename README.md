# Clinical Transcript Navigator

AI-powered clinical transcript extraction and SOP routing 
assistant for healthcare Care Teams.

**Live demo:** https://clinical-transcript-navigator.vercel.app/
**Built with:** Claude Opus · Google Gemini · Python

## What It Does
Extracts structured clinical facts from unstructured patient 
call transcripts, evaluates them against clinical SOPs, and 
delivers prioritized routing recommendations with confidence 
scoring — in under 30 seconds.

## Architecture
- **Stage 1:** Claude Opus extracts 13 clinical flags 
  with source quotes and field-level confidence
- **Stage 2:** Deterministic Python SOP rule engine 
  evaluates all 8 rules — same output every run
- **Stage 3:** Google Gemini independently scores recommendation quality across 4 dimensions 
   SOP accuracy 40%, extraction completeness 30%, 
   next step actionability 20%, human review 10%
- **Stage 4:** Specialist reviews, agrees or disagrees, 
  pushes structured output to EHR


## Edge Cases Handled
- Incomplete transcripts → Pending disposition, no false routing
- Inaudible gaps → null flags, never inferred
- Patient contradictions → surfaced explicitly, not resolved
- Wrong file type → blocked before API call
- API timeout → safe fallback, human review triggered
