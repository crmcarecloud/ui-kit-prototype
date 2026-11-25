# Systém UI kitu

Průvodce pravidly a stavbou aplikace na CareCloud UI kitu. Určený pro programátory i AI agenty — popisuje, jak je systém poskládaný a kdy který prvek použít.

## Architektura

- `packages/ui` je sdílená komponentová knihovna. Je **source-only** — konzument (web, docs, email-designer) kompiluje TSX a generuje Tailwind styly sám.
- Komponenty stojí na `@base-ui/react` primitivách, varianty řeší `class-variance-authority`, ikony jsou `lucide-react`.
- Styly jsou Tailwind CSS 4 utility třídy. Téma a výchozí CareCloud skin definuje `packages/ui/theme.css`.
- Vše se importuje z barrelu: `import { Button, Dialog, cn } from 'ui'`.

## Napojení stylů

Hlavní CSS soubor konzumenta musí obsahovat:

```css
@import 'tailwindcss';
@import 'ui/theme.css';

/* povinné — bez @source jsou komponenty kitu neostylované */
@source '../packages/ui/src';
```

- `theme.css` už obsahuje `tw-animate-css` a `shadcn/tailwind.css` — neimportuj je znovu.
- `file:` závislost vyžaduje sdílené instance Reactu — konzument musí mít v `vite.config.ts` `resolve.dedupe` pro `react`, `react-dom`, `@base-ui/react` a další peer závislosti (vzor: `ui-kit-docs/vite.config.ts`).

## Tokeny a skinování

Tok barvy teče třemi vrstvami:

1. `--cc-*` proměnné v `:root` jsou zdroj skinu (`--cc-brand-blue`, `--cc-canvas`, `--cc-text`, `--cc-radius-control`, `--cc-motion-base` …).
2. Sémantické tokeny (`--background`, `--primary`, `--muted-foreground`, `--ring` …) se od `--cc-*` odvozují v `:root` a `.dark` bloku `theme.css`.
3. Tailwind utility (`bg-primary`, `text-muted-foreground`, `border-border`) mapují na sémantické tokeny přes `@theme inline`.

Pravidla:

- V komponentách a stránkách piš **sémantické utility** (`bg-card`, `text-foreground`), ne `bg-white` ani hex barvy — jinak skin a dark režim nefungují.
- Brand plochy (hlavní sidebar, gradient akce) smí sahat přímo na `--cc-*` (`bg-[var(--cc-sidebar)]`, `var(--cc-gradient)`).
- Nový skin = přepsání `--cc-*` (případně sémantických tokenů) ve vlastním `:root` za importem `theme.css`. Komponenty se nemění.
- Dark režim se zapíná třídou `.dark` na libovolném rodiči; tokeny pro dark jsou v `.dark` bloku `theme.css`.

## Utility

- `cn(...)` — `clsx` + `tailwind-merge`; používej pro skládání `className`, deduplikuje konfliktní třídy.
- `uuid()` — generování ID (položky seznamů, `htmlFor`/id páry).
- `useIsMobile()` — breakpoint hook pro mobilní varianty chování.

## Komponenty

- Každá komponenta má `data-slot` atribut (např. `data-slot="card"`) — slouží pro cílený polish v `theme.css` a pro testy.
- Všechny komponenty přijímají `className` a skládají ho přes `cn` — doladění dělej přes `className`, ne přes nové varianty.
- Složené komponenty (Dialog, Select, Table …) se montují z pojmenovaných částí (`DialogContent`, `TableRow` …) — drž se exportovaných částí, nepřepisuj jejich vnitřní strukturu.
- Base UI `render` prop slouží k výměně elementu (např. `CollapsibleTrigger render={<Button />}`).

## Kam co patří

- `packages/ui/src/` — generické komponenty použitelné napříč projekty. Nové shadcn komponenty se sem přesouvají (postup v `packages/ui/README.md`).
- `web/components/ui/` — aplikační komponenty vázané na doménu (`entity-picker-dialog`, `date-time-picker`, `chart`).
- `web/components/campaigns/` — feature komponenty konkrétní oblasti.
- `web/components/campaigns/app-shell.tsx` — aplikační shell (sidebar, header, main slot).

## Základní pravidla

- Uživatelské texty jsou česky, včetně `aria-label` a placeholderů.
- Ikony pouze z `lucide-react`; v tlačítkách s textem používej `data-icon="inline-start"` / `inline-end`.
- Motion řídí tokeny `--cc-motion-fast/base/slow` a `--cc-ease`; `prefers-reduced-motion` je vyřešené globálně v `theme.css`.
- Nepřidávej paralelní komponenty — nejdřív hledej v `ui`, potom ve `web/components/ui`.
