# skillset

Agent skills by [luiscm17](https://github.com/luiscm17), installable with the [skills CLI](https://skills.sh) via `npx skills`.

[![skills.sh](https://skills.sh/b/luiscm17/skillset)](https://skills.sh/luiscm17/skillset)

## Skills

### arch-hexagonal-ddd

Evidence-based, project-agnostic guidance for Hexagonal Architecture, DDD, SOLID, and design patterns. Reviews project evidence and recommends context-appropriate boundaries, domain models, contracts, composition, and refactoring.

### frontend-arch

Capability-oriented, contract-driven frontend architecture skill. Organizes by product capability ownership, enforces public contracts at capability boundaries, and controls composition direction. Covers organization, dependency rules, public contracts, composition, shared code extraction, anti-patterns, migration recipes, and audit checklists.

### rbac-policy-design

Project-agnostic RBAC authorization policy design and review. Models stable domain capabilities, roles, scopes, and resource instances with safe administration and semantic verification. Covers authorization policy, not authentication implementation.

## Install

```bash
# Install globally to all detected agents
npx skills add luiscm17/skillset --skill arch-hexagonal-ddd -g
npx skills add luiscm17/skillset --skill frontend-arch -g
npx skills add luiscm17/skillset --skill rbac-policy-design -g

# Install globally to specific agents
npx skills add luiscm17/skillset --skill arch-hexagonal-ddd -g -a opencode -a claude-code
npx skills add luiscm17/skillset --skill frontend-arch -g -a opencode -a claude-code
npx skills add luiscm17/skillset --skill rbac-policy-design -g -a opencode -a claude-code

# Install everything in this repo
npx skills add luiscm17/skillset
```

## Structure

```
skills/
├── arch-hexagonal-ddd/
│   ├── SKILL.md
│   └── references/
│       ├── ANTI-PATTERNS.md
│       ├── AUDIT-CHECKLIST.md
│       ├── COMPOSITION-ROOT.md
│       ├── DEPENDENCY-RULE.md
│       ├── DOMAIN-RICHNESS.md
│       ├── FILE-ORGANIZATION.md
│       ├── PORT-CONTRACTS.md
│       ├── REFACTORING-RECIPES.md
│       └── SHARED-KERNEL.md
├── frontend-arch/
│   ├── SKILL.md
│   └── references/
│       ├── ANTI-PATTERNS.md
│       ├── AUDIT-CHECKLIST.md
│       ├── COMPOSITION.md
│       ├── DEPENDENCY-RULES.md
│       ├── MIGRATION-RECIPES.md
│       ├── ORGANIZATION.md
│       ├── PUBLIC-CONTRACTS.md
│       └── SHARED-AND-REUSE.md
└── rbac-policy-design/
    ├── SKILL.md
    └── references/
        ├── administration-safety.md
        ├── policy-model.md
        └── verification-checklist.md
```

Each skill defines its operating contract in `SKILL.md` and provides focused guidance in its `references/` directory.
