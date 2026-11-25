---
name: _prototypuj
description: Vytvoř nebo uprav jediný proklikávací HTML prototyp nad CareCloud UI kitem. Výstupem je soubor prototyp.html s živým Reactem, komponentami packages/ui a mock daty bez API.
---

# /_prototypuj — HTML prototyp nad CareCloud UI kitem

Vytváříš **jeden soubor `prototyp.html`**, který uživatel otevře dvojklikem
(`file://`, potřebuje internet pro CDN). Uvnitř běží React přes Babel
standalone, skutečné komponenty UI kitu přes `window.CCUI` a Tailwind Play
CDN pro styly.

## Podklady v tomto skillu

- `assets/prototyp.html` — povinná kostra výstupu: import mapa (React z
  esm.sh), Babel, vložený browserový build UI kitu a téma. Začni VŽDY
  touto kopií, nikdy vlastním HTML od nuly.
- `assets/exports.json` — aktuální katalog exportů `window.CCUI`.
- `assets/guide/` — pravidla použití kitu (volba komponent, formuláře,
  data, překryvky, checklist). Pracuj podle nich; pokud soubor chybí,
  drž se vzorů v kostře.

## Postup

1. **Scénáře a obrazovky.** Ze zadání vytáhni obrazovky a hlavní
   klikací cesty. Ptej se jen na to, co skutečně blokuje práci.
2. **Zkopíruj `assets/prototyp.html`** a uprav POUZE označené oblasti:
   - `/* == MOCK DATA == */` — realistická česká data jako samostatná
     konstanta, nikdy volání API,
   - JSX aplikace v `<script type="text/babel" data-type="module">` —
     obrazovky, navigace, React state,
   - blok `/* == NOVÉ STYLY PROTOTYPU == */` v `<style type="text/
     tailwindcss">` — nové Tailwind utility/komponentové styly; komentář
     označení ponech.
   Sdílený runtime (inline `ui` modul, kompilované `kit.css`, blok tématu)
   **neměň** — generuje ho sync skript.
3. **Komponenty z `window.CCUI`** podle `assets/exports.json` a pravidel
   z `assets/guide/` (např. `Dialog` pro soustředěnou akci, `Sheet` pro
   vedlejší panel, `Field`+`Input` pro formuláře, `Tabs`, `Table`,
   `Badge`, `toast.add` + `<Toaster />`). API komponent neodhaduj.
4. **Všechno musí fungovat:** navigace přepíná, filtry filtrují,
   formuláře validují a zapisují do mock dat, dialogy se otevírají
   i zavírají (myš i klávesnice), tlačítka mají akce. Žádný prvek nesmí
   jen vypadat funkčně.
5. **Stavy:** přidej prázdný stav seznamu a aspoň jednu chybovou/edge
   situaci, pokud zadání dává smysl. Uživatelské texty i přístupné
   názvy česky.
6. **Výstup:** vrať soubor `prototyp.html` ke stažení + stručný seznam
   simulovaných chování. Nikdy netvrď, že jde o produkční API.

## Úprava existujícího prototypu

Při požadavku na změnu uprav dodaný `prototyp.html`; zachovej dosavadní
funkční scénáře, strukturu souboru a označené bloky. Měň jen to, co
zadání vyžaduje — nepřegenerovávej sdílený runtime.

## Kontrolní checklist před odevzdáním

- [ ] Každé tlačítko/odkaz/formulář něco dělá; žádná mrtvá interakce.
- [ ] Dialogy a menu fungují myší i klávesnicí (Escape, Tab, Enter).
- [ ] Prázdný stav seznamu a aspoň jeden chybový/edge stav.
- [ ] Nové styly jsou Tailwind utility nebo styly v označeném bloku
      `== NOVÉ STYLY PROTOTYPU ==`; žádné přepisování stylů kitu.
- [ ] Mock data oddělená, české texty, žádné `fetch`/API volání.
- [ ] Rozvržení drží i na mobilní šířce (~375 px).
- [ ] Soubor je kompletní a samostatný — otevře se dvojklikem.

## Omezení

- Soubor vyžaduje internet (CDN: esm.sh, jsdelivr, unpkg); offline není
  cílem.
- Změny mock dat se po obnovení stránky zahodí (není-li v zadání jinak).
