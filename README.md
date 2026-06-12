# Agent Skills

Single source of truth for all agent skills (Claude Code & friends). Lives at `~/.agents/skills/`;
`~/.claude/skills/` contains only symlinks into this directory. Invoke any skill as `/<folder-name>`.

## Provenance

### Own — CustomGPT.ai design work
| Skill | What it does |
|---|---|
| `customgpt-design` | CustomGPT.ai design system, strict token mode (system root: `~/customgpt-design-system/`) |
| `pxo` | Full 13-step Product Experience OS workflow analysis |
| `premortem` | Structured premortem risk analysis (8 steps) |

### Own — UX pattern library
Distilled from Vitaly Friedman's "Fixing Frustrating Design Patterns" workshop, Baymard, NN/g, and related sources.

| Skill | Area |
|---|---|
| `filtering-ux` | Filter UIs — interaction, taxonomy, layout |
| `sorting-ux` | Sorting — filter-vs-sort, faceted sorting, labels |
| `data-table-ux` | Tables/lists/grids — layout, interaction, a11y |
| `navigation-ux` | Nav, breadcrumbs, menus, IA |
| `web-forms-ux` | Forms, validation, multi-step, a11y |
| `dashboard-dataviz` | Dashboards, chart selection, KPI cards |
| `search-and-content-ux` | Search, autocomplete, scanability |
| `voice-ux` | Voice interfaces, conversation design |
| `ai-design-patterns` | Human-AI interaction, trust, transparency |
| `prompt-engineering` | Prompt design and refinement for LLMs |

The list-view trio (`filtering-ux`, `sorting-ux`, `data-table-ux`) is designed to load together
whenever a list/table/grid is in scope.

### Own — analysis frameworks
| Skill | What it does |
|---|---|
| `bourdieu-distinction` | Bourdieu's *Distinction* framework → 8-section sociological analysis |

### Third-party (vendored)
| Set | Source | Skills |
|---|---|---|
| Layers of Product Design | [jamiemill/layers-skills](https://github.com/jamiemill/layers-skills) | `layers-intro`, `layers-orient`, `layers-domain`, `layers-observed-behaviour`, `layers-user-needs`, `layers-product-strategy`, `layers-conceptual-model`, `layers-interaction-flow`, `layers-surface` |
| Accessibility (Mike Gifford / CivicActions) | [mgifford/accessibility-skills](https://github.com/mgifford/accessibility-skills) | 27 granular a11y skills: `accessibility-general`, `color-contrast`, `keyboard`, `forms`, `tables`, `tooltips`, `navigation`, `aria-live-regions`, `image-alt-text`, `svg`, `charts-graphs`, `touch-pointer`, `light-dark-mode`, `plain-language`, `content-design`, `manual-testing`, `axe-rules`, `bug-reporting`, `ci-cd`, `audio-video`, `maps`, `mermaid`, `print`, `progressive-enhancement`, `user-personalization`, `anchor-links`, `opquast-digital-quality` |
| Remotion | [remotion-dev](https://github.com/remotion-dev) via `npx skills` | `remotion-best-practices` |
| skills.sh | [vercel-labs/skills](https://github.com/vercel-labs/skills) | `find-skills`, `project-kickstart` |
| Designer skills | [Owl-Listener/designer-skills](https://github.com/Owl-Listener/designer-skills) | ~93 skills across design-research, design-systems, interaction-design, design-ops, designer-toolkit (critiques, UX laws, tokens, research methods, specs) |
| PM skills | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) | 68 skills across product-discovery, product-strategy, execution, data-analytics, market-research, go-to-market, marketing-growth, ai-shipping, toolkit |
| Animate | [delphi-ai/animate-skill](https://github.com/delphi-ai/animate-skill) | `animate` — React/Next.js animation patterns (Emil Kowalski's course) |
| Journey Mapping | [cuellarfr/design-skills](https://github.com/cuellarfr/design-skills) | `journey-mapping` — alignment diagrams (CJM, service blueprints, experience/empathy maps, JJG Visual Vocabulary) with references/templates/examples; single skill vendored from the set to avoid name collisions |

Name collisions: `interview-script` and `summarize-interview` exist in both designer-skills and
pm-skills — the pm-skills versions (installed later, Mom Test/JTBD-based) are the ones on disk.

Third-party sets are vendored copies — re-installable via `npx skills add <source>`, but tracked
here so local modifications survive.

## Which analysis skill when

- **`/pxo`** — full product-experience flow analysis of an existing screen/flow
- **`/premortem`** — pre-launch risk list ("it already failed — why?")
- **`/layers-orient`** — diagnose *which design layer* (research → strategy → model → flow → surface) is the bottleneck; then drill in with the matching `/layers-*` skill (load `/layers-intro` first)
