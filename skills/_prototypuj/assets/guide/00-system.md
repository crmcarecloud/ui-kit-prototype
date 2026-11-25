# Systém UI kitu

Průvodce pravidly a stavbou aplikace na CareCloud UI kitu. Určený pro programátory i AI agenty — popisuje, jak je systém poskládaný a kdy který prvek použít.

## Samostatný HTML prototyp

Tento plugin obsahuje hotový browserový build UI kitu v `../prototyp.html`.
Zdrojové monorepo ani npm nejsou potřeba pro vytvoření prototypu.

- Začni kopií přibaleného HTML. React a ReactDOM načítá existující import mapa.
- Komponenty a utility ber z `window.CCUI`, například `const { Button, Dialog, cn } = window.CCUI`.
- Dostupné názvy ověř v `../exports.json`; neimportuj balíček `ui` ani soubory z monorepa.
- Kompilované CSS a téma jsou vložené v HTML. Nepřidávej CSS importy ani `@source` na lokální zdroje.
- Nové styly patří pouze do označeného bloku `NOVÉ STYLY PROTOTYPU`.
- Runtime mění správce při obnově bundlu podle README pluginu, nikoli agent při tvorbě prototypu.

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

## Skládání obrazovek

Používej exportované komponenty z `window.CCUI`. Doménové prvky (shell,
výběr zákazníka, výběr času) sestav přímo v JSX prototypu nad těmito
komponentami a mock daty. Soubory původní aplikace nejsou součástí pluginu.

## Základní pravidla

- Uživatelské texty jsou česky, včetně `aria-label` a placeholderů.
- Ikony pouze z `lucide-react`; v tlačítkách s textem používej `data-icon="inline-start"` / `inline-end`.
- Motion řídí tokeny `--cc-motion-fast/base/slow` a `--cc-ease`; `prefers-reduced-motion` je vyřešené globálně v `theme.css`.
- Nepřidávej paralelní komponenty — nejdřív hledej v katalogu `window.CCUI`.
