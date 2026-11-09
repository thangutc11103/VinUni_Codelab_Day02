# Plan 004: Finalize 02-deep-dive-report.md

- **Status**: Pending Approval
- **Date**: 2026-11-09 | **Subagents**: none (main agent only)
- **Target Components**: 02-deep-dive-report.md

---

## 1. Objective & Scope
Replace the blank template with the user's draft (Vinhomes ticket triage), fixing gaps found in review: savings math, emergency coverage, privacy, decision thresholds, readability.
- **Layers Affected**: N/A (documentation only)
- **Observability Impact**: None

---

## 2. Implementation Steps
- [ ] **Step 1: Header + 3.1** - info table (nhóm/thành viên); add compact step table (time, 🔴/🔄) beside ASCII flow; mark handoff cư dân→CSKH; fix ASCII alignment
- [ ] **Step 2: 3.2** - Business Impact: total cost vs realistic saving (8→2 min ≈ 75%); clarify 90% model accuracy vs <5% misroute after HITL; boundary: anonymize PII before LLM call
- [ ] **Step 3: 3.3** - emergency defense-in-depth (Rule first; LLM may only escalate up); hotline voice + image handling; align model name with prototype; cost compares against saving
- [ ] **Step 4: Phase 5** - close 5–10% decision gap; unambiguous condition refs; keep NOT YET

---

## 3. Verification Checklist
- [ ] Numbers consistent with 01-problem-scan.md
- [ ] `python autograder/autograder.py --check-file-2` pass

---

## 4. Completion Summary
- **Completed**: YYYY-MM-DD | **Deliverables**: [Key files created/modified]
- **Notes**: [Brief outcome or observations]
