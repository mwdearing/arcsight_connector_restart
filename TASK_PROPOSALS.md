# Task Proposals from Codebase Review

## 1) Typo fix task
**Task:** Fix the typo "Threaddump fles" to "Threaddump files" in the README features list.

**Why this matters:** The typo makes documentation look unpolished and can reduce trust in operational instructions.

**Evidence:** `README.md` currently says "Remove Threaddump fles.".

---

## 2) Bug fix task
**Task:** Fix the broken task import in `tasks/main.yml` by aligning the imported filename with the actual task file (`bakfile_clean.yml`) or renaming the file to match the import.

**Why this matters:** The role currently imports `bakfile_cleanup.yml`, but only `tasks/bakfile_clean.yml` exists. This causes role execution to fail when Ansible tries to load a non-existent task file.

**Evidence:**
- `tasks/main.yml` imports `bakfile_cleanup.yml`.
- Repository contains `tasks/bakfile_clean.yml`.

---

## 3) Comment/documentation discrepancy task
**Task:** Reconcile README behavior documentation with actual role behavior around connector input handling.

**Why this matters:** The README says the role prompts for user input and points to importing `set_connector.yml`, but `tasks/main.yml` has that import commented out and relies on defaults or externally provided vars. This discrepancy can mislead operators.

**Proposed update options:**
- Update README to describe the current behavior (no prompt by default).
- Or uncomment/import `set_connector.yml` and document interactive mode as default.

**Evidence:**
- README describes prompting behavior and references `set_connector.yml` import.
- `tasks/main.yml` currently comments out `import_tasks: set_connector.yml`.

---

## 4) Test improvement task
**Task:** Add Molecule/CI coverage for role task loading and a minimal execution path, including a regression assertion for task file imports.

**Why this matters:** There are no automated tests in this repository. A syntax/role-load test would have caught the broken `bakfile_cleanup.yml` import immediately.

**Suggested scope:**
1. Add a Molecule scenario (or equivalent CI job) that runs:
   - `ansible-playbook --syntax-check` against an example playbook using the role.
   - A converge run in a container with mocked connector paths.
2. Add assertions for idempotence and that cleanup tasks do not fail when target files are absent.
3. Add a regression check ensuring all `import_tasks` files exist.
