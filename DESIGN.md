# Al Rahma HMS — design

Mode: **Operate.** Staff are inside a task. Scanability, consistency and
predictable affordances outrank expression. The brand lives in precise
details, not in decoration.

## Palette

Sampled from the hospital's own logo, then extended into a usable scale.

| Token | Hex | Role |
|---|---|---|
| `--arh-blue` | `#01AFEE` | Brand. Logo areas, active-state marker. Decorative only. |
| `--arh-green` | `#6DBC45` | Brand. Decorative only. |
| `--arh-blue-700` | `#0B6E96` | Primary actions, links. 5.70:1 on white. |
| `--arh-blue-800` | `#09576F` | Sidebar base, hover of primary. |
| `--arh-green-700` | `#3D7A24` | Success text, "open" status. |
| `--arh-ink` | `#10212B` | Body text. 16.5:1. |
| `--arh-canvas` | `#F5F8FA` | Page. A second, cooler neutral under white panels. |

The two brand colours fail WCAG AA for text, so they never carry text. This is
recorded in `docs/branding.md` and enforced by the token names themselves: a
developer reaching for `--arh-blue` for a label has to pass `--arh-blue-700`
on the way.

Clinical status colours are deliberately separate from the brand. Green means
"result within range", never "Al Rahma".

## Type

One family, a tuned system stack led by Segoe UI. A webfont was rejected: the
server has no outbound internet and the workstations are Windows, so a font
that fails to load would reflow every form mid-shift. Operate surfaces are
explicitly allowed familiar sans defaults, and this is a deliberate use of
that allowance rather than a lack of choice.

Fixed rem scale, ratio ≈1.14, 12→22px. Product UI has many type roles; a wide
ratio turns a dense screen into noise. Tabular numerals globally — proportional
digits make two amounts of equal length look unequal, which is exactly wrong on
an invoice.

## Layout

Fixed 248px sidebar, sticky 56px top bar, 1400px content column. Responsive
behaviour is structural: below 920px the sidebar becomes an overlay. Typography
does not fluidly scale; users view at a consistent distance on a consistent
screen.

```
┌──────────┬────────────────────────────────────────────┐
│ logo     │ search            name · role · sign out   │
│──────────┼────────────────────────────────────────────┤
│ Dashboard│  Hospital structure            [Add sector]│
│          │  Sectors, departments and units …          │
│ PATIENT  │ ┌────────────────────────────────────────┐ │
│  Patients│ │ ▸ Clinical Services          CLIN      │ │
│  Queue   │ │   ├ Outpatient (OPD)   Core  OPD       │ │
│          │ │   └ Maternity                MAT       │ │
│ ADMIN    │ │ ▸ Diagnostic & Support       SUPP      │ │
│  Sectors │ └────────────────────────────────────────┘ │
└──────────┴────────────────────────────────────────────┘
```

## Principles

1. **The structure screen is a tree, not a table.** The hospital's shape is
   hierarchical; flattening it into rows would hide the one thing the screen
   exists to show.
2. **Row actions appear on hover and on focus, and are always visible on
   touch.** Density without a wall of buttons.
3. **Destructive actions explain the consequence and name the safe
   alternative.** "Close it instead so the history stays intact" is the
   sentence that prevents the support call.
4. **A nav item the person cannot use is not rendered.** A menu full of 403s
   teaches staff to distrust the menu.
5. **Browser surfaces are themed** — selection, focus ring, scrollbars — because
   those are the parts that give away an assembled interface.
6. **Empty states teach.** "The structure is empty" offers the first action
   rather than announcing nothing.
