# VAIB SYSTEM RULES & CONSTRAINTS (v16.3)

## 1. ⛔ ABSOLUTE PROHIBITIONS (Safety Rails)
Unless explicitly requested in `development_plan.md` OR by User:

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
Typing: Strict typing required (No any in TS, Type hints in Python).

Grace Anchors: ALWAYS keep # START_CONTRACT INSIDE functions.

Error Handling: No swallowing errors. Log the error with trace.

5. 🛡️ Security Gates
Secrets: No hardcoded keys/passwords. Use ENV variables.

Injection: Use parameterized queries only (SQL).

Validation: Explicit input validation on all public methods.

6. 🔄 Loop Discipline (Single Source of Truth)
Self-Correction: Coder and Editor MUST run syntax checks before handoff.

Termination: If the Coder ↔ Tester loop exceeds 3 iterations without progress -> STOP -> Call Stage 7 (Expert) for forensic analysis.

Routing Criteria:

Major Logic Fail: Contract violated / Wrong Output -> Route to Coder.

Minor/Syntax: Typo / Import error / Style -> Route to Editor.

7. 🧪 Test Ownership
Coder: May create minimal smoke tests (1-2 happy path cases) ONLY if explicitly requested in development_plan.md.

Tester: OWNS test coverage. MUST write comprehensive tests (including adversarial cases) if they are missing or weak.