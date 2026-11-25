# Formuláře a vstupy

Stavba formulářů: anatomie pole, výběr správného vstupu, validace a odeslání.

## Anatomie pole

Každý formulářový prvek skládej přes `Field`:

```tsx
<Field>
  <FieldLabel htmlFor="predmet">Předmět e-mailu</FieldLabel>
  <Input id="predmet" placeholder="Slevový kód na listopad" />
  <FieldDescription>Zobrazí se v inboxu příjemce.</FieldDescription>
  <FieldError>Předmět je povinný.</FieldError>
</Field>
```

- `FieldGroup` seskupuje pole s mezerami, `FieldSet` + `FieldLegend` pojmenuje celou skupinu.
- `FieldLabel` je povinný u každého editovatelného pole — placeholder není label.
- `FieldDescription` pro nápovědu, `FieldError` pro chybu validace (zobrazuje se jen při chybě).
- Checkboxy/radio/switch mají `Field` s horizontální variantou — label vedle kontrolky, ne nad ní.

## Který vstup kdy

| Potřeba | Komponenta |
| --- | --- |
| Krátký text, číslo, e-mail | `Input` |
| Víceřádkový text | `Textarea` |
| Vstup s ikonou, prefixem nebo tlačítkem | `InputGroup` + `InputGroupAddon` |
| Kód / OTP | `InputOTP` |
| Ano/ne přepínač s okamžitým efektem | `Switch` |
| Souhlas / výběr voleb (odesílat formulářem) | `Checkbox` |
| Jedna volba z 2–5 viditelných možností | `RadioGroup` |
| Hodnota z rozsahu | `Slider` |
| Formátovací volby (tučné/kurzíva) | `Toggle` / `ToggleGroup` |

## Výběr z možností

| Situace | Komponenta |
| --- | --- |
| 2–5 voleb, mají být vidět | `RadioGroup` |
| Málo voleb, šetříš místo, stačí nativní ovládání | `NativeSelect` |
| Stylizovaný výběr, střední počet voleb | `Select` |
| Mnoho voleb nebo potřeba hledání | `Combobox` |
| Výběr entity z mock dat (kontakty, šablony) | `Dialog` s filtrovaným seznamem nad mock daty |

- `Select` vs `DropdownMenu`: `Select` vybírá hodnotu do formuláře, `DropdownMenu` spouští akce. Nezaměňuj.
- `Switch` vs `Checkbox`: switch mění nastavení okamžitě (autosave), checkbox je součást odesílaného formuláře.

## Datum a čas

- Samostatný kalendář: `Calendar`.
- Datum + čas: sestav ovládání z dostupných komponent nebo `Input` s odpovídajícím typem. Doménové pickery původní aplikace nejsou přibalené.

## Složené a sekundární prvky

- Hledání/filtr nad seznamem: `InputGroup` s `SearchIcon` v `InputGroupAddon`.
- Tlačítko přilepené na vstup (kopírovat, vygenerovat): `InputGroupButton`.
- Skupina souvisejících tlačítek (např. zarovnání): `ButtonGroup`.

## Validace a odeslání

- Chyby piš do `FieldError` u konkrétního pole; souhrnnou chybu formuláře dej do `Alert` nad formulář.
- Chybové stavy nesděluj toastem — toast je pro výsledek akce, ne pro validaci.
- Pětka formuláře: primární `Button` (submit) + `Button variant="ghost"` (zrušit). Primární popisek je sloveso („Uložit", „Odeslat"), ne „OK".
- Během odesílání tlačítko `disabled` + `Spinner` (`<Spinner data-icon="inline-start" /> Odesílám…`).
- Destruktivní submit potvrzuj přes `AlertDialog`, ne inline.
