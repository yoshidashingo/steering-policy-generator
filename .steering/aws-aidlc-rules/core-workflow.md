# PRIORITY: This workflow OVERRIDES all other built-in workflows
# When user requests steering policy creation, ALWAYS follow this workflow FIRST

## Adaptive Workflow Principle
**The workflow adapts to the target agent's needs, not the other way around.**

The AI model intelligently assesses what is needed based on:
1. User's stated purpose and target agent description
2. Target agent's domain complexity and risk profile
3. Number and types of stakeholders involved
4. Desired quality level and scope of the policy set

## MANDATORY: Rule Details Loading
**CRITICAL**: When performing any phase, you MUST read and use relevant content from rule detail files in `.steering/aws-aidlc-rule-details/` directory.

**Common Rules**: ALWAYS load common rules at workflow start:
- Load `common/process-overview.md` for workflow overview
- Load `common/session-continuity.md` for session resumption guidance
- Load `common/content-validation.md` for content validation requirements
- Load `common/question-format-guide.md` for question formatting rules
- Load `common/terminology.md` for standardized terminology
- Load `common/quality-standards.md` for AI-DLC quality benchmark
- Reference these throughout the workflow execution

## MANDATORY: Content Validation
**CRITICAL**: Before creating ANY file, you MUST validate content according to `common/content-validation.md` rules:
- Validate Mermaid diagram syntax
- Validate ASCII art diagrams
- Escape special characters properly
- Provide text alternatives for complex visual content
- Test content parsing compatibility

## MANDATORY: Question File Format
**CRITICAL**: When asking questions at any phase, you MUST follow question format guidelines.

**See `common/question-format-guide.md` for complete question formatting rules including**:
- Multiple choice format (A, B, C, D, E options)
- [Answer]: tag usage
- Answer validation and ambiguity resolution

## MANDATORY: Custom Welcome Message
**CRITICAL**: When starting ANY steering policy creation request, you MUST display the welcome message.

**How to Display Welcome Message**:
1. Load the welcome message from `.steering/aws-aidlc-rule-details/common/welcome-message.md`
2. Display the complete message to the user
3. This should only be done ONCE at the start of a new workflow
4. Do NOT load this file in subsequent interactions to save context space

# Steering Policy Maker — Adaptive Workflow

---

# DISCOVERY PHASE

**Purpose**: Understanding WHAT steering policy to create and WHY

**Focus**: Analyze the user's purpose, research the domain, identify stakeholders, and define scope

**Stages in DISCOVERY PHASE**:
- Purpose Analysis (ALWAYS)
- Domain Research (ALWAYS — Adaptive depth)
- Stakeholder Identification (CONDITIONAL)
- Scope Definition (ALWAYS)

---

## Purpose Analysis (ALWAYS EXECUTE)

1. **MANDATORY**: Log initial user request in audit.md with complete raw input
2. Load all steps from `discovery/purpose-analysis.md`
3. Execute purpose analysis:
   - Analyze user's stated purpose and intent
   - Classify target agent type (Process / Task / Analytical / Hybrid)
   - Identify core capabilities the agent needs
   - Determine complexity level (Simple / Standard / Complex)
   - Map purpose to known agent patterns
4. **MANDATORY**: Create purpose analysis question file if clarification needed
5. **Wait for Explicit Approval**: Present purpose analysis summary — DO NOT PROCEED until user confirms
6. **MANDATORY**: Log findings and user response in audit.md
7. Present completion message to user (see purpose-analysis.md for message formats)

## Domain Research (ALWAYS EXECUTE — Adaptive Depth)

**Always executes** but depth varies based on domain familiarity and complexity:
- **Minimal**: Well-known domain with clear best practices (e.g., code review)
- **Standard**: Domain with some nuance requiring research (e.g., security audit)
- **Comprehensive**: Novel or high-risk domain requiring deep research (e.g., medical device compliance)

**Execution**:
1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `discovery/domain-research.md`
3. Execute domain research:
   - Research domain-specific best practices
   - Identify domain quality standards and compliance requirements
   - Catalog common pitfalls and failure modes
   - Document domain-specific terminology
   - Identify reference implementations or standards
4. Execute at appropriate depth (minimal/standard/comprehensive)
5. **Wait for Explicit Approval**: Present domain research summary — DO NOT PROCEED until user confirms
6. **MANDATORY**: Log user's response in audit.md with complete raw input

## Stakeholder Identification (CONDITIONAL)

**Execute IF**:
- Multiple user types or personas will interact with the target agent
- Different stakeholders have different needs or perspectives
- The target agent serves cross-functional teams
- Access control or role-based behavior is needed

**Skip IF**:
- Single user type with clear, unified needs
- Simple task agent with one interaction pattern
- User explicitly confirms single stakeholder

**Execution**:
1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `discovery/stakeholder-identification.md`
3. Execute stakeholder identification:
   - Identify all user types and personas
   - Map stakeholder needs and priorities
   - Define interaction patterns per stakeholder
   - Identify conflicting requirements between stakeholders
4. **Wait for Explicit Approval**: Present stakeholder map — DO NOT PROCEED until user confirms
5. **MANDATORY**: Log user's response in audit.md with complete raw input

## Scope Definition (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `discovery/scope-definition.md`
3. Load all prior DISCOVERY context:
   - Purpose analysis results
   - Domain research findings
   - Stakeholder identification (if executed)
4. Execute scope definition:
   - Define policy set boundaries (what's in scope, what's out)
   - Estimate number of phases and stages for target agent
   - Estimate number of rule files needed
   - Define quality level target
   - Create preliminary directory structure
5. **Wait for Explicit Approval**: Present scope definition with estimated effort — DO NOT PROCEED until user confirms
6. **MANDATORY**: Log user's response in audit.md with complete raw input

---

# DESIGN PHASE

**Purpose**: Architecting the steering policy structure

**Focus**: Design the workflow, rules, and quality mechanisms for the target agent

**Stages in DESIGN PHASE**:
- Workflow Architecture (ALWAYS)
- Common Rules Design (ALWAYS)
- Phase Rules Design (ALWAYS)
- Quality Mechanisms Design (ALWAYS)

---

## Workflow Architecture (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `design/workflow-architecture.md`
3. **MANDATORY**: Load content validation rules from `common/content-validation.md`
4. Load all prior context:
   - Purpose analysis (agent type classification)
   - Domain research (best practices, standards)
   - Stakeholder identification (if executed)
   - Scope definition (boundaries, estimates)
5. Execute workflow architecture design:
   - Design phase structure based on agent type
   - Define stages within each phase with ALWAYS/CONDITIONAL classification
   - Design stage dependencies and flow
   - Create workflow visualization (VALIDATE Mermaid syntax before writing)
   - Define checkpoint and approval gate placement
   - Design state tracking mechanism
6. **MANDATORY**: Validate all content before file creation per content-validation.md rules
7. **Wait for Explicit Approval**: Present workflow architecture with visualization — DO NOT PROCEED until user confirms
8. **MANDATORY**: Log user's response in audit.md with complete raw input

## Common Rules Design (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `design/common-rules-design.md`
3. Load all prior context including workflow architecture
4. Execute common rules design:
   - Select which standard common rules apply (from template set)
   - Identify domain-specific common rules needed
   - Design welcome message content
   - Design question format adaptations
   - Design content validation rules specific to domain
   - Design error handling procedures
   - Design session continuity approach
5. **Wait for Explicit Approval**: Present common rules design — DO NOT PROCEED until user confirms
6. **MANDATORY**: Log user's response in audit.md with complete raw input

## Phase Rules Design (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `design/phase-rules-design.md`
3. Load all prior context including workflow architecture and common rules design
4. Execute phase rules design:
   - For each phase in the target workflow:
     - Define stage-level rule files needed
     - Design each rule file's structure and content outline
     - Define inter-stage dependencies
     - Design stage completion criteria
     - Design approval gate content
   - Map rule files to directory structure
   - Verify complete coverage of all workflow paths
5. **Wait for Explicit Approval**: Present phase rules design with file list — DO NOT PROCEED until user confirms
6. **MANDATORY**: Log user's response in audit.md with complete raw input

## Quality Mechanisms Design (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `design/quality-mechanisms-design.md`
3. Load all prior context including workflow architecture and rules designs
4. Execute quality mechanisms design:
   - Design checkpoint system (plan-level and stage-level)
   - Design audit trail format and requirements
   - Design content validation rules
   - Design overconfidence prevention measures
   - Design error handling and recovery procedures
   - Design completeness verification approach
   - Map to AI-DLC 11 quality dimensions
5. **Wait for Explicit Approval**: Present quality mechanisms design — DO NOT PROCEED until user confirms
6. **MANDATORY**: Log user's response in audit.md with complete raw input

---

# GENERATION PHASE

**Purpose**: Creating the actual steering policy files

**Focus**: Generate all rule files with full content, following the approved designs

**Stages in GENERATION PHASE**:
- Core Workflow Generation (ALWAYS)
- Common Rules Generation (ALWAYS)
- Phase Rules Generation (ALWAYS)
- Integration Validation (ALWAYS)

---

## Core Workflow Generation (ALWAYS EXECUTE)

**Core Workflow Generation has two parts within one stage**:
1. **Part 1 - Planning**: Create detailed generation plan with explicit steps
2. **Part 2 - Generation**: Execute approved plan to generate the core workflow file

**Execution**:
1. **MANDATORY**: Log any user input during this stage in audit.md
2. Load all steps from `generation/core-workflow-generation.md`
3. Load all DESIGN phase artifacts:
   - Workflow architecture
   - Common rules design
   - Phase rules design
   - Quality mechanisms design
4. **PART 1 - Planning**: Create core workflow generation plan with checkboxes, get user approval
5. **PART 2 - Generation**: Execute approved plan to generate the target's core-workflow.md
6. **MANDATORY**: Present standardized 2-option completion message — DO NOT use emergent behavior
7. **Wait for Explicit Approval**: User must choose between "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
8. **MANDATORY**: Log user's response in audit.md with complete raw input

## Common Rules Generation (ALWAYS EXECUTE)

**Execution**:
1. **MANDATORY**: Log any user input during this stage in audit.md
2. Load all steps from `generation/common-rules-generation.md`
3. Load all DESIGN phase artifacts and generated core workflow
4. Execute common rules generation:
   - Generate each common rule file following the design specifications
   - Apply domain-specific adaptations
   - Cross-reference with core workflow for consistency
   - Validate content before writing each file
5. **MANDATORY**: Present standardized 2-option completion message
6. **Wait for Explicit Approval**: User must choose between "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
7. **MANDATORY**: Log user's response in audit.md with complete raw input

## Phase Rules Generation (ALWAYS EXECUTE)

**Execution**:
1. **MANDATORY**: Log any user input during this stage in audit.md
2. Load all steps from `generation/phase-rules-generation.md`
3. Load all DESIGN phase artifacts, core workflow, and common rules
4. Execute phase rules generation:
   - For each phase in the target workflow:
     - Generate each stage rule file following the design
     - Ensure stage rule files reference appropriate common rules
     - Validate inter-stage references and dependencies
     - Validate content before writing each file
5. **MANDATORY**: Present standardized 2-option completion message
6. **Wait for Explicit Approval**: User must choose between "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
7. **MANDATORY**: Log user's response in audit.md with complete raw input

## Integration Validation (ALWAYS EXECUTE)

**Execution**:
1. **MANDATORY**: Log any user input during this stage in audit.md
2. Load all steps from `generation/integration-validation.md`
3. Load ALL generated files
4. Execute integration validation:
   - Verify all files referenced in core-workflow.md exist
   - Verify all cross-references between files resolve correctly
   - Verify workflow flow completeness (no dead ends, no orphaned stages)
   - Verify pattern consistency across all files
   - Verify quality dimension coverage
   - Generate validation report
5. **If validation fails**: Return to appropriate generation stage to fix issues
6. **Wait for Explicit Approval**: Present validation report — DO NOT PROCEED until user confirms
7. **MANDATORY**: Log user's response in audit.md with complete raw input

---

# REFINEMENT PHASE

**Purpose**: Ensuring AI-DLC-level quality in the generated policy set

**Focus**: Review, validate, and calibrate the complete policy set

**Stages in REFINEMENT PHASE**:
- Completeness Review (ALWAYS)
- Consistency Review (ALWAYS)
- Quality Calibration (ALWAYS)

---

## Completeness Review (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `refinement/completeness-review.md`
3. Load ALL generated policy files
4. Execute completeness review:
   - Check all workflow paths have corresponding rule files
   - Check all ALWAYS/CONDITIONAL classifications are documented
   - Check all approval gates are defined
   - Check all error scenarios have handling procedures
   - Check all quality dimensions are covered
   - Generate completeness checklist with pass/fail for each item
5. **If gaps found**: Return to GENERATION phase to fill gaps
6. **Wait for Explicit Approval**: Present completeness report — DO NOT PROCEED until user confirms
7. **MANDATORY**: Log user's response in audit.md with complete raw input

## Consistency Review (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `refinement/consistency-review.md`
3. Load ALL generated policy files
4. Execute consistency review:
   - Verify terminology usage matches terminology.md across all files
   - Verify structural patterns are consistent (headings, sections, formatting)
   - Verify completion message formats are consistent
   - Verify question file formats follow the guide
   - Verify error handling patterns are consistent
   - Generate consistency report with findings
5. **If inconsistencies found**: Fix them in-place or return to GENERATION
6. **Wait for Explicit Approval**: Present consistency report — DO NOT PROCEED until user confirms
7. **MANDATORY**: Log user's response in audit.md with complete raw input

## Quality Calibration (ALWAYS EXECUTE)

1. **MANDATORY**: Log any user input during this phase in audit.md
2. Load all steps from `refinement/quality-calibration.md`
3. Load ALL generated policy files AND AI-DLC reference files
4. Execute quality calibration against 11 dimensions:
   - **Dimension 1**: Adaptive Workflow — stages classified ALWAYS/CONDITIONAL
   - **Dimension 2**: Mandatory Checkpoints — user approval at critical decisions
   - **Dimension 3**: Question File Format — multi-choice + Other + [Answer]: tag
   - **Dimension 4**: Content Validation — pre-creation validation rules
   - **Dimension 5**: Audit Trail — ISO 8601 timestamps, complete logging
   - **Dimension 6**: Error Handling — phase-specific recovery procedures
   - **Dimension 7**: Overconfidence Prevention — "when in doubt, ask" philosophy
   - **Dimension 8**: Depth Levels — adaptive detail based on complexity
   - **Dimension 9**: Session Continuity — state tracking and resumption
   - **Dimension 10**: Terminology Standardization — glossary enforcement
   - **Dimension 11**: Standardized Completion Messages — REVIEW REQUIRED + WHAT'S NEXT
5. Score each dimension (Pass / Partial / Fail) with evidence
6. **If any dimension fails**: Return to appropriate phase to remediate
7. **Wait for Explicit Approval**: Present calibration scorecard — DO NOT PROCEED until user confirms
8. **MANDATORY**: Log user's response in audit.md with complete raw input

---

## Key Principles

- **Adaptive Execution**: Only execute stages that add value for the specific target agent
- **Transparent Planning**: Always show execution plan before starting
- **User Control**: User can request stage inclusion/exclusion
- **Progress Tracking**: Update steering-state.md with executed and skipped stages
- **Complete Audit Trail**: Log ALL user inputs and AI responses in audit.md with timestamps
  - **CRITICAL**: Capture user's COMPLETE RAW INPUT exactly as provided
  - **CRITICAL**: Never summarize or paraphrase user input in audit log
  - **CRITICAL**: Log every interaction, not just approvals
- **Quality Focus**: Complex agents get full treatment, simple agents stay efficient
- **Content Validation**: Always validate content before file creation per content-validation.md rules
- **NO EMERGENT BEHAVIOR**: All phases MUST use standardized 2-option completion messages as defined in their respective rule files. DO NOT create 3-option menus or other emergent navigation patterns.

## MANDATORY: Plan-Level Checkbox Enforcement

### MANDATORY RULES FOR PLAN EXECUTION
1. **NEVER complete any work without updating plan checkboxes**
2. **IMMEDIATELY after completing ANY step described in a plan file, mark that step [x]**
3. **This must happen in the SAME interaction where the work is completed**
4. **NO EXCEPTIONS**: Every plan step completion MUST be tracked with checkbox updates

### Two-Level Checkbox Tracking System
- **Plan-Level**: Track detailed execution progress within each stage
- **Stage-Level**: Track overall workflow progress in steering-state.md
- **Update immediately**: All progress updates in SAME interaction where work is completed

## Prompts Logging Requirements
- **MANDATORY**: Log EVERY user input (prompts, questions, responses) with timestamp in audit.md
- **MANDATORY**: Capture user's COMPLETE RAW INPUT exactly as provided (never summarize)
- **MANDATORY**: Log every approval prompt with timestamp before asking the user
- **MANDATORY**: Record every user response with timestamp after receiving it
- **CRITICAL**: ALWAYS append changes to EDIT audit.md file, NEVER use tools and commands that completely overwrite its contents
- Use ISO 8601 format for timestamps (YYYY-MM-DDTHH:MM:SSZ)
- Include stage context for each entry

### Audit Log Format:
```markdown
## [Stage Name or Interaction Type]
**Timestamp**: [ISO timestamp]
**User Input**: "[Complete raw user input — never summarized]"
**AI Response**: "[AI's response or action taken]"
**Context**: [Stage, action, or decision made]

---
```

### Correct Tool Usage for audit.md

CORRECT:

1. Read the audit.md file
2. Append/Edit the file to make changes

WRONG:

1. Read the audit.md file
2. Completely overwrite the audit.md with the contents of what you read, plus the new changes you want to add to it

## Domain Adaptation — Agent Type Classification

The Steering Policy Maker classifies target agents into four types, which determine the generated policy structure:

### Process Agent (e.g., AI-DLC, CI/CD Pipeline Manager)
- **Structure**: Multi-phase workflow with conditional stages
- **Pattern**: Plan → Approve → Execute → Verify
- **Characteristics**: Sequential phases, checkpoint-heavy, state tracking, audit trail
- **Typical Phases**: 3-5 phases with 3-7 stages each

### Task Agent (e.g., Code Reviewer, Bug Triager)
- **Structure**: Preparation → Execution → Reporting
- **Pattern**: Analyze → Process → Report
- **Characteristics**: Focused scope, fewer phases, rapid execution
- **Typical Phases**: 2-3 phases with 2-4 stages each

### Analytical Agent (e.g., Document Generator, Data Analyzer)
- **Structure**: Input Analysis → Processing → Output Formatting
- **Pattern**: Ingest → Transform → Produce
- **Characteristics**: Data-driven, template-based outputs, validation-heavy
- **Typical Phases**: 2-4 phases with 2-5 stages each

### Hybrid Agent (e.g., DevOps Assistant, Project Manager)
- **Structure**: Combination of above types
- **Pattern**: Context-dependent switching between patterns
- **Characteristics**: Multiple operation modes, complex state management
- **Typical Phases**: 3-6 phases with variable stages

## Generated Output Directory Structure

```text
.<agent-name>/
├── <agent-name>-rules/
│   └── core-workflow.md                    Master orchestrator
└── <agent-name>-rule-details/
    ├── common/                             Cross-phase rules
    │   ├── welcome-message.md
    │   ├── process-overview.md
    │   ├── question-format-guide.md
    │   ├── content-validation.md
    │   ├── session-continuity.md
    │   ├── error-handling.md
    │   ├── overconfidence-prevention.md
    │   ├── terminology.md
    │   ├── quality-standards.md
    │   └── output-structure-patterns.md
    ├── <phase-1>/                          Phase-specific rules
    │   ├── <stage-1>.md
    │   ├── <stage-2>.md
    │   └── ...
    ├── <phase-2>/
    │   └── ...
    └── <phase-N>/
        └── ...
```

**CRITICAL RULES**:
- Generated policy files: Inside `.<agent-name>/` directory
- Each target agent gets its own top-level dot-directory
- Common rules are always generated (adapted to domain)
- Phase-specific rules match the designed workflow architecture
- All file references in core-workflow.md must resolve to actual files
