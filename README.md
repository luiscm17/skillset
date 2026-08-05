# skillset

Agent skills by [luiscm17](https://github.com/luiscm17), installable with the [skills CLI](https://skills.sh) via `npx skills`.

[![skills.sh](https://skills.sh/b/luiscm17/skillset)](https://skills.sh/luiscm17/skillset)

## Skills

### arch-hexagonal-ddd

Prescriptive Hexagonal Architecture + DDD operational skill. Enforces the dependency rule, rich domain models, precise port contracts, and clean file organization. Language-agnostic with concrete rules and reference guides.

## Install

```bash
# Install globally to all detected agents
npx skills add luiscm17/skillset --skill arch-hexagonal-ddd -g

# Install globally to specific agents
npx skills add luiscm17/skillset --skill arch-hexagonal-ddd -g -a opencode -a claude-code

# Install everything in this repo
npx skills add luiscm17/skillset
```

## Structure

```
skills/
└── arch-hexagonal-ddd/
    ├── SKILL.md
    └── references/
        ├── ANTI-PATTERNS.md
        ├── AUDIT-CHECKLIST.md
        ├── COMPOSITION-ROOT.md
        ├── DEPENDENCY-RULE.md
        ├── DOMAIN-RICHNESS.md
        ├── FILE-ORGANIZATION.md
        ├── PORT-CONTRACTS.md
        ├── REFACTORING-RECIPES.md
        └── SHARED-KERNEL.md
```

The main `SKILL.md` routes the agent to the right reference before each task: dependency direction, file organization, domain richness, port contracts, shared kernel, composition root, refactoring recipes, audit checklists, and anti-pattern detection.
