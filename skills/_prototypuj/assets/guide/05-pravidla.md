# Pravidla a checklist

Rychlá rozhodovací pravidla, typické chyby a kontrolní seznam pro novou obrazovku.

## Rozhodovací pravidla

- Akce → `Button` (jedna primární na kontext). Skupina voleb vzhledu → `ToggleGroup`. Navigace → `a`/sidebar, ne tlačítko.
- Hodnota do formuláře → `Select`/`Combobox`/`RadioGroup`. Spuštění akce z menu → `DropdownMenu`.
- Krátká zpětná vazba po akci → `toast`. Informace, která má zůstat → `Alert`. Chyba pole → `FieldError`.
- Potvrzení destruktivní akce → `AlertDialog`. Soustředěný úkol → `Dialog`. Vedlejší panel → `Sheet`. Mobil → `Drawer`.
- Obsah se načítá → `Skeleton`. Operace běží → `Spinner`/`Progress`. Kolekce je prázdná → `Empty`.
- Data ve sloupcích → `Table`. Klikací entity → `Item`/`Card`. Podsekce detailu → `Tabs`. Hierarchie → `Breadcrumb`.

## Do / Don't

- **Do**: sémantické tokeny (`bg-card`, `text-muted-foreground`), `cn()` pro skládání tříd, `data-icon` u ikon v tlačítkách.
- **Don't**: hardcoded barvy a hex kódy mimo `--cc-*`/tokeny — rozbíjí skin a dark režim.
- **Do**: `aria-label` u icon-only tlačítek, `FieldLabel` u každého pole, `DialogDescription` v dialogu.
- **Don't**: placeholder jako label, tooltip pro povinnou informaci, toast pro chybu validace.
- **Do**: české texty včetně `aria-label` a popisů; popisky tlačítek jako slovesa („Uložit koncept").
- **Don't**: více primárních akcí v jednom kontextu, `Dialog` na potvrzení mazání, vlastní paralelní komponenty místo kitu.
- **Do**: komponenty a utility `cn`/`uuid` z `window.CCUI`; dostupnost ověř v `exports.json`.
- **Don't**: nové importy CSS, `@source` na lokální soubory nebo přepis vloženého runtime; styly kitu jsou již v šabloně.

## Ikony a typografie

- Ikony výhradně `lucide-react`, výchozí velikost `size-4`; v `Button` bez textu `size="icon"` + `aria-label`.
- Ikona + text v tlačítku: `<Icon data-icon="inline-start" />` (příp. `inline-end`) — řeší mezery samo.
- Nadpisy: stránka `h1` + popis, sekce `h2`; `font-heading` pro brand nadpisy; muted text přes `text-muted-foreground`.

## Motion

- Délky a křivka jen z tokenů: `--cc-motion-fast` (hover/focus), `--cc-motion-base` (přechody panelů), `--cc-motion-slow` (vstupní animace), `--cc-ease`.
- Vestavěné animace: karty (`cc-scale-in`), obsah `<main>` (`cc-rise`), dialogy — nespamuj vlastními keyframes.
- `prefers-reduced-motion` je vyřešené globálně — vlastní animace přidávej jen za smysluplným účelem (orientace, zpětná vazba).

## Přístupnost

- Každá interakce je ovladatelná klávesnicí — Base UI primitiva to řeší, neobcházej je nativními div onClick hacky.
- Icon-only prvky: `aria-label` + `Tooltip`. Disabled prvky vysvětli (description/tooltip), ne jen zhasni.
- Stavy nechte na `data-*` atributech (`data-open`, `data-disabled`) — styly i testy se na ně spoléhají.

## Checklist pro novou obrazovku

1. Shell: má prototyp sidebar/navigaci podle zadání? Primární akce stránky definovaná?
2. Obsah: karty/sekce se sémantickými tokeny, žádné hardcoded barvy.
3. Stavy: skeleton pro načítání, `Empty` pro prázdnou kolekci, `Alert` pro chybu.
4. Formuláře: `Field` anatomie, `FieldError` validace, správný vstup dle tabulky.
5. Akce: destruktivní přes `AlertDialog`, výsledek přes `toast`, primární akce jedna.
6. Texty česky, ikony lucide, `aria-label` u icon-only.
7. Ověření: otevři výsledné HTML přes `file://` s internetem; zkontroluj konzoli, navigaci, formuláře, dialogy, klávesnici a mobilní šířku. Pokud nemáš prohlížeč k dispozici, uveď, že běh nebyl ověřen. Kontroly npm původního monorepa se na samostatný HTML soubor nevztahují.
