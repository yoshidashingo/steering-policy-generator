# Steering Policy Maker — Terminology Glossary

## Core Terminology

### Steering Policy
A structured set of markdown-based rules and instructions that guide an AI agent through a specific workflow. A steering policy defines WHAT the agent does, WHEN it does it, and HOW it ensures quality.

### Steering Policy Set
The complete collection of files (core workflow + common rules + phase rules) that together form a comprehensive steering policy for a target agent.

### Target Agent
The AI agent for which steering policies are being created. The target agent will ultimately use the generated policy set to guide its behavior.

### Steering Policy Maker (SPM)
This system — the meta-agent that creates steering policies for other agents. The SPM itself is guided by the files in `.steering/`.

---

## Phase vs Stage

**Phase**: One of the four high-level lifecycle phases in the Steering Policy Maker
- DISCOVERY PHASE — Understanding and Research (WHAT and WHY)
- DESIGN PHASE — Architecture and Planning (HOW to structure)
- GENERATION PHASE — File Creation (BUILD the policies)
- REFINEMENT PHASE — Quality Assurance (VERIFY and CALIBRATE)

**Stage**: An individual workflow activity within a phase
- Examples: Purpose Analysis stage, Domain Research stage, Core Workflow Generation stage
- Each stage has specific prerequisites, steps, and outputs
- Stages can be ALWAYS-EXECUTE or CONDITIONAL

**Usage Examples**:
- "The DISCOVERY phase contains 4 stages"
- "The Purpose Analysis stage is always executed"
- "We're in the DESIGN phase, executing the Workflow Architecture stage"
- WRONG: "The Purpose Analysis phase" (should be "stage")
- WRONG: "The DISCOVERY stage" (should be "phase")

---

## Four-Phase Lifecycle

### DISCOVERY PHASE
**Purpose**: Understanding what steering policy to create
**Focus**: Analyze purpose, research domain, define scope
**Location**: `discovery/` directory

**Stages**:
- Purpose Analysis (ALWAYS)
- Domain Research (ALWAYS — Adaptive depth)
- Stakeholder Identification (CONDITIONAL)
- Scope Definition (ALWAYS)

**Outputs**: Purpose classification, domain best practices, stakeholder map, scope definition

### DESIGN PHASE
**Purpose**: Architecting the policy structure
**Focus**: Design workflow, rules, and quality mechanisms
**Location**: `design/` directory

**Stages**:
- Workflow Architecture (ALWAYS)
- Common Rules Design (ALWAYS)
- Phase Rules Design (ALWAYS)
- Quality Mechanisms Design (ALWAYS)

**Outputs**: Workflow diagram, rules inventory, file list, quality plan

### GENERATION PHASE
**Purpose**: Creating the actual policy files
**Focus**: Generate all files with full content
**Location**: `generation/` directory

**Stages**:
- Core Workflow Generation (ALWAYS)
- Common Rules Generation (ALWAYS)
- Phase Rules Generation (ALWAYS)
- Integration Validation (ALWAYS)

**Outputs**: Complete set of policy files, validation report

### REFINEMENT PHASE
**Purpose**: Ensuring AI-DLC-level quality
**Focus**: Review, validate, and calibrate
**Location**: `refinement/` directory

**Stages**:
- Completeness Review (ALWAYS)
- Consistency Review (ALWAYS)
- Quality Calibration (ALWAYS)

**Outputs**: Completeness report, consistency report, calibration scorecard

---

## Agent Type Classification

### Process Agent
An agent that guides users through a multi-phase workflow with sequential stages, checkpoints, and state tracking. The most complex agent type.

**Examples**: AI-DLC (software development), CI/CD Pipeline Manager, Compliance Auditor
**Characteristics**: Multi-phase, checkpoint-heavy, plan-approve-execute pattern

### Task Agent
An agent focused on executing a specific, bounded task with preparation, execution, and reporting stages.

**Examples**: Code Reviewer, Bug Triager, PR Reviewer, Linter
**Characteristics**: Focused scope, fewer phases, rapid execution cycle

### Analytical Agent
An agent that processes inputs, applies analysis or transformation, and produces structured outputs.

**Examples**: Document Generator, Data Analyzer, Report Builder, Log Parser
**Characteristics**: Data-driven, template-based outputs, validation-heavy

### Hybrid Agent
An agent that combines characteristics of multiple agent types, switching between operational modes based on context.

**Examples**: DevOps Assistant, Project Manager, Full-Stack Developer Assistant
**Characteristics**: Multiple operation modes, complex state management

---

## Execution Classification

### ALWAYS Execute
A stage that MUST execute regardless of context. These stages are foundational to the workflow and cannot be skipped.

**Examples**: Purpose Analysis, Scope Definition, Core Workflow Generation

### CONDITIONAL Execute
A stage that executes only when specific conditions are met. The conditions are explicitly documented with Execute IF and Skip IF criteria.

**Examples**: Stakeholder Identification (only if multiple user types)

### Adaptive Depth
A stage that ALWAYS executes but adjusts its depth based on complexity and context.

**Depth Levels**:
- **Minimal**: Quick, focused execution for simple/well-known scenarios
- **Standard**: Normal depth with standard artifacts for typical projects
- **Comprehensive**: Full depth with all artifacts for complex/high-risk scenarios

---

## Quality Dimensions (11 Dimensions from AI-DLC)

### Dimension 1: Adaptive Workflow
Stages are classified as ALWAYS or CONDITIONAL with explicit conditions.

### Dimension 2: Mandatory Checkpoints
Critical decisions require explicit user approval before proceeding.

### Dimension 3: Question File Format
All questions use multiple-choice format with mandatory "Other" option and [Answer]: tags.

### Dimension 4: Content Validation
All generated content is validated before file creation (syntax, escaping, fallbacks).

### Dimension 5: Audit Trail
Complete logging with ISO 8601 timestamps, raw user inputs (never summarized).

### Dimension 6: Error Handling
Phase-specific recovery procedures for all error severity levels.

### Dimension 7: Overconfidence Prevention
"When in doubt, ask" philosophy — default to asking rather than assuming.

### Dimension 8: Depth Levels
Adaptive detail based on complexity — all artifacts created, detail level adapts.

### Dimension 9: Session Continuity
State tracking via state file, session resumption with context loading.

### Dimension 10: Terminology Standardization
Consistent terminology enforced via glossary across all files.

### Dimension 11: Standardized Completion Messages
Every stage ends with REVIEW REQUIRED section followed by WHAT'S NEXT section with 2 options.

---

## Artifact Types

### Policy Files
The markdown files that constitute the steering policy set for a target agent.
- **Core Workflow**: The master orchestrator file (`core-workflow.md`)
- **Common Rules**: Cross-phase rules (`common/` directory)
- **Phase Rules**: Phase-specific stage rules (`<phase>/` directory)

### Working Artifacts
Files created during the steering policy creation process itself.
- **Question Files**: `{phase-name}-questions.md` — structured questions for user
- **Plans**: Documents with checkboxes tracking execution progress
- **Reports**: Validation, completeness, consistency, and calibration reports

### State Files
Files tracking workflow progress and status.
- `steering-state.md`: Overall workflow state and progress
- `audit.md`: Complete audit trail of all interactions

---

## Common Abbreviations

- **SPM**: Steering Policy Maker
- **AI-DLC**: AI-Driven Development Life Cycle (reference implementation)
- **SDLC**: Software Development Life Cycle

---

## Terminology Guidelines

### When Referring to This System
- "Steering Policy Maker" or "SPM" — the system creating policies
- "steering policy creation workflow" — the process this system follows

### When Referring to Target Agents
- "target agent" — the agent being created
- "target workflow" — the workflow designed for the target agent
- "generated policy set" — the output files for the target agent

### When Referring to Quality Reference
- "AI-DLC" — the reference implementation for quality benchmarking
- "11 quality dimensions" — the quality criteria derived from AI-DLC
- "AI-DLC-level quality" — the quality standard the generated policies should achieve
