# Clinical Transcript Navigator

AI-powered clinical transcript extraction and SOP routing 
assistant for healthcare Care Teams.

**Live demo:** [Clinical Transcript Navigator Application](https://clinical-transcript-navigator.vercel.app/)
**Built with:** Claude Opus · Google Gemini · Python

## What It Does
After every patient call, a Healthcare Specialist extracts clinical facts from unstructured transcripts, cross-references them against SOPs, and routes the case to the right next step. As volume scales, that manual work creates two compounding problems: specialists lose confidence that no SOP flag was missed, and bottlenecks delay a patient's path to care. The Clinical Transcript Navigator automates the two highest-friction steps in that workflow, fact extraction and SOP-driven routing, returning a structured summary and prioritized next steps in under 30 seconds so the specialist can focus on judgment, not data entry.

<img width="507" height="172" alt="image" src="https://github.com/user-attachments/assets/301359b7-7056-4aee-9c49-cb41be902bf2" />

↑ SSO login — one credential across all Healthcare systems, every action logged for HIPAA audit trail

Patient calls generate unstructured call transcripts — raw conversational text requiring extraction before it can drive routing decisions. Specialists import transcripts from vector databases or paste directly. Vector databases are HIPAA compliant with signed BAA coverage. Claude Opus extracts clinical flags; Google Gemini independently evaluates recommendations and next steps. Specialists agree or disagree with each output — disagreements captured as free-form notes pushed to the EHR. All artifacts sync back to the patient's EHR profile on completion.

<img width="489" height="267" alt="image" src="https://github.com/user-attachments/assets/9ac27912-2ffb-41a4-9d7a-17009ec80c40" />

Every integration is governed by a signed HIPAA BAA. Transcript data is sourced exclusively from HIPAA-compliant systems with encrypted storage and BAA coverage — ensuring PHI is secured before it reaches the Care Navigator. The SSO audit log ties every read, write, and export event to a named user and timestamp — satisfying HIPAA's audit control requirements without custom logging infrastructure.




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
