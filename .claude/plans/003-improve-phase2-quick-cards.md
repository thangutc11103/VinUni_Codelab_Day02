# Plan 003: Improve Phase 2 Quick Problem Cards

- **Status**: Completed
- **Date**: 2026-11-09 | **Subagents**: none (main agent only)
- **Target Components**: 01-problem-scan.md (Phase 2 + selection section only)

---

## 1. Objective & Scope
Replace the blank Phase 2 / selection sections with the user's draft (3 Vinhomes cards: SCAN #1, #3, #5), improved for consistency, rubric fit and readability. Phase 1 and header untouched.
- **Layers Affected**: N/A (documentation only)
- **Observability Impact**: None

---

## 2. Implementation Steps
- [x] **Step 1: Fix number mismatches vs SCAN** - Card #1 time (5 vs 8 min), Card #3 effort (3-5 ngày công vs 4 ngày), label all figures as estimates (ước tính)
- [x] **Step 2: Convert ASCII boxes → Markdown tables** - matches template, fixes broken box alignment on GitHub
- [x] **Step 3: Strengthen cards** - guardrail metric + safety boundary (urgent cases via Rule, HITL), baseline data source per metric
- [x] **Step 4: Selection section** - add scoring matrix (volume, data readiness, error risk, AI fit, measurability) + keep rationale; fill stress-test critique slot

---

## 3. Verification Checklist
- [x] Numbers consistent between SCAN table and cards
- [x] `python autograder/autograder.py --check-file-1` pass

---

## 4. Completion Summary
- **Completed**: 2026-11-09 | **Deliverables**: 01-problem-scan.md (Phase 2 + selection)
- **Notes**: Cards as tables, numbers aligned with SCAN, guardrails + baseline sources, scoring matrix (23/15/13). Stress-test critique is self-written; replace with real AI output if available.
