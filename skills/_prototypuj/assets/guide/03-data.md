# Data, seznamy a stavy

Jak zobrazovat data, kolekce entit a průběh operací.

## Reprezentace kolekcí

| Povaha dat | Prvek |
| --- | --- |
| Strukturovaná data se sloupci, porovnání hodnot | `Table` |
| Klikací řádky s médiem, popisem a akcemi | `Item` v `ItemGroup` |
| Entity jako vizuální celky (kampaně, šablony) | `Card` grid |
| Plochý seznam v panelu/menu | `Item` bez `ItemGroup` |

- `Table`: hlavička `TableHeader`/`TableHead`, čísla zarovnávej doprava (`text-right`), stavy vykresluj `Badge`, per-row akce přes `DropdownMenu` nebo ghost icon button v poslední buňce. `TableCaption` pro přístupný popis.
- `Item`: `ItemMedia` (ikona/avatar) + `ItemContent` (`ItemTitle`, `ItemDescription`) + `ItemActions`; řádky odděluj `ItemSeparator`.
- `Card` grid: jednotný obsah karet (název + `Badge` stav v `CardAction`, metriky v `CardContent`, akce v `CardFooter`).

## Stavy a hodnoty

- Stav entity (Koncept, Naplánovaná, Odeslaná, Chyba) vždy přes `Badge` — default/secondary/outline/destructive dle významu.
- Uživatele zobrazuj `Avatar` + `AvatarFallback` (iniciály), skupiny přes `AvatarGroup` + `AvatarGroupCount`.
- Krátké kódy a klávesy: `Kbd`/`KbdGroup`.
- Média s fixním poměrem stran: `AspectRatio`.

## Průběh a načítání

| Situace | Prvek |
| --- | --- |
| Načítá se obsah bloku/stránky | `Skeleton` ve tvaru obsahu |
| Krátká operace, inline | `Spinner` |
| Operace v tlačítku | `disabled` + `Spinner` s textem („Odesílám…") |
| Měřitelný průběh (import, odeslání) | `Progress` (`ProgressTrack`, `ProgressIndicator`, `ProgressValue`) |
| Globální async indikace v aplikaci | `cc-async-loader` lišta (`web/app/globals.css`) |

## Prázdné a okrajové stavy

- Prázdná kolekce: `Empty` s ikonou (`EmptyMedia variant="icon"`), vysvětlením a primární akcí („Nová kampaň").
- Žádné výsledky hledání/filtru: `Empty` s jiným textem („Nic nenalezeno") a akcí na reset filtru.
- Chyba načtení: `Alert` s možností opakovat — ne toast, chyba má zůstat viditelná.

## Organizace delšího obsahu

- `Pagination` pod tabulkami a seznamy.
- `ScrollArea` pro panely s vlastním scrollem (seznam v dialogu, sidebar) — ne pro hlavní stránku.
- `Carousel` pro horizontální procházení náhledů (varianty šablon).
- `Resizable` (`ResizablePanelGroup`) pro panely, kde uživatel ladí poměr (editor ↔ náhled).
- `Skeleton` vs `Spinner`: skeleton pro obsah, který se teprve vykreslí; spinner pro operaci, která právě běží.
