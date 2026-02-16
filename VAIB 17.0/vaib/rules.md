# VAIB SYSTEM RULES & CONSTRAINTS (v17.0 Adaptive)

## 0. 🎛️ SYSTEM MODE
# Options: 
#   - PROTOTYPE: Fast iterations, relaxed checks, NO Skeptic.
#   - PRODUCTION: Strict limits, mandatory logs, Skeptic Active.
CURRENT_MODE: PROTOTYPE 

## 1. ⛔ ABSOLUTE PROHIBITIONS (Safety Rails)
Unless explicitly requested in `vaib/02-architect/development_plan.md` OR by User:
| Action | Why Forbidden | Exception Protocol |
|--------|---------------|--------------------|
| **Dependency Upgrades** | Risk of breaking changes | Create separate task with version check |
| **DB Schema Changes** | Data loss / Migration complexity | Require explicit migration plan with rollback |
| **Infra/CI-CD Changes** | Environment impact / breakage | ADR + staged rollout plan |
| **Breaking API Changes** | Client compatibility | Versioned API (v2) or migration guide |
| **Deletion of Logic** | Loss of business value | Mark as `@deprecated` instead |

## 2. 🗣️ Language Policy
- **Chat/Reasoning:** Russian (Русский).
- **Artifacts (Code, variables, file names):** English.
- **Comments/Docstrings:**
  - **NEW Code:** Russian (Intent/Why/Contract).
  - **LEGACY Code:** **STRICTLY PROHIBITED** to translate existing comments. Keep original.
  - **Public Docs:** English.

## 3. 🛑 STOP Discipline
- **Ambiguity:** If requirements are unclear -> STOP -> Ask User.
- **No Assumptions:** Never assume approval. Wait for explicit "GO" / "Approved".

## 4. 💻 Coding Standards & Belief Logs
- **Logging Template (MANDATORY):**
  All agents must use this format for `[Belief]` logs to allow automated verification:
  ```python
  logger.debug("[Module][Function] Belief: <Intent> | Input: <Args> | Expected: <Postcondition>")
  ```
- **Typing:** Strict typing required (No `any` in TS, Type hints in Python).
- **Grace Anchors:** ALWAYS keep `# START_CONTRACT` INSIDE functions.
- **Error Handling:** No swallowing errors. Log the error with trace.

## 5. 🛡️ Security Gates
- **Secrets:** No hardcoded keys/passwords. Use ENV variables.
- **Injection:** Use parameterized queries only (SQL).
- **Validation:** Explicit input validation on all public methods.

## 6. 🔄 Loop Discipline (Single Source of Truth)
- **Self-Correction:** Coder and Editor MUST run syntax checks before handoff.
- **Termination:** If the Coder ↔ Tester loop exceeds 3 iterations without progress -> STOP -> Call **Stage 7 (Expert)** for forensic analysis.
- **Routing Criteria:**
  - **Major Logic Fail:** Contract violated / Wrong Output -> Route to Coder.
  - **Minor/Syntax:** Typo / Import error / Style -> Route to Editor.

## 7. 🧪 Test Ownership
- **Coder:** May create minimal smoke tests (1-2 happy path cases) ONLY if explicitly requested in `development_plan.md`.
- **Tester:** OWNS test coverage. MUST write comprehensive tests (including adversarial cases) if they are missing or weak.

## 8. 📉 Complexity Limits (Adaptive)
This section defines the "Clean Code" threshold.
- **Function Length:** 
  - IF PROTOTYPE: Max 60 lines.
  - IF PRODUCTION: Max 20 lines.
- **Cyclomatic Complexity:** 
  - IF PROTOTYPE: Max 12 (Rank B).
  - IF PRODUCTION: Max 7 (Rank A).
- **Class Size:** 
  - IF PROTOTYPE: Max 400 lines.
  - IF PRODUCTION: Max 200 lines.
- **Tools:** Use `radon`, `flake8`, `wemake-python-styleguide` (or JS equivalents).

## 9. 🧐 The Skeptic's Audit Criteria
- **Trigger:** Runs ONLY IF CURRENT_MODE == PRODUCTION.
- **Rejection Criteria:**
  1. **Duplication:** Logic repeated > 2 times (DRY violation).
  2. **Over-Engineering:** Using heavy libs for trivial tasks.
  3. **Fragility:** Hardcoded values instead of config.
  4. **Contract Rot:** Code does not match `Intent` description.

- **Arbitration Protocol:**
  IF Skeptic rejects the SAME module > 2 times:
  -> STOP & ESCALATE to Vaib7 Expert with tag [Arbitration Needed].
  -> Expert decides: Force Approve (Override Skeptic) OR Rewrite Architecture.
