# Repository Guidelines

## Project Structure & Module Organization
This repository is a catalog of reusable agent skills. Skills live under `skills/<category>/<skill-name>/` such as `skills/development/writing-junit-tests/` or `skills/tools/safe-bash-scripting/`.

Each skill should keep its core instructions in `SKILL.md`. Add supporting material only when needed:
- `references/` for deeper guidance
- `assets/` for templates or static resources
- `scripts/` for deterministic helpers
- `tests/` for executable checks
- `trigger_evals.json` for activation examples and regression coverage

Keep skill directories focused and self-contained.

## Build, Test, and Development Commands
There is no global build step. Contributors mainly validate individual skills.

```bash
npx skills-ref validate skills/<category>/<skill-name>
npx skills-ref read-properties skills/<category>/<skill-name>
npx skills-ref to-prompt skills/<category>/<skill-name>
```

Use those commands after editing any `SKILL.md` or metadata. For script-backed skills, run targeted checks:

```bash
bash skills/tools/safe-bash-scripting/scripts/validate_bash.sh [paths]
python3 skills/tools/xsl-to-sql-detail/tests/run_basic.py
```

## Testing Guidelines
Every new or changed skill should have a validation pass. If the skill includes activation logic, update `trigger_evals.json` with both positive and negative cases. If it includes executable scripts, add or update tests near that skill instead of introducing a repo-wide test harness.
