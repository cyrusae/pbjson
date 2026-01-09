# Domain Example: Academic Research Project

A realistic example of using pbjson for a thesis, dissertation, or research paper project.

## Why pbjson works for research

Academic work faces unique challenges:
- **Methodology decisions**: "Why did we choose this statistical test over others?"
- **Literature review context**: "What was the gap we identified?"
- **Experimental design**: "Why did we run the experiment this way?"
- **Changing hypotheses**: "When and why did we pivot?"
- **Collaboration with advisors/AI assistants**: "What did we decide about this?"

Unlike software projects (output is code) or writing (output is narrative), research is *about* the decisions—the methodology, the reasoning, the choices. pbjson captures exactly that.

## Scenario: Comparative Study on Academic Persistence

You're researching why some students persist in STEM fields while others switch majors. You're using Claude to work through methodology, interpret results, and develop arguments.

## Research Planning Phase

```bash
# Put pbjson.py in your project context

# Start discussing! 

# Research question and scope
./pbjson.py decided "Study student persistence in STEM through institutional support lens"
./pbjson.py decided "Focus on first-generation college students (underrepresented in STEM)"
./pbjson.py context "Gap in literature: most studies focus on individual factors, not institutional"
./pbjson.py context "Advisor wants mixedmethods approach: surveys + interviews"

# Methodology decisions
./pbjson.py decided:methodology "Mixed methods: quantitative survey (n=500) + qualitative interviews (n=30)"
./pbjson.py question:methodology "Survey instrument: adapt existing NSF survey or create new?"
./pbjson.py context:methodology "Timeline is 18 months including recruitment, so pre-tested instrument preferred"
./pbjson.py resolved:methodology "survey instrument" "Adapt NSF SMET survey with additional institutional support questions, pilot test with 50 students"

# Participant selection
./pbjson.py decided "Recruit from 4 institutions: 1 R1, 1 regional university, 1 HBCU, 1 community college"
./pbjson.py context "Geographic diversity: South, Midwest, Northeast"
./pbjson.py file "methodology.md - research design and participant selection"

# Data analysis approach
./pbjson.py question:data "Quantitative: hierarchical linear modeling to account for institutional nesting?"
./pbjson.py context:data "Preliminary power analysis suggests n=500 sufficient with institutional clustering"
```

## Literature Review & Theory

```bash
# Building the theoretical foundation
./pbjson.py built:theory "Literature review outline (search: lit_review_outline.md, 15 major studies)"
./pbjson.py built:theory "Tinto's Integration Theory application (search: tinto_framework.md)"
./pbjson.py built:theory "Institutional support framework synthesis (search: support_framework.md)"

# Key decisions about framing
./pbjson.py question:theory "Should we use Tinto or newer retention models?"
./pbjson.py context:theory "Tinto foundational but criticized for not addressing institutional context"
./pbjson.py resolved:theory "theoretical framework" "Use Tinto as foundation but extend with Braxton's institutional context model"

# Literature gap
./pbjson.py context:theory "Key finding: no studies examine intersection of first-gen status + institutional support"
./pbjson.py file:theory "literature_matrix.md - all studies coded by methodology and findings"
```

## Pilot Study Phase

```bash
# Preliminary work to refine instruments
./pbjson.py built:data "Pilot survey with 50 students (search: pilot_survey_results.csv)"
./pbjson.py question:methodology "Survey response rate is 38%—acceptable or need modifications?"
./pbjson.py context:methodology "Advisor says 30% acceptable for anonymous survey, but item analysis shows some questions unclear"

./pbjson.py built:methodology "Revised survey based on pilot feedback (search: survey_v2.md)"
./pbjson.py context "Simplified questions 7-12 based on low response variance"

# Interview pilot
./pbjson.py built:methodology "Interview protocol pilot with 5 students (search: interview_protocol_pilot.md)"
./pbjson.py question:methodology "Are we getting at the right aspects of institutional support?"
./pbjson.py resolved:methodology "interview focus" "Add specific questions about advising quality, peer community, STEM identity"

# IRB approval
./pbjson.py context "IRB approved protocol with minor modifications"
./pbjson.py context "Full study launch approved for Fall 2026"
```

## Main Study: Recruitment Phase

```bash
# Participant recruitment
./pbjson.py built:data "Recruitment poster and email templates (search: recruitment_materials.md)"
./pbjson.py built:data "Participant information sheet and consent form (search: consent_form.md)"

# Tracking enrollment
./pbjson.py context "Institution 1 (R1): 156 survey responses (target: 125)"
./pbjson.py context "Institution 2 (Regional): 98 survey responses (target: 125, struggling with recruitment)"
./pbjson.py question:data "Institution 2 low enrollment—should we extend timeline or adjust target?"

# Enrollment decision
./pbjson.py decided:data "Extend recruitment 4 weeks for Institution 2, keep overall target at 500"

# Interview recruitment from survey respondents
./pbjson.py context "Interview recruitment: 28 students agreed (target: 30)"
./pbjson.py context "Interview pool: 40% switched from STEM, 60% still persisting"
```

## Main Study: Data Collection

```bash
# Survey progress
./pbjson.py built:data "Survey data collection: 512 responses (search: survey_data_2026.csv)"
./pbjson.py context "Demographic analysis: 45% first-generation, 55% continuing-generation"
./pbjson.py context "Discipline representation: 35% biology, 25% engineering, 20% chemistry, 20% physics"

# Interview process
./pbjson.py built:data "Interviews 1-10 completed and transcribed (search: interview_transcripts_batch1/)"
./pbjson.py question:data "Preliminary interview themes emerging—should we adjust remaining interview questions?"
./pbjson.py context "Early theme: institutional support varies dramatically by major"

# Mid-study decision
./pbjson.py decided:data "Add follow-up probe question about major-specific support in remaining interviews"
./pbjson.py built:data "Interviews 11-30 completed with revised protocol (search: interview_transcripts_batch2-3/)"

# Preliminary coding
./pbjson.py built:data "Initial coding scheme (search: codebook_v1.md, 23 codes)"
./pbjson.py question:data "Should we use NVivo or manual coding? Team split on efficiency vs control"
./pbjson.py resolved:data "coding approach" "Use NVivo but manual verification of coding—ensures consistency with hand coding validity"
```

## Analysis Phase

```bash
# Quantitative analysis
./pbjson.py built:data "Survey descriptive statistics (search: survey_descriptives.md)"
./pbjson.py built:data "Correlation matrix institutional support × persistence (search: correlation_analysis.md)"

# Key finding
./pbjson.py context "Strong correlation between institutional support and persistence (r=0.67, p<.001)"
./pbjson.py context "Effect larger for first-gen students (r=0.72) than continuing-gen (r=0.58)"

# Hierarchical linear modeling
./pbjson.py built:data "HLM results: institutional nesting effect (search: hlm_results.txt)"
./pbjson.py question:data "HLM shows institutional variance accounts for 23% of persistence difference—implications?"

# Qualitative analysis
./pbjson.py built:data "Thematic analysis: 6 major themes emerged (search: thematic_analysis.md)"
./pbjson.py context "Themes: 1) peer community, 2) mentorship, 3) belonging, 4) major relevance, 5) financial security, 6) identity affirmation"

# Integration of results
./pbjson.py question "How do quantitative and qualitative results integrate? Do themes correspond to survey items?"
./pbjson.py resolved "mixed methods integration" "Thematic categories map to institutional support domains—qualitative interviews explain mechanisms behind quantitative correlations"
```

## Results & Interpretation

```bash
# Key results synthesis
./pbjson.py built "Results chapter draft (search: results_chapter_draft.md)"
./pbjson.py context "Main finding: institutional support predicts persistence better than individual factors alone"

# Unexpected findings
./pbjson.py context "Surprising: major fit matters less for first-gen students with high institutional support"
./pbjson.py question "Interpretation: does institutional support compensate for major fit concerns?"
./pbjson.py context "Qualitative data suggests yes—students can tolerate major mismatch if they feel supported"

# Advisor feedback
./pbjson.py built "Revised results chapter incorporating advisor feedback (search: results_chapter_v2.md)"

# Discussion draft
./pbjson.py built "Discussion chapter draft (search: discussion_draft.md)"
./pbjson.py question "How does our institutional context finding extend Tinto's theory?"
./pbjson.py resolved "theoretical contribution" "Our work demonstrates Tinto's integration mechanism operates primarily at institutional level for first-gen students—individual factors less predictive when institutional context strong"
```

## Writing & Revision

```bash
# Full draft compilation
./pbjson.py built "Full dissertation draft (search: dissertation_draft_full.pdf)"

# Advisor committee feedback
./pbjson.py context "Committee feedback: strengthen limitations discussion, explain practical implications more"

# Revisions
./pbjson.py built "Revised dissertation with committee feedback (search: dissertation_v2.pdf)"

# Final defense preparation
./pbjson.py built "Defense presentation slides (search: defense_slides.md)"
./pbjson.py file "dissertation_final.pdf - final version approved for defense"
```

## Final State: 

### `project.json`

```json
{
  "what_we_decided": [
    "2026-01-15 - Study student persistence in STEM through institutional support lens",
    "2026-01-15 - Focus on first-generation college students (underrepresented in STEM)",
    "2026-01-20 - Mixed methods: quantitative survey (n=500) + qualitative interviews (n=30)",
    "2026-01-25 - Recruit from 4 institutions: 1 R1, 1 regional university, 1 HBCU, 1 community college",
    "2026-02-10 - Extend recruitment 4 weeks for Institution 2, keep overall target at 500"
  ],
  "what_we_built": [
    "2026-01-15 - Literature review outline (search: lit_review_outline.md)",
    "2026-02-01 - Research methodology document (search: methodology.md)",
    "2026-02-20 - IRB protocol approval",
    "2026-03-01 - Revised survey based on pilot feedback (search: survey_v2.md)",
    "2026-04-15 - Survey data collection complete: 512 responses (search: survey_data_2026.csv)",
    "2026-05-30 - All 30 interviews completed and transcribed (search: interview_transcripts/)",
    "2026-06-15 - Initial coding scheme (search: codebook_v1.md)",
    "2026-07-01 - HLM analysis complete (search: hlm_results.txt)",
    "2026-07-15 - Thematic analysis complete (search: thematic_analysis.md)",
    "2026-08-01 - Results chapter draft (search: results_chapter_draft.md)",
    "2026-08-15 - Discussion chapter draft (search: discussion_draft.md)",
    "2026-09-01 - Full dissertation draft (search: dissertation_draft_full.pdf)",
    "2026-09-20 - Revised dissertation with committee feedback (search: dissertation_v2.pdf)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-20 - Survey instrument: adapt existing NSF survey or create new? → Decided: Adapt NSF SMET survey with additional institutional support questions",
    "2026-01-25 - Quantitative: hierarchical linear modeling to account for institutional nesting? → Decided: Yes, HLM appropriate for nested data structure",
    "2026-02-01 - Theoretical framework: Tinto or newer models? → Decided: Use Tinto as foundation but extend with Braxton's institutional context model"
  ],
  "important_files": [
    "2026-01-15 - methodology.md - research design and participant selection",
    "2026-02-01 - literature_matrix.md - all studies coded by methodology and findings",
    "2026-09-20 - dissertation_final.pdf - final version approved for defense"
  ],
  "context": [
    "2026-01-15 - Gap in literature: most studies focus on individual factors, not institutional",
    "2026-01-15 - Advisor wants mixed-methods approach: surveys + interviews",
    "2026-01-15 - Timeline is 18 months including recruitment",
    "2026-04-20 - Demographics: 45% first-generation, 55% continuing-generation",
    "2026-05-01 - Preliminary theme emerging: institutional support varies by major",
    "2026-06-15 - Strong correlation between institutional support and persistence (r=0.67, p<.001)",
    "2026-07-01 - Effect larger for first-gen students (r=0.72) than continuing-gen (r=0.58)",
    "2026-07-10 - HLM shows institutional variance accounts for 23% of persistence difference",
    "2026-08-01 - Major finding: institutional support predicts persistence better than individual factors",
    "2026-08-10 - Unexpected: major fit matters less for first-gen students with high institutional support"
  ]
}
```

### Subsystem: `theory-state.json`

```json
{
  "subsystem": "theory",
  "what_we_decided": [
    "2026-01-25 - Use Tinto as foundation but extend with Braxton's institutional context model"
  ],
  "what_we_built": [
    "2026-01-22 - Literature review outline (search: lit_review_outline.md, 15 major studies)",
    "2026-01-23 - Tinto's Integration Theory application (search: tinto_framework.md)",
    "2026-01-24 - Institutional support framework synthesis (search: support_framework.md)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-25 - Should we use Tinto or newer retention models? → Decided: Use Tinto as foundation but extend with Braxton's institutional context model"
  ],
  "important_files": [
    "2026-01-24 - literature_matrix.md - all studies coded by methodology and findings"
  ],
  "context": [
    "2026-01-23 - Tinto foundational but criticized for not addressing institutional context",
    "2026-01-24 - Key finding: no studies examine intersection of first-gen status + institutional support"
  ]
}
```

### Subsystem: `methodology-state.json`

```json
{
  "subsystem": "methodology",
  "what_we_decided": [
    "2026-01-20 - Adapt NSF SMET survey with institutional support questions, pilot test with 50 students",
    "2026-02-05 - Pilot survey response rate acceptable, revise questions 7-12 based on variance analysis",
    "2026-02-10 - Interview protocol: add specific questions about advising, peer community, STEM identity"
  ],
  "what_we_built": [
    "2026-01-20 - Survey instrument design (search: survey_instrument.md)",
    "2026-02-01 - Pilot survey analysis (search: pilot_results.csv)",
    "2026-02-05 - Revised survey (search: survey_v2.md)",
    "2026-02-08 - Interview protocol (search: interview_protocol.md)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-20 - Survey instrument: adapt NSF or create new? → Decided: Adapt NSF SMET with additional questions, pilot first"
  ],
  "important_files": [
    "2026-01-20 - survey_instrument.md - final survey design"
  ],
  "context": [
    "2026-01-20 - Timeline constraint: need pre-tested instrument",
    "2026-02-01 - Pilot survey response rate: 38% (acceptable per advisor)"
  ]
}
```

### Subsystem: `data-state.json`

```json
{
  "subsystem": "data",
  "what_we_decided": [
    "2026-02-10 - Extend recruitment 4 weeks for Institution 2, keep overall target at 500",
    "2026-05-15 - Add follow-up probe question about major-specific support in remaining interviews"
  ],
  "what_we_built": [
    "2026-02-01 - Pilot survey with 50 students (search: pilot_survey_results.csv)",
    "2026-03-01 - Recruitment poster and email templates (search: recruitment_materials.md)",
    "2026-03-01 - Participant information sheet and consent form (search: consent_form.md)",
    "2026-04-15 - Survey data collection: 512 responses (search: survey_data_2026.csv)",
    "2026-05-10 - Interviews 1-10 completed and transcribed (search: interview_transcripts_batch1/)",
    "2026-05-30 - Interviews 11-30 completed with revised protocol (search: interview_transcripts_batch2-3/)",
    "2026-06-15 - Initial coding scheme (search: codebook_v1.md, 23 codes)",
    "2026-07-01 - Survey descriptive statistics (search: survey_descriptives.md)",
    "2026-07-01 - Correlation matrix institutional support × persistence (search: correlation_analysis.md)",
    "2026-07-10 - HLM results: institutional nesting effect (search: hlm_results.txt)",
    "2026-07-20 - Thematic analysis: 6 major themes emerged (search: thematic_analysis.md)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-02-08 - Survey response rate is 38%—acceptable or need modifications? → Decided: Acceptable per advisor, but revised questions 7-12 based on low variance",
    "2026-02-10 - Institution 2 low enrollment—should we extend timeline or adjust target? → Decided: Extend recruitment 4 weeks for Institution 2",
    "2026-05-10 - Preliminary interview themes emerging—should we adjust remaining interview questions? → Decided: Yes, add follow-up probe about major-specific support",
    "2026-06-20 - Should we use NVivo or manual coding? Team split on efficiency vs control → Decided: Use NVivo but manual verification of coding—ensures consistency",
    "2026-07-15 - HLM shows institutional variance accounts for 23% of persistence difference—implications? → Decided: This is the key finding—institutional context matters more than previously thought"
  ],
  "important_files": [],
  "context": [
    "2026-01-25 - Preliminary power analysis suggests n=500 sufficient with institutional clustering",
    "2026-04-01 - Institution 1 (R1): 156 survey responses (target: 125)",
    "2026-04-01 - Institution 2 (Regional): 98 survey responses (target: 125, struggling with recruitment)",
    "2026-04-15 - Interview recruitment: 28 students agreed (target: 30)",
    "2026-04-15 - Interview pool: 40% switched from STEM, 60% still persisting",
    "2026-04-20 - Demographic analysis: 45% first-generation, 55% continuing-generation",
    "2026-04-20 - Discipline representation: 35% biology, 25% engineering, 20% chemistry, 20% physics",
    "2026-05-10 - Early theme: institutional support varies dramatically by major",
    "2026-06-15 - Strong correlation between institutional support and persistence (r=0.67, p<.001)",
    "2026-06-15 - Effect larger for first-gen students (r=0.72) than continuing-gen (r=0.58)",
    "2026-07-10 - HLM shows institutional variance accounts for 23% of persistence difference",
    "2026-07-20 - Themes: 1) peer community, 2) mentorship, 3) belonging, 4) major relevance, 5) financial security, 6) identity affirmation",
    "2026-07-25 - Qualitative data suggests institutional support can compensate for major fit concerns"
  ]
}
```

## Why This Matters for Researchers

- **Methodological transparency**: Future readers of your dissertation can see *why* you made design choices
- **Decision accountability**: "Why did you choose HLM over regression?" is answerable by looking at the research journal
- **Analytical flexibility**: When you pivot from one analysis to another, you record what you were thinking
- **Collaboration with advisors**: Your advisor can see your reasoning and comment on it
- **Generative for writing**: The journal becomes notes for your methodology and discussion chapters
- **Prevention of p-hacking**: Having a record of pre-planned vs post-hoc analyses helps maintain research integrity

The state files are essentially your research lab notebook, but organized, searchable, and integrated with your actual work.