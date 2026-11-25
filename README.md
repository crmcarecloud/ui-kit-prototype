# CareCloud UI Prototypes

Plugin generuje jediný proklikávací `prototyp.html` nad přibaleným
CareCloud UI kitem, Reactem a mock daty. Soubor vyžaduje internet pro CDN.

## Instalace do ChatGPT workspace

Správce otevře **Admin → Plugins → Add → Import marketplace**.
Source: `https://github.com/crmcarecloud/ui-kit-prototype`, Path: prázdné,
Branch: `main`. Po importu zkontroluje výsledek a dostupnost pluginu;
uživatel jej nainstaluje z katalogu workspace a případně vybere přes @.
V monorepu použij URL příslušného repozitáře a Path `ui-kit-prototype`.
Změny musí být nejdříve publikované na zvolené větvi GitHubu.

Manifest `.agents/plugins/marketplace.json` odkazuje na plugin v kořeni.
Jde o skill bez MCP serveru a bez připojení k API. Instalace sama
nezaručuje spuštění HTML uvnitř chatu: výsledkem je soubor ke stažení.

Ukázka zadání: „Vytvoř správu zákazníků s vyhledáváním, filtrem segmentu,
dialogem přidání zákazníka a českými mock daty. Vrať prototyp.html.“
Otevři stažený soubor v prohlížeči s internetem. Ověř formulář,
záložky, dialogy, konzoli a mobilní šířku. Data se po obnovení zahodí.

## Obsah a údržba

- `plugin.json`: Agent Plugins 1.0 manifest a metadata pro ChatGPT.
- `.agents/plugins/marketplace.json`: vstup pro GitHub import.
- `skills/_prototypuj/`: instrukce, HTML bundle, katalog exportů a průvodce.

Toto je distribuční kopie. Neobsahuje npm projekt ani generátor.
Zdrojový projekt je `ui-carecloud-prototypes/ui-kit-prototype`;
správce v něm po změně UI kitu spustí `npm run sync-ui` a `npm run verify`.
Poté sem přenese aktualizované `skills/`, `plugin.json` a marketplace manifest,
zkontroluje diff a publikuje změny na GitHub. README ponechá distribuční.
Průvodce jsou ručně upravené pro samostatné HTML; nekopíruj sem nezměněnou
dokumentaci monorepa. Odkaz na sync skript v HTML je informace pro správce,
nikoli závislost potřebná k otevření souboru nebo použití skillu.
