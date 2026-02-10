# Quality Calibration — REFINEMENT Phase

## Purpose
Calibrate the generated steering policy set against the AI-DLC reference implementation's 11 quality dimensions. This is the final quality gate — it ensures the generated policy set meets the same quality standard as the proven AI-DLC system (~26 files, ~4,500 lines).

## Prerequisites
- Completeness Review completed and passed
- Consistency Review completed and passed (inconsistencies fixed)
- All generated policy files in final state
- Access to AI-DLC reference files in `.aidlc/` directory
- `common/quality-standards.md` loaded for scoring criteria

## Execution Classification
**Type**: ALWAYS
This stage always executes as it's the definitive quality validation for the generated policy set.

---

## Execution Steps

### Step 1: Load Complete Context
**Action**: Load all generated files AND AI-DLC reference files
**Input**: Generated policy set + AI-DLC reference
**Output**: Complete comparison context

**Generated Policy Set**: Load from `.<agent-name>/`
**AI-DLC Reference**: Load key files from `.aidlc/` for comparison:
- `.aidlc/aws-aidlc-rules/core-workflow.md` — Master workflow reference
- `.aidlc/aws-aidlc-rule-details/common/` — Common rules reference
- `.aidlc/aws-aidlc-rule-details/inception/` — Phase rules reference (sample)
- `.aidlc/aws-aidlc-rule-details/construction/` — Phase rules reference (sample)

### Step 2: Calibrate Dimension 1 — Adaptive Workflow
**Standard**: All stages MUST be explicitly classified as ALWAYS or CONDITIONAL

**Calibration Process**:
1. Count total stages in generated core-workflow.md
2. Count stages with explicit ALWAYS classification
3. Count stages with explicit CONDITIONAL classification (with criteria)
4. Count stages without clear classification
5. Verify CONDITIONAL criteria are specific and testable
6. Compare against AI-DLC: 16 stages, all classified, clear Execute IF/Skip IF

**Scoring**:
- **PASS**: All stages classified, all CONDITIONAL have specific criteria
- **PARTIAL**: 80%+ stages classified, some criteria vague
- **FAIL**: Stages without classification or missing criteria

### Step 3: Calibrate Dimension 2 — Mandatory Checkpoints
**Standard**: Critical decisions MUST require explicit user approval

**Calibration Process**:
1. Count total stages in generated workflow
2. Count stages with explicit approval gates
3. Verify approval gate format (2-option standard)
4. Check for any stages that proceed without approval when they shouldn't
5. Compare against AI-DLC: Every stage has "Wait for Explicit Approval"

**Scoring**:
- **PASS**: All appropriate stages have approval gates with 2-option format
- **PARTIAL**: Most stages have gates, some format variations
- **FAIL**: Stages missing required approval gates

### Step 4: Calibrate Dimension 3 — Question File Format
**Standard**: All questions use structured files with mandatory "Other" option

**Calibration Process**:
1. Verify question-format-guide.md exists and is comprehensive
2. Check that all stage files reference question format guide
3. Verify question format includes: Multiple choice + Other + [Answer]: tag
4. Check for contradiction/ambiguity detection rules
5. Compare against AI-DLC: ~330 lines in question-format-guide.md

**Scoring**:
- **PASS**: Comprehensive guide, all stages reference it, format complete
- **PARTIAL**: Guide exists but thin, or some stages don't reference it
- **FAIL**: No guide, or format doesn't include required elements

### Step 5: Calibrate Dimension 4 — Content Validation
**Standard**: All generated content validated before file creation

**Calibration Process**:
1. Verify content-validation.md exists with validation rules
2. Check for pre-creation validation checklist
3. Verify fallback strategies defined
4. Check that core workflow references content validation as MANDATORY
5. Compare against AI-DLC: content-validation.md + ascii-diagram-standards.md

**Scoring**:
- **PASS**: Validation rules defined, checklist present, fallbacks included
- **PARTIAL**: Validation exists but incomplete coverage
- **FAIL**: No validation rules or no fallbacks

### Step 6: Calibrate Dimension 5 — Audit Trail
**Standard**: Complete logging with ISO 8601 timestamps and raw user inputs

**Calibration Process**:
1. Verify audit trail format defined (in core workflow or common rules)
2. Check for ISO 8601 timestamp requirement
3. Verify "COMPLETE RAW INPUT" requirement (never summarize)
4. Check for append-only rule (never overwrite)
5. Verify stage context included in log entries
6. Compare against AI-DLC: Detailed audit log format with strict rules

**Scoring**:
- **PASS**: Format defined, raw input required, timestamps mandatory, append-only
- **PARTIAL**: Audit exists but missing some requirements
- **FAIL**: No audit trail or critical requirements missing

### Step 7: Calibrate Dimension 6 — Error Handling
**Standard**: Phase-specific recovery procedures for all error severity levels

**Calibration Process**:
1. Verify error-handling.md exists and is comprehensive
2. Check error severity classification (Critical/High/Medium/Low)
3. Verify each phase has specific error scenarios documented
4. Check recovery procedures for each error type
5. Verify escalation guidelines present
6. Check session resumption error handling
7. Compare against AI-DLC: ~370 lines covering all phases

**Scoring**:
- **PASS**: All phases covered, severity levels defined, recovery complete
- **PARTIAL**: Most phases covered, some recovery gaps
- **FAIL**: Minimal error handling or missing phase coverage

### Step 8: Calibrate Dimension 7 — Overconfidence Prevention
**Standard**: "When in doubt, ask" philosophy enforced

**Calibration Process**:
1. Verify overconfidence-prevention.md exists
2. Check for explicit philosophy statement
3. Verify phase-specific guidance for question generation
4. Check for red flags and success indicators
5. Verify answer analysis requirements
6. Compare against AI-DLC: ~100 lines with root cause analysis and phase-specific guidance

**Scoring**:
- **PASS**: Philosophy documented, phase-specific guidance, red flags defined
- **PARTIAL**: General guidance without phase-specific detail
- **FAIL**: No overconfidence prevention

### Step 9: Calibrate Dimension 8 — Depth Levels
**Standard**: Adaptive detail based on complexity

**Calibration Process**:
1. Verify depth levels defined (Minimal/Standard/Comprehensive)
2. Check for depth selection criteria
3. Verify stages with adaptive depth reference the criteria
4. Check for examples of each depth level
5. Compare against AI-DLC: Depth levels in requirements-analysis.md and depth-levels.md

**Scoring**:
- **PASS**: Three levels defined with criteria, referenced in stages
- **PARTIAL**: Levels mentioned but criteria unclear
- **FAIL**: No depth level adaptation

### Step 10: Calibrate Dimension 9 — Session Continuity
**Standard**: State tracking and session resumption with context loading

**Calibration Process**:
1. Verify session-continuity.md exists
2. Check for state file format definition
3. Verify welcome back prompt template
4. Check for context loading priorities per phase
5. Verify artifact loading requirements
6. Compare against AI-DLC: ~47 lines with templates and loading rules

**Scoring**:
- **PASS**: State format defined, loading rules specified, templates provided
- **PARTIAL**: Basic state tracking without loading priorities
- **FAIL**: No session continuity mechanism

### Step 11: Calibrate Dimension 10 — Terminology Standardization
**Standard**: Consistent terminology enforced via glossary

**Calibration Process**:
1. Verify terminology.md exists and is comprehensive
2. Check for domain-specific terms
3. Verify usage examples (correct and incorrect)
4. Check for clear hierarchy (Phase > Stage > Step)
5. Verify abbreviations defined
6. Compare against AI-DLC: ~190 lines with extensive definitions

**Scoring**:
- **PASS**: Comprehensive glossary, domain terms, usage examples
- **PARTIAL**: Basic glossary without examples or domain terms
- **FAIL**: No terminology glossary

### Step 12: Calibrate Dimension 11 — Standardized Completion Messages
**Standard**: Every stage ends with REVIEW REQUIRED + WHAT'S NEXT 2-option format

**Calibration Process**:
1. Count total stage files that should have completion messages
2. Count files with correct REVIEW REQUIRED heading
3. Count files with correct WHAT'S NEXT heading
4. Count files with exactly 2 options
5. Verify no emergent 3-option or variable-option patterns
6. Compare against AI-DLC: Strict 2-option enforcement, explicit anti-3-option rule

**Scoring**:
- **PASS**: All stages use 2-option format consistently
- **PARTIAL**: Most stages compliant, some variations
- **FAIL**: No standard format or emergent patterns

### Step 13: Generate Quality Calibration Scorecard
**Action**: Compile all dimension scores into final scorecard
**Input**: All calibration results
**Output**: Quality calibration scorecard

```markdown
# Quality Calibration Scorecard

## Target Agent: [agent-name]
## Calibration Date: [ISO timestamp]
## Reference: AI-DLC v1.0 (~26 files, ~4,500 lines)

| # | Dimension | Score | Evidence | AI-DLC Comparison |
|---|-----------|-------|----------|--------------------|
| 1 | Adaptive Workflow | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 2 | Mandatory Checkpoints | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 3 | Question File Format | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 4 | Content Validation | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 5 | Audit Trail | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 6 | Error Handling | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 7 | Overconfidence Prevention | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 8 | Depth Levels | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 9 | Session Continuity | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 10 | Terminology | PASS/PARTIAL/FAIL | [evidence] | [comparison] |
| 11 | Completion Messages | PASS/PARTIAL/FAIL | [evidence] | [comparison] |

## Quantitative Comparison
| Metric | Generated | AI-DLC Reference | Status |
|--------|-----------|-----------------|--------|
| Total Files | [N] | 26 | [OK/LOW/HIGH] |
| Total Lines | [N] | ~4,500 | [OK/LOW/HIGH] |
| Core Workflow Lines | [N] | 511 | [OK/LOW/HIGH] |
| Common Rule Files | [N] | 11 | [OK/LOW/HIGH] |
| Phase Rule Files | [N] | 14 | [OK/LOW/HIGH] |

## Overall Score
- **PASS**: [N]/11
- **PARTIAL**: [N]/11
- **FAIL**: [N]/11

## Verdict: [APPROVED / CONDITIONAL APPROVAL / NEEDS REMEDIATION]

### Approval Criteria:
- APPROVED: All 11 dimensions PASS
- CONDITIONAL APPROVAL: 9+ PASS, remainder PARTIAL, none FAIL
- NEEDS REMEDIATION: Any FAIL, or 3+ PARTIAL
```

### Step 14: Handle Calibration Failures
**Action**: If any dimension fails, plan remediation
**Input**: Scorecard with failures
**Output**: Remediation plan or approval

**If NEEDS REMEDIATION**:
1. Identify root cause for each failing dimension
2. Determine which phase/stage needs revision
3. Estimate fix effort
4. Present remediation plan to user
5. Return to appropriate phase to fix

**If CONDITIONAL APPROVAL**:
1. Document the PARTIAL dimensions
2. Explain what would be needed for PASS
3. Ask user if CONDITIONAL APPROVAL is acceptable
4. Log decision in audit.md

### Step 15: Present Quality Calibration Results
**Action**: Present the final scorecard for approval
**Input**: Scorecard + remediation plan (if any)
**Output**: Formatted results for user review

---

## Output Artifacts

### Quality Calibration Scorecard
- **File**: `steering-docs/refinement/quality-calibration-scorecard.md`
- **Content**: Complete 11-dimension scoring with evidence and AI-DLC comparison

---

## Completion Message

### REVIEW REQUIRED

**Quality Calibration is complete.** Here's the final scorecard:

- **Dimensions PASS**: [N]/11
- **Dimensions PARTIAL**: [N]/11
- **Dimensions FAIL**: [N]/11
- **Verdict**: [APPROVED / CONDITIONAL APPROVAL / NEEDS REMEDIATION]

Dimension scores:
1. Adaptive Workflow: [score]
2. Mandatory Checkpoints: [score]
3. Question File Format: [score]
4. Content Validation: [score]
5. Audit Trail: [score]
6. Error Handling: [score]
7. Overconfidence Prevention: [score]
8. Depth Levels: [score]
9. Session Continuity: [score]
10. Terminology: [score]
11. Completion Messages: [score]

[If NEEDS REMEDIATION: Summary of issues and remediation plan]

### WHAT'S NEXT

[If APPROVED or CONDITIONAL APPROVAL:]
**A) Accept and Finalize** — Complete the steering policy creation workflow
**B) Request Improvements** — Further enhance specific dimensions

[If NEEDS REMEDIATION:]
**A) Execute Remediation** — Fix failing dimensions (returns to appropriate phase)
**B) Accept Current Quality** — Accept with known gaps (must confirm)

---

### WORKFLOW COMPLETE (If Approved)

### STEERING POLICY SET COMPLETE

**The steering policy set for [Agent Name] has been generated, validated, and quality-calibrated.**

**Final Deliverables:**
- **Directory**: `.[agent-name]/`
- **Total Files**: [N]
- **Total Lines**: [N]
- **Quality Score**: [N]/11 dimensions PASS
- **Verdict**: [APPROVED / CONDITIONAL APPROVAL]

**Contents:**
- Core Workflow: `[agent-name]-rules/core-workflow.md`
- Common Rules: [N] files in `[agent-name]-rule-details/common/`
- Phase Rules: [N] files across [N] phase directories

### NEXT STEPS

1. **Review** the generated policy files in `.[agent-name]/`
2. **Test** the workflow by running the target agent with a sample task
3. **Iterate** on any rules that need adjustment based on real usage

---

### PHASE COMPLETE: REFINEMENT

**All stages in the REFINEMENT phase are complete.**

**Summary of outputs:**
- Completeness Review: [PASS/FAIL]
- Consistency Review: [N]% consistency score
- Quality Calibration: [N]/11 dimensions PASS — [VERDICT]

---

## Error Handling

### Multiple Dimensions Fail
- **Issue**: Systemic quality issues across multiple dimensions
- **Solution**: Identify root cause (often in DESIGN phase), may need significant revision
- **Recovery**: Return to DESIGN phase if architectural issues, GENERATION if content issues

### AI-DLC Reference Not Available
- **Issue**: Cannot load AI-DLC reference files for comparison
- **Solution**: Use quality-standards.md criteria without direct comparison
- **Workaround**: Score based on absolute criteria rather than relative comparison

### Disagreement on Scoring
- **Issue**: User disagrees with dimension scoring
- **Solution**: Present evidence for each score, ask user to provide counter-evidence
- **Recovery**: Adjust scores based on user input, document reasoning

### Quality Target Mismatch
- **Issue**: Generated policy set's quality doesn't match scope definition's quality target
- **Solution**: Review if quality target was realistic, adjust expectations or enhance quality
- **Recovery**: May need to return to scope definition to adjust targets

## References
- `common/quality-standards.md` — Quality dimension definitions and scoring criteria
- `refinement/completeness-review.md` — Completeness review results
- `refinement/consistency-review.md` — Consistency review results
- `.aidlc/aws-aidlc-rules/core-workflow.md` — AI-DLC reference
- `.aidlc/aws-aidlc-rule-details/common/` — AI-DLC common rules reference
