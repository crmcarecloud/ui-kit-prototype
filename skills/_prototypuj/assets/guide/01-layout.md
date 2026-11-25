# Layout a stavba stránek

Jak je poskládaná obrazovka aplikace — od shellu po obsah stránky a její stavy.

## Aplikační shell

- Struktura: fixní levý sidebar + sticky header + `<main>` obsah. Referenční implementace je `AppShell` ve `web/components/campaigns/app-shell.tsx`.
- Hlavní sidebar je brand plocha — tmavé `--cc-sidebar` pozadí, položky menu s ikonou a labelem, aktivní položka má `cc-gradient-action`.
- Sidebar lze přepnout do režimu `custom` (portal `#cc-sidebar-slot`) pro kontextové panely — používá ho composer přípravy kampaně.
- Header: aplikační breadcrumb/název vlevo, globální akce a uživatel vpravo.
- Alternativa pro jednodušší aplikace: kit komponenta `Sidebar` + `SidebarProvider` + `SidebarInset` (sbalitelná, s `SidebarTrigger`).

## Anatomie stránky

1. Hlavička stránky: `h1` název + krátký popis v `text-muted-foreground`, primární akce vpravo (např. „Nová kampaň").
2. Případně druhá řada: vyhledávání (`InputGroup` s ikonou), filtry, přehledové `Badge`/`Tabs`.
3. Obsah: karty nebo sekce. Sekci odděluj nadpisem `h2`, ne prázdným místem.
4. Páté akce (uložit/zrušit) drž v `CardFooter` nebo sticky liště dole — ne uprostřed obsahu.

- Odsazení stránky používá token `--cc-space-page` (clamp podle viewportu).
- Primární akce stránky je vždy jen jedna a používá výraznou variantu (`Button` default nebo `cc-gradient-action`); vedlejší akce jsou `outline`/`ghost`.

## Seskupování obsahu

| Potřeba | Prvek |
| --- | --- |
| Samostatný celek s hlavičkou a akcemi | `Card` (`CardHeader`, `CardTitle`, `CardAction`, `CardContent`, `CardFooter`) |
| Podsekce v rámci stránky/detailu | `Tabs` |
| Pokročilé nebo sekundární nastavení | `Accordion` / `Collapsible` |
| Jen vizuální oddělení | `Separator` |
| Dlouhý panel s vlastním scrollem | `ScrollArea` |

## Navigace — co kdy použít

| Situace | Prvek |
| --- | --- |
| Hlavní navigace aplikace | `Sidebar` (nebo `AppShell`) |
| Přepínání sekcí v rámci jedné stránky | `Tabs` |
| Orientace v hierarchii (seznam → detail) | `Breadcrumb` |
| Stránkování dlouhého seznamu/tabulky | `Pagination` |
| Top navigace s rozbalovacími panely | `NavigationMenu` |
| Aplikační lišta menu (desktop) | `Menubar` |

- `Tabs` vs. `Sidebar`: tabs řeší podsekce jednoho celku (detail kampaně), sidebar řeší hlavní oblasti aplikace.
- `Breadcrumb` doplňuje sidebar u hlubokých struktur — nenahrazuje ho.

## Stavy stránky

- Načítání: `Skeleton` ve tvaru budoucího obsahu; `Spinner` jen pro krátké inline operace nebo v tlačítku.
- Prázdný stav: `Empty` + `EmptyMedia` + `EmptyTitle` + `EmptyDescription` + primární akce v `EmptyContent`. Nikdy nenechávej prázdnou stránku bez vysvětlení a dalšího kroku.
- Chyba celé stránky/sekce: `Alert` (variant `destructive` pro chybu) s akcí na opakování.
- Částečná data (načítá se doplněk): `Skeleton` místo konkrétního bloku, ne celá stránka.

## Responzivita

- Sidebar se pod `lg` skrývá — hlavní akce musí být dostupné i bez něj.
- Obsah skládej přes `grid`/`flex` s breakpointy (`sm:`, `lg:`); tabulky v přesných šířkách obal do horizontálního scrollu.
- Touch alternativy: na mobilu preferuj `Drawer` místo `Sheet`, bottom akce místo hover závislostí.
