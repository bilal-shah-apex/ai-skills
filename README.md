# AI Agent Skills & System Configurations Archive

## Purpose
This repository serves as a centralized, framework-agnostic prompt library. It organizes structural configurations, prescriptive operational boundaries, and system instructions for AI agents. 

---

## Repository Directory Blueprint

All modular instructions are highly discretized and organized into logical component folders:

### 1. `skills/`
Contains tactical instructions teaching an agent *how* to perform a single specialized workflow or task.
* `skills/skill-PRD.md` — Directives for generating structural, metric-driven PRDs.
* `skills/skill-API.md` — Guidelines for mapping databases and endpoints.

### 2. `personas/`
Defines high-level system agent roles, operational constraints, behavioral guardrails, and tones.
* `personas/product-manager.md` — The mindset of an analytical product leader.
* `personas/backend-engineer.md` — The mindset of a strict, performance-first developer.

### 3. `hooks/`
Defines mid-execution validation checkpoints and code-quality gatekeepers.
* `hooks/validation-gate/hook-PRD-check.md` — Rules for parsing generated PRDs to ensure no sections are missing.
* `hooks/security-audit/hook-OWASP.md` — Script-level rules to audit code outputs for vulnerabilities.

### 4. `agents/`
Orchestration blueprints that combine specific **Personas**, **Skills**, and **Hooks** into single execution workflows.
* `agents/scoped-pm-agent.md` — Ties the Product Manager Persona to the PRD Skill and PRD Validation Hook.

---

## File Structure Conventions

To maintain strict parsing compatibility across scripts, every discretized `.md` file must adhere to this file name format:
- **Skills:** `skill-[NAME].md`
- **Personas:** `persona-[NAME].md`
- **Hooks:** `hook-[NAME].md`
- **Agents:** `agent-[NAME].md`

---

## Synchronization Blueprint

This repository is linked to application frameworks using Git Submodules.

### To push updates from this repository:
```bash
git add .
git commit -m "feat(skills): update discretization layouts"
git push origin main
```

### To pull down updates inside the parent project:
```bash
git submodule update --remote --merge
```
