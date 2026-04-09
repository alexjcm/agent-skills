# 🧩 Agent Skills Catalog

My repository for Agent Skills, templates, and operational guidance.

## 📚 Catalog (Summary)

This is a growing catalog. The list below is a summary, not an exhaustive inventory of every current/future skill.

### 💻 Development

#### [documenting-java-code](skills/development/documenting-java-code)
Generates JavaDoc for Java classes, constructors, and public methods.

#### [writing-junit-tests](skills/development/writing-junit-tests)
Generates Java 8 + JUnit 4 tests.

### 🛠️ Tools

#### [cli-tui-builder](skills/tools/cli-tui-builder)
Guidance for TypeScript terminal apps (CLI/TUI), stack selection, and terminal UX contracts.
- Development runtime: Node.js 20+ or Bun.
- Distribution runtime: Node.js 20+.

#### [safe-bash-scripting](skills/tools/safe-bash-scripting)
Portable and safe Bash scripting standards (macOS Bash 3.2 + Linux).

#### [publishing-npm-packages](skills/tools/publishing-npm-packages)
Manual publish/readiness workflow for public, unscoped TypeScript npm packages.

#### [xsl-to-sql-detail](skills/tools/xsl-to-sql-detail)
Embeds XSL content into SQL insert detail records by identifier.

## 🚀 Installation

```bash
# Add the full catalog
npx skills add alexjcm/agent-skills

# Add one skill only
npx skills add alexjcm/agent-skills --skill <skill-name>
```

## ⚙️ Operational Workflow (Maintain/Update Skills)

### 1) Edit skill content

Update the relevant files:
- `SKILL.md` (required)
- `references/*` (optional, detailed guidance)
- `scripts/*` (optional, deterministic helpers)
- `assets/*` (optional, templates/static resources)
- `trigger_evals.json` (optional but recommended for trigger quality)

### 2) Validate structure and metadata

Run these checks before committing:

```bash
npx skills-ref validate skills/<category>/<skill-name>
npx skills-ref read-properties skills/<category>/<skill-name>
npx skills-ref to-prompt skills/<category>/<skill-name>
```

### 3) Verify behavior quality

For skills with activation logic, keep/update `trigger_evals.json` with:
- positive cases (`should_trigger: true`)
- negative cases (`should_trigger: false`)

### 4) Commit versioned artifacts

Version these files in Git:
- `SKILL.md`
- `trigger_evals.json` (when present)
- `references/*`, `scripts/*`, `assets/*` (when present)

Do not treat skill evals as disposable output; they are part of skill quality and regression safety.

## 📄 License

MIT

## 🔗 References

- Format specification for Agent Skills: https://agentskills.io/specification.md
- Skill authoring best practices: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices.md
- Validate skills tool: https://github.com/agentskills/agentskills/tree/main/skills-ref
- Skill creator: https://github.com/anthropics/skills/blob/main/skills/skill-creator/SKILL.md

- Agent skills tool: https://github.com/vercel-labs/skills
- Collection of agent skills: https://github.com/vercel-labs/agent-skills/tree/main
- More sample skills: https://github.com/tech-leads-club/agent-skills/tree/main

- Agent Skills: https://developers.openai.com/codex/skills
