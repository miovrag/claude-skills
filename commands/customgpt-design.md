---
name: customgpt-design
description: Use this skill to generate well-branded interfaces, mockups, and assets for CustomGPT.ai. Strict design system execution mode — all output must use defined tokens only. Includes brand tokens (colors, type, spacing, radius, shadows), Public Sans + Inter fonts, Vuetify SCSS configuration (vuetify.scss), production component wrappers, two reference UI kits (chat + dashboard), and ~20 component preview cards. Invoke when designing anything that should feel like CustomGPT — chat surfaces, admin/studio screens, marketing slides, widgets, or component spec sheets.
user-invocable: true
---

# CustomGPT.ai Design System — AI Execution Skill (Strict Mode)

Design system root: `/Users/miodragristovski/Downloads/CustomGPT.ai Design System-2/`

---

## CORE RULE (NON-NEGOTIABLE)

The design system is the single source of truth. Do not introduce new tokens, modify existing tokens, use raw values, or invent new components. Use the closest existing semantic token.

Token hierarchy: Raw → Semantic → Component → Usage. Never use raw tokens. Prefer component tokens over semantic when available.

---

## COLOR TOKENS

### Brand
| Token | Value |
| --- | --- |
| `brand-primary-default` | `#7367F0` |
| `brand-primary-hover` | `#685DD8` |
| `brand-primary-active` | `#5C53C0` |
| `brand-primary-tint` | `#EAE8FD` |

### Background / Surface
| Token | Value |
| --- | --- |
| `bg-canvas` | `#FAFAFA` |
| `bg-surface` | `#FFFFFF` |
| `bg-elevated` | `#FFFFFF` + shadow |
| `bg-selected` | `#F5F5F5` |
| `bg-overlay` | `rgba(23,23,23,0.5)` |

### Typography Colors
| Token | Value |
| --- | --- |
| `text-heading` | `#212121` |
| `text-body` | `#565656` |
| `text-muted` | `#B7B5BE` |
| `text-disabled` | `#A3A3A3` |
| `text-placeholder` | `#999999` |
| `text-link` | `#7367F0` |

### Borders
| Token | Value |
| --- | --- |
| `border-default` | `#E5E5E5` |
| `border-emphasis` | `#D4D4D4` |

### Status
| Token | Value |
| --- | --- |
| `color-success` | `#28C76F` |
| `color-warning` | `#FF9F43` |
| `color-error` / `color-danger` | `#EA5455` |
| `color-info` | `#0076E5` |

### Disabled
| Token | Value |
| --- | --- |
| `bg-disabled` | `#DBDADE` |
| `text-disabled` | `#A3A3A3` |

### Background color utilities (Tailwind `backgroundColor` overrides)
| Class | Value | Use |
| --- | --- | --- |
| `bg-body` | `#F5F5F5` | page/app background |
| `bg-dark` | `#4B4B4B` | dark surface |
| `bg-light` | `#DFDFE3` | light surface |
| `bg-divider` | `#DBDADE` | divider lines |
| `bg-header` | `#ECECEE` | table/section headers |
| `bg-muted` | `#4B465C` | muted dark surface (≠ text-muted) |

### Gradient background classes (Tailwind `backgroundImage`)
| Class | Value |
| --- | --- |
| `bg-gradient-primary` | `linear-gradient(64deg, #6D7CFF 16%, #FF51FF 84%)` |
| `bg-gradient-menu-active` | `linear-gradient(64deg, #7367F0 16%, rgba(115,103,240,.70) 84%)` |
| `bg-gradient-success` | `linear-gradient(45deg, #28C76F 0%, #48DA89 100%)` |
| `bg-gradient-danger` | `linear-gradient(45deg, #EA5455 0%, #F08182 100%)` |
| `bg-gradient-warning` | `linear-gradient(45deg, #FF9F43 0%, #FFB976 100%)` |
| `bg-gradient-info` | `linear-gradient(45deg, #0076E5 0%, #1CE7FF 100%)` |
| `bg-gradient-secondary` | `linear-gradient(45deg, #82868B 0%, #9CA0A4 100%)` |

### Color scales (tints and shades)
Each status color has a full scale — use tints for backgrounds, shades for hover/active states:
- `primary-8` (#F4F3FE), `primary-16` (#E9E7FD), `primary-100` through `primary-900`, `primary-hover` (#685DD8), `primary-active` (#6258CC)
- Same pattern for `secondary`, `success`, `danger`, `warning`, `info`, `gray`, `teal`, `orange`
- `danger-50` (#FBF2F2) — lightest danger tint, used for destructive ghost hover

---

## HDR / DISPLAY P3

Add to `:root` in `colors_and_type.css` on P3-capable displays. Neutral tokens (backgrounds, text, borders) do NOT get P3 overrides.

```css
@media (color-gamut: p3) {
  :root {
    --brand-primary-default: color(display-p3 0.45 0.40 0.94);
    --brand-primary-hover:   color(display-p3 0.41 0.36 0.85);
    --brand-primary-active:  color(display-p3 0.36 0.32 0.75);
    --color-success: color(display-p3 0.15 0.78 0.43);
    --color-warning: color(display-p3 1.00 0.62 0.26);
    --color-error:   color(display-p3 0.92 0.33 0.33);
    --color-info:    color(display-p3 0.00 0.46 0.90);
  }
}
```

---

## SPACING TOKENS

| Token | Value | | Token | Value |
| --- | --- | --- | --- | --- |
| `spacing-xs` | 4px | | `section-sm` | 48px |
| `spacing-sm` | 8px | | `section-md` | 64px |
| `spacing-md` | 12px | | `section-lg` | 96px |
| `spacing-lg` | 16px | | `section-xl` | 128px |
| `spacing-xl` | 24px | | | |
| `spacing-2xl` | 32px | | | |
| `spacing-3xl` | 48px | | | |

4-point grid only. Use `section-*` for page-level spacing, `spacing-*` for component internals. No arbitrary values (18px, 10px, 6px, etc.).

---

## RADIUS TOKENS

**CSS variables** (from `colors_and_type.css`) — use with `var(--radius-*)`:
| Token | Value | Use |
| --- | --- | --- |
| `radius-none` | 0px | — |
| `radius-sm` | 4px | small controls |
| `radius-md` | 8px | buttons, inputs |
| `radius-lg` | 12px | chat bubbles |
| `radius-xl` | 16px | cards, composer, modals |
| `radius-full` | 999px | pills, badges, avatars |

**Tailwind `rounded-*` classes** (from `tailwind.config.js`) — different values, use with `@apply` or class attribute:
| Class | Value | Use |
| --- | --- | --- |
| `rounded-sm` | 4px | small controls |
| `rounded-md` | 6px | buttons, inputs, cards, modals, tooltips |
| `rounded-lg` | 8px | — |
| `rounded-xl` | 10px | — |
| `rounded-xxl` | 12px | — |
| `rounded-xxxl` | 14px | — |
| `rounded-pill` | 9999px | chips, badges, avatars |
| `rounded-circle` | 50% | avatar circles |

> These two systems coexist. Tailwind `rounded-*` classes and CSS `var(--radius-*)` produce **different values** — do not mix them on the same component.

---

## TYPOGRAPHY TOKENS

**Fonts:** Vuetify components → `Public Sans`. Tailwind `font-sans` → `Inter`. Do not force Inter onto Vuetify components.

**Size scale (Tailwind v3):** `text-xs` 12px · `text-sm` 14px · `text-base` 16px · `text-lg` 18px · `text-xl` 20px · `text-2xl` 24px · `text-3xl` 30px · `text-4xl` 36px

**Headings** (`addBase` — all `weight-medium` 500):
h1=38px/52px · h2=32px/44px · h3=26px/36px · h4=22px/30px · h5=18px/24px · h6=15px/21px

**`p` element** (`addBase`): 15px / line-height 22px (not `text-base` 16px — addBase overrides it)

**`caption` element** (`addBase`): 11px / 14px

**`th` element** (`addBase`): 13px · uppercase · 500 weight · 1px letter-spacing · `text-heading` color — this is the one framework-level uppercase exception for table column headers.

**Paragraph utility classes** (use these instead of raw text-* for body copy):
| Class | Size | Leading | Weight |
| --- | --- | --- | --- |
| `.paragraph-lead` | 18px | 28px | 400 |
| `.paragraph-normal` | 15px | 24px | — |
| `.paragraph-small` | 13px | 20px | — |
| `.paragraph-extra-small` | 11px | 16px | — |

**Display utility classes:**
| Class | Size | Leading | Weight |
| --- | --- | --- | --- |
| `.display-3` | 48px | 68px | 500 |
| `.display-4` | 32px | 46px | 500 |

**Line height:** `leading-tight` 1.2 · `leading-normal` 1.4 · `leading-relaxed` 1.6 · `leading-loose` 1.8

**Font weight:** `weight-regular` 400 · `weight-medium` 500 · `weight-semibold` 600 · `weight-bold` 700

Rules: Body copy → `.paragraph-normal` or `.paragraph-lead`. Labels → `text-sm` + `weight-medium`. Max 3 font sizes per screen. Never hardcode px values.

---

## BORDER & SHADOW TOKENS

**Borders:** `border-none` 0px · `border-thin` 1px · `border-thick` 2px. Default → `border-thin` with `border-default`. Note: Tailwind's `borderColor DEFAULT` is `#DBDADE` (used by `divide-*` and bare `border` class).

**Shadows (Tailwind `shadow-*` classes from config):**
| Class | Value |
| --- | --- |
| `shadow-card` | `0px 4px 18px 0px rgba(75, 70, 92, 0.10)` |
| `shadow-modal` | `0px 5px 20px 0px rgba(75, 70, 92, 0.40)` |
| `shadow-primary` | `0px 4px 16px 0px rgba(115, 103, 240, 0.45)` |
| `shadow-primary-sm` | `0px 2px 3px 0px rgba(115, 103, 240, 0.30)` |
| `shadow-danger` | `0px 4px 16px 0px rgba(234, 84, 85, 0.45)` |
| `shadow-success` | `0px 4px 16px 0px rgba(40, 199, 111, 0.45)` |
| `shadow-warning` | `0px 4px 16px 0px rgba(255, 159, 67, 0.45)` |
| `shadow-info` | `0px 4px 16px 0px rgba(0, 118, 229, 0.45)` |
| `shadow-gray` | `0px 4px 16px 0px rgba(165, 163, 174, 0.45)` |
| `shadow-darkyellow` | `0px 4px 16px 0px rgba(215, 179, 0, 0.45)` |

Each status shadow also has `-sm` and `-lg` variants. Use `shadow-primary` only on primary CTAs. Use `shadow-card` for all cards. Use `shadow-modal` for overlays/dialogs.

---

## COMPONENT TOKENS

### Button

| Variant | Background | Text | Border | Hover bg |
| --- | --- | --- | --- | --- |
| `btn-primary` | `brand-primary-default` | `#fff` | transparent | `brand-primary-hover` |
| `btn-secondary` | `bg-surface` | `brand-primary-default` | `brand-primary-default` | `brand-primary-tint` |
| `btn-ghost` | transparent | `text-body` | `border-default` | `bg-selected` |
| `btn-destructive` | `color-error` | `#fff` | transparent | `#D44849` |
| `btn-destructive-ghost` | transparent | `color-error` | `color-error` | `#FEF2F2` |

| Size | Height | H-padding | Font |
| --- | --- | --- | --- |
| `.btn-xs` | 24px | 8px | 12px |
| `.btn-sm` | 32px | 12px | 12px |
| `.btn` | 40px | 16px | 14px |
| `.btn-lg` | 48px | 24px | 16px |

- Radius → `radius-md` for all sizes. Disabled → `bg-disabled` + `text-disabled` + `pointer-events: none`.
- Button text never wraps — `white-space: nowrap`. Shorten labels rather than letting the button grow taller.

### Input
- Border: default → `border-default`; hover → `border-emphasis`; focus → `brand-primary-default`; error → `color-error`
- Label always visible above. Error message below in `color-error`. Placeholder → `text-placeholder`. Radius → `radius-md`.

### Card
- `bg-surface` + `rounded="card"` (6px) + `density="comfortable"` + `shadow-card` — no explicit border by default

### Icons
- Always use `<Icon>` — never `<VIcon>` or `@iconify/vue` directly in feature code.
- Default family: `tabler`. Icons served via Iconify API.

```vue
<Icon icon="x" />
<Icon icon="chevron-down" size="16" />
<Icon icon="robot" size="20" color="#7367F0" />
<Icon icon="check" family="mdi" />   <!-- only when non-tabler set needed -->
```

---

## LAYOUT RULES

- Default → vertical stack, left-aligned. Center only for empty states and chat surface.
- **Dashboard:** 260px sidebar (collapsible to 72px), 24px content gutter, full-width cards.
- **Chat surface:** single-column, centered, max 760px, input anchored bottom, bubbles `radius-lg`.
- No `backdrop-filter: blur`. No neon gradients. No full-bleed photography.

---

## INTERACTION RULES

- Define all three states on every interactive element: hover, active, disabled.
- Provide feedback for: success, error, loading.
- Transitions: state changes 120ms; entry/exit 200–280ms; easing `cubic-bezier(.2,.7,.3,1)`.
- Tooltips: white bg (#fff), `shadow-card`, `rounded-md` — never dark.

---

## COPYWRITING RULES

- Sentence case ("Create agent", not "Create Agent"). Title case for plan/product names only.
- You-forward, action verbs, present tense. No hype adjectives.
- "Knowledge base" / "sources" — not "RAG" or "embeddings".
- No `text-transform: uppercase` in UI labels — exception: `th` elements get uppercase automatically via `addBase` (data table column headers only).

---

## ANTI-PATTERNS (FORBIDDEN)

- Raw hex or px values instead of tokens
- Arbitrary spacing (18px, 10px, 6px) — 4-point grid only
- New colors outside the defined scale
- `backdrop-filter: blur`, emoji in UI, typefaces beyond Inter / Public Sans / JetBrains Mono
- Disabled state using a tinted primary color
- P3 overrides on neutral tokens
- Designing colors in sRGB first — always P3-first on XDR, then derive sRGB fallback

---

## FILE REFERENCES

| Asset | Path |
| --- | --- |
| CSS tokens | `<root>/colors_and_type.css` |
| Logo mark | `<root>/assets/logo-48px.svg` |
| Logo wordmark | `<root>/assets/logo-wordmark.svg` |
| Dashboard UI kit | `<root>/ui_kits/dashboard/index.html` |
| Chat UI kit | `<root>/ui_kits/chat/index.html` |
| Preview cards | `<root>/preview/*.html` |
| Extended component library | `/Users/miodragristovski/Desktop/settings-panel.html` |
| Inter variable font | `<root>/fonts/Inter-VariableFont_opsz_wght.ttf` |

Link tokens CSS in any new HTML output:
```html
<link rel="stylesheet" href="/Users/miodragristovski/Downloads/CustomGPT.ai Design System-2/colors_and_type.css">
```

**Preview cards:** `brand-mark` · `colors-primary` · `colors-gray` · `colors-semantic` · `colors-opacity` · `colors-gradients` · `type-headings` · `type-body` · `spacing-scale` · `radius-scale` · `shadows` · `iconography` · `components-buttons` · `components-inputs` · `components-badges` · `components-avatars` · `components-cards` · `components-alerts` · `components-chat` · `components-composer`

**Extended library** (`settings-panel.html`): `button` (5 variants × 4 sizes + disabled) · `input` · `badge` · `avatar` · `alert` · `checkbox` · `toggle` · `select` · `tabs` · `segmented-control` · `tooltip` · `project-card` · `paginator` · `citation-panel` · `plan-card` · `change-password-card` · `two-factor-auth-card` · `confirm-password-modal`

---

## HOW TO USE

1. Read `colors_and_type.css` — every CSS variable and `.cg-*` class lives there. Always link it.
2. Browse `preview/*.html` — copy patterns from these rather than inventing new ones.
3. Start from a UI kit — `ui_kits/dashboard/` for admin/studio, `ui_kits/chat/` for the chat surface.
4. For component specs (plan card, paginator, citation panel, etc.) — read `settings-panel.html` directly.

---

## VUETIFY — PRIMARY COMPONENT FRAMEWORK

All new features use Vuetify. All Vuetify components are wrapped in `@/Components/CustomGPT/`. Never use raw `v-*` components in feature code.

### Component wrapper library

**Buttons** (built on `ButtonBase.vue` → `v-btn`):
- `PrimaryButton` (color=primary, variant=flat) · `DangerButton` (color=danger, variant=flat) · `ButtonBase` (avoid in feature code)
- `SecondaryButton` (color=secondary #A8AAAE gray, variant=flat) — NOT an outlined primary button
- `IconButton` (variant=text, color=default) — for modal close: `variant="flat" color="white" density="compact" class="absolute right-[-1rem] top-[-1rem] z-10 shadow"`
- All accept: `type`, `block`, `disabled`, `flat`, `density`, `variant`, `rounded`
- Primary/Secondary/Danger also accept `prepend` / `default` / `append` slots

**Inputs:**
- `TextInput` (`VTextField`, compact, outlined, flat, rounded=md, hide-details, bg=#fff) — expose `focus()` method
- `TextInputWithIcon` · `Checkbox` · `CheckboxGroup` · `RadioButton`
- `RadioButtonGroup` — prop is `:options` (array of `{ value, label }`), NOT `:items`
- `Switch` (inset, primary, compact) · `DatePicker` · `TimeInput` · `ColorPicker` / `ColorPickerInput`

**Containers:**
- `Card` (`VCard`, `rounded="card"` 6px, `density="comfortable"`, `shadow-card` — slots: `title`, `subTitle`, `body`, `actions`; props: `noBodyPadding`, `noTitlePadding`, `noActionsPadding`)
- `CardWithLoader` · `CardWithScrollBox`
- `Modal` (`VDialog`, width=500 default, optional close button) · `ModalWithCard` · `ModalWithCardWithLoader`
- `NavigationDrawer`

**Feedback:**
- `Alert` (prop: `status` — `"danger"`, `"success"`, `"warning"`, `"info"` — NOT `type`)
- `SnackBar` (location="top end", variant="flat") · `Tooltip` · `CircularLoader` · `ProgressBar`

**Display:**
- `Chip` (`bgCustomOpacity` prop, default 0.12) · `ChipWithIcon` · `Tag` · `Icon` · `Pagination` · `HorizontalTabs`
- `DropdownMenu` (`VMenu`, width=225, close-on-content-click=false — slots: `activator`, `default`) · `DropdownList`

### Theme setup (`plugins/vuetify.js`)

```js
export default createVuetify({
  theme: {
    defaultTheme: 'customgpt',
    themes: {
      customgpt: {
        dark: false,
        colors: {
          primary:    '#7367F0',
          secondary:  '#A8AAAE',
          success:    '#28C76F',
          warning:    '#FF9F43',
          danger:     '#EA5455',  // NOT 'error'
          info:       '#0076E5',
          background: '#F5F5F5',
          surface:    '#FFFFFF',
          teal:       '#46AEAE',
          orange:     '#F7922F',
          darkyellow: '#D7B300',
          gray:       '#A5A2AD',
          white:      '#FFFFFF',
        },
      },
    },
  },
  defaults: {
    VBtn:       { elevation: 0 },
    VCard:      { elevation: 0 },
    VTextField: { variant: 'outlined', rounded: 'md', density: 'compact', flat: true, hideDetails: true },
    VSelect:    { variant: 'outlined', rounded: 'md', density: 'compact', hideDetails: true },
    VSwitch:    { color: 'primary', density: 'compact', inset: true, hideDetails: true },
    VCheckbox:  { color: 'primary', density: 'compact', hideDetails: true },
  },
})
```

### Vuetify `rounded` prop → radius token map

| `rounded` prop | SCSS value | Design token | Use |
| --- | --- | --- | --- |
| `sm` | 4px | `radius-sm` | small controls, tooltips |
| `md` | 6px | `radius-md` | buttons, inputs |
| `card` | 6px | — | `Card.vue` default |
| `xxl` | 12px | — | — |
| `pill` | 9999px | `radius-full` | chips, badges, avatars |

> `$border-radius-root: 4px` in `vuetify.scss`. `Card.vue` uses `rounded="card"` (6px).

### Key component patterns

**Tooltip** — always use the wrapper (raw `VTooltip` renders dark):
```vue
<Tooltip parent text="Explain this setting" />
<Tooltip text="Explain this setting">
  <template #activator><MyTriggerElement /></template>
</Tooltip>
<Tooltip parent text="Simple tip" :without-ask-ai="true" />
```

**Alert:**
```vue
<Alert status="danger" message="Something went wrong." :closable="true" />
<Alert status="success"><template #title>Saved</template></Alert>
```

**Switch:**
```vue
<Switch v-model="isEnabled"><template #label>Enable feature</template></Switch>
<Switch v-model="isEnabled" size="large" />
```

**Card:**
```vue
<Card>
  <template #title>Heading</template>
  <template #body>Content</template>
  <template #actions><PrimaryButton>Save</PrimaryButton></template>
</Card>
<!-- No padding variants: -->
<Card :no-body-padding="true" :no-title-padding="true" />
```
`Card` uses `rounded="card"` (6px) and `class="shadow-card"` automatically — do not add them again.

**Modal — close button is outside the card, not inside:**
```vue
<!-- Modal.vue renders IconButton BEFORE the card slot, positioned absolutely -->
<Modal v-model="isOpen" :show-close-button="true">
  <!-- IconButton class="absolute right-[-1rem] top-[-1rem] z-10 shadow"
       variant="flat" density="compact" color="white" -->
</Modal>
```

**ModalWithCard:**
```vue
<ModalWithCard v-model="isOpen" :show-close-button="true">
  <template #title>Dialog Title</template>
  <template #content>Content</template>   <!-- slot is #content, NOT #body -->
  <template #actions><PrimaryButton>Confirm</PrimaryButton></template>
</ModalWithCard>
```

⚠ `ModalWithCard` maps `#content` → `Card #body`. Using `#body` directly does nothing.

**DropdownMenu:**
```vue
<DropdownMenu :width="225" location="bottom">
  <template #activator="props"><PrimaryButton v-bind="props">Options</PrimaryButton></template>
  <DropdownList><!-- items --></DropdownList>
</DropdownMenu>
```

**Custom CSS in SFCs:**
```vue
<style scoped lang="scss">
.my-element { @apply bg-white shadow-card rounded-xl p-6; }
:deep(.v-card__text) { @apply text-body; }
</style>
```

### Key SCSS settings (`/Users/miodragristovski/Downloads/vuetify.scss`)

```scss
$reset: false           // Tailwind coexists
$utilities: false       // Tailwind handles utilities
$color-pack: false      // Tailwind handles colors
$body-font-family: 'Public Sans', sans-serif
$border-radius-root: 4px
$card-elevation: 6      // Card.vue overrides via shadow-card class
$card-text-padding: 24px
$button-height: 40px
$tabs-height: 42px
$switch-inset-track-height: 1.125rem   // 18px — do not override
$switch-inset-track-width:  1.875rem   // 30px — do not override
$tooltip-background-color: #212121    // Tooltip.vue overrides to white bg
$grid-breakpoints: (xs: 0, sm: 576px, md: 768px, lg: 992px, xl: 1200px, xxl: 1400px)
```

### Production color reference

| Vuetify key | Hex | Tailwind key |
| --- | --- | --- |
| `primary` | `#7367F0` | `primary.DEFAULT` |
| `secondary` | `#A8AAAE` | `secondary.DEFAULT` |
| `success` | `#28C76F` | `success.DEFAULT` |
| `warning` | `#FF9F43` | `warning.DEFAULT` |
| `danger` | `#EA5455` | `danger.DEFAULT` |
| `info` | `#0076E5` | `info.DEFAULT` |
| `surface` | `#FFFFFF` | `white` |
| `background` | `#F5F5F5` | body bg |

**Text / UI colors (Tailwind only — not Vuetify theme keys):**
`text-heading` #212121 · `text-body` #565656 · `text-muted` #B7B5BE · `text-placeholder` #999999

### Vuetify anti-patterns (FORBIDDEN)

- Raw `<v-btn>`, `<v-card>`, etc. — always use custom wrapper
- `color="error"` — use `color="danger"`
- `variant="elevated"` on buttons — use `variant="flat"` or `variant="outlined"`
- Color prop as raw hex — use named theme color keys
- Dark tooltips — always use `Tooltip` wrapper
- `elevation > 0` on cards — use `Card` component with `class="shadow-card"`
- Omitting `hide-details` on inputs/checkboxes (unless validation messages are needed)
- Hardcoding `border-radius` px values — use `rounded="*"` or Tailwind classes
- Button text uppercase — globally: `.v-btn { text-transform: none !important; letter-spacing: normal !important; }`
- Button text wrapping — always `whitespace-nowrap`; shorten the label instead
- `elevation > 0` on `VBtn` globally — apply `shadow-cta` via Tailwind class explicitly on CTAs only
