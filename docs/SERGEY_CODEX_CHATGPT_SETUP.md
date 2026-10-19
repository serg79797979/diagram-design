# Sergey setup: Codex + ChatGPT

Source repository: https://github.com/serg79797979/diagram-design

This fork already contains the canonical `skills/diagram-design/` skill and `.codex-plugin/plugin.json`. Do not copy or rewrite the skill.

## A. Codex (recommended, fork-tracked plugin)

Run in a terminal with a Codex CLI that supports the plugin marketplace:

```powershell
codex plugin marketplace add serg79797979/diagram-design
codex plugin add diagram-design@diagram-design
```

Restart Codex, then ask: "Using diagram-design, make an architecture diagram of a WPF -> .NET API -> SQL Server 2022 system; save it as HTML + inline SVG."

Alternative Agent Skills installation (if the installed Codex does not support the plugin marketplace):

```powershell
npx skills add serg79797979/diagram-design
```

Select Codex as the target agent. Choose **only one** installation method to avoid duplicate skill discovery.

For a directly editable Windows checkout:

```powershell
git clone https://github.com/serg79797979/diagram-design.git "$env:USERPROFILE\source\diagram-design"
New-Item -ItemType Directory -Force "$env:USERPROFILE\.agents\skills" | Out-Null
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\diagram-design" "$env:USERPROFILE\source\diagram-design\skills\diagram-design"
```

Only run the last command if that target does not exist. Do not overlay a managed installation.

## B. ChatGPT personal skill-only plugin ZIP

Use the fork's existing packager. Requires Python and Git. Clone the fork, then:

```powershell
git clone https://github.com/serg79797979/diagram-design.git
cd diagram-design
python scripts/build-openai-plugin-zip.py --check
python scripts/build-openai-plugin-zip.py
```

The resulting ZIP is written to `dist/`. Upload the ZIP through the ChatGPT Plugin Creator / Plugins workflow to create a **private** plugin. After creation, explicitly connect/enable it in ChatGPT. Merely creating a ZIP or GitHub fork does not install a ChatGPT plugin.

The script packages the `.codex-plugin/plugin.json` manifest, the canonical skills files, required license and declared listing assets. It does not deploy an MCP server or access a database.

**Important:** Do not rename the original plugin identity merely to distinguish the private fork unless you also intentionally update associated metadata and references. Review third-party code before enabling script execution. Only run optional Playwright/Chromium exporters in an environment you trust.

## C. Verification and guardrails

- Try an architecture, a sequence diagram, and a SQL Server ER diagram.
- Check text contrast, layout, arrow crossings, and the diagram's semantic correctness.
- Default to static SVG/HTML; export PNG only on request.
- Use this skill for documentation, **not** for live WPF trading charts, market feeds, or runtime rendering.
- Keep `AI_ENGINEERING_RUNBOOK.md` as the architectural decision process; the diagram skill formats **verified** results.
- Keep a project-specific design profile instead of altering the upstream default style guide.
- Treat repository updates as code updates: review diff and tests before upgrading.
