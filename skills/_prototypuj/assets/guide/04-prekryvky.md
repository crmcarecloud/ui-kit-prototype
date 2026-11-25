# Překryvné prvky a zpětná vazba

Dialogy, menu, nápovědy a oznámení — co kdy použít a čemu se vyhnout.

## Modální vrstvy

| Situace | Prvek |
| --- | --- |
| Úkol v modálním okně (formulář, výběr) | `Dialog` |
| Potvrzení nevratné/destruktivní akce | `AlertDialog` |
| Detail nebo panel z boku, delší obsah | `Sheet` |
| Bottom sheet (primárně mobil) | `Drawer` |

- `Dialog` má `DialogHeader` (`DialogTitle`, `DialogDescription`), obsah a `DialogFooter` s akcemi. `DialogDescription` nevynechej — nese kontext pro screen reader.
- `AlertDialog` je povinný u mazání a nevratných akcí: `AlertDialogTitle` popisuje důsledek, `AlertDialogAction` je destruktivní `Button`, `AlertDialogCancel` je výchozí cesta ven. Nikdy nepoužívej obyčejný `Dialog` na potvrzení.
- `Sheet` vs `Dialog`: sheet pro obsah, který souvisí se stránkou pod ním (detail entity, filtry); dialog pro soustředěný úkol, který stránku přerušuje.
- Composer/editor jako celá stránka ≠ překryvka — to je route/view, ne dialog.

## Kontextové plochy

| Situace | Prvek |
| --- | --- |
| Malý obsah navázaný na prvek (filtr, výběr barvy) | `Popover` |
| Krátký popis ikony/ovladace při hoveru | `Tooltip` |
| Bohatý náhled entity při hoveru (karta kontaktu) | `HoverCard` |

- `Tooltip` jen pro krátký text doplněk — nikdy pro povinnou informaci nebo interaktivní obsah. Icon-only `Button` má vždy `aria-label` + `Tooltip`.
- `Popover` vs `Tooltip`: popover nese interaktivní obsah a otevírá se klikem; tooltip je jen label.
- `HoverCard` je desktop vylepšení — obsah musí být dostupný i jinak (detail po kliku).

## Menu

| Situace | Prvek |
| --- | --- |
| Akce u řádku/karty („⋯") | `DropdownMenu` |
| Pravý klik na položku | `ContextMenu` |
| Aplikační lišta (desktop) | `Menubar` |
| Paleta příkazů ⌘K / vyhledávání akcí | `Command` |

- `DropdownMenu` seskupuj přes `DropdownMenuGroup`/`DropdownMenuLabel`, destruktivní položky odděl `DropdownMenuSeparator` a označ destruktivní variantou.
- `ContextMenu` je doplněk — stejné akce musí být dostupné i z viditelného menu.
- `Command` používej pro globální hledání/paletu, typicky v `Dialog` (command palette vzor).

## Oznámení a zpětná vazba

| Situace | Prvek |
| --- | --- |
| Potvrzení dokončené akce („Uloženo") | `toast` + `<Toaster />` v rootu |
| Trvalá informace/varování uvnitř obsahu | `Alert` |
| Chyba u konkrétního pole formuláře | `FieldError` |
| Potvrzení před nevratnou akcí | `AlertDialog` |

- Toast voláš přes `toast.add({ title, description })`; vyžaduje `<Toaster />` v rootu aplikace.
- Toast je pomíjivý — nepiš do něj nic, co uživatel musí vidět (chyby, podmínky). To patří do `Alert` nebo `AlertDialog`.
- Úspěch po `AlertDialog`/`Dialog` akci potvrď toastem; chybu akce ukaž `Alert` v místě nebo novým `AlertDialog` podle závažnosti.
