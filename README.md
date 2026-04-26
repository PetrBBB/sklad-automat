# 🏪 Sklad & Automat

Jednoduchá webová aplikace pro evidenci zásob skladu a prodejního automatu.

## Funkce

- **Příjem zboží** — naskladnění dodávky do skladu
- **Přesun do automatu** — odečte ze skladu, přičte do automatu
- **Objednat** — seznam produktů pod minimální zásobou
- **Inventura** — fyzické přepočítání a oprava stavů
- **Historie** — záznamy všech pohybů s časovými razítky
- **Přidání produktu** — rozšíření seznamu o nové zboží

## Technologie

- Čistý HTML + JavaScript, žádné závislosti
- Data uložena v `localStorage` prohlížeče
- Funguje offline, lze přidat na plochu mobilu (PWA)

## Nasazení na GitHub Pages

1. Nahraj soubory do repozitáře
2. Jdi do **Settings → Pages**
3. Zvol branch `main`, složku `/ (root)` nebo `/docs`
4. Ulož — za chvíli bude appka dostupná na `https://tvoje-jmeno.github.io/nazev-repo/`

## Struktura

```
/
└── index.html    # celá aplikace v jednom souboru
└── README.md
```

## Changelog

### v1.0.0
- První verze
- 63 produktů přednastaveno
- Příjem, přesun do automatu, inventura, historie, objednávky
