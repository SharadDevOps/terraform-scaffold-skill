# terraform-scaffold

A [Claude Agent Skill](https://docs.claude.com/en/docs/claude-code/skills) that scaffolds and extends **standardized Azure Terraform repositories** using Terraform Cloud, GitHub Actions, and a prescribed module/environment layout.

Give Claude this skill and a few inputs (org name, brand, project, region, environments) and it produces a complete, convention-compliant infrastructure repo: reusable modules, per-environment roots, CI/CD workflows for plan/apply/destroy, pre-checks (fmt, TFLint, KICS), `.gitignore`, and docs.

## What it generates

```text
<organization>/
  .github/workflows/     # terraform-plan, terraform-apply, terraform-destroy, pre-checks
  .gitignore
  docs/                  # architecture, operations, onboarding
  foundation/            # all Terraform config
    AGENTS.md
    README.md
    deployments/
      modules/resource-group/        # main.tf, variables.tf, outputs.tf
      projects/<project>/<env>/       # backend.tf, locals.tf, main.tf, outputs.tf,
                                      # terraform.tfvars, variables.tf
  scripts/               # repeatable non-secret automation
```

## Conventions enforced

- Terraform `>= 1.5.0`, `hashicorp/azurerm` pinned to `~> 3.100`.
- State in Terraform Cloud; workspace naming `<brand>-<project>-<env>-HCP`.
- Resource-group naming `<brand_short>-<project_short>-<env_short>-<location_short>-rg`.
- Standard tags: `Brand`, `Environment`, `Project`, `ManagedBy`.
- Secrets never committed — they live in Terraform Cloud workspace variables and GitHub secrets.

## Installation

### Claude Code / Cowork (personal skill)

Clone or download this repo and copy `SKILL.md` into a skills directory Claude reads, e.g.:

```bash
git clone https://github.com/SharadDevOps/terraform-scaffold-skill.git
mkdir -p ~/.claude/skills/terraform-scaffold
cp terraform-scaffold-skill/SKILL.md ~/.claude/skills/terraform-scaffold/SKILL.md
```

Restart Claude Code / Cowork. The skill triggers automatically when you ask to scaffold Terraform infrastructure, or invoke it by name.

### Project skill

Place `SKILL.md` at `.claude/skills/terraform-scaffold/SKILL.md` inside a project to share it with everyone working in that repo.

## Usage

Just ask, for example:

> Scaffold a new Azure Terraform repo. Org `acme`, brand Acme/acme, project core/core, region Central India/cin, environments dev and prod.

Claude will confirm any missing inputs, then generate the full layout above.

## License

Apache-2.0 — see [LICENSE](LICENSE).
