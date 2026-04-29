# 🏪 Sklad & Automat

Jednoduchá webová aplikace pro evidenci zásob skladu a prodejního automatu.
Funguje na všech zařízeních, data sdílená online přes Supabase.

---

## Jak to funguje (přehled)

```
Telefon / Tablet / PC
        ↓
   index.html          ← aplikace (hostovaná na GitHub Pages)
        ↓
    Supabase           ← online databáze (zdarma), ukládá všechna data
```

- Appka je hostovaná na **GitHub Pages** (tento repozitář)
- Data (produkty, zásoby, historie) jsou uložena v **Supabase** databázi
- Funguje na všech zařízeních najednou — změna na jednom se projeví všude
- Na iPhone přidej přes Safari → Sdílet → Přidat na plochu

---

## Funkce aplikace

- **Příjem zboží** — naskladnění dodávky do skladu
- **Přesun do automatu** — odečte ze skladu, přičte do automatu
- **Objednat** — seznam produktů pod minimální zásobou
- **Inventura** — fyzické přepočítání a oprava stavů
- **Historie** — záznamy všech pohybů s časovými razítky
- **Přidání produktu** — rozšíření seznamu o nové zboží

---

## Technické informace

### GitHub Pages
- Repozitář: `https://github.com/[tvůj-účet]/[název-repozitáře]`
- Appka URL: `https://[tvůj-účet].github.io/[název-repozitáře]/sklad/`
- Soubor aplikace: `sklad/index.html`

### Supabase (databáze)
- Dashboard: `https://supabase.com/dashboard`
- Project URL: *(doplnit po vytvoření projektu)*
- Tabulky v databázi:
  - `products` — seznam produktů se zásobami
  - `history` — záznamy všech pohybů

> ⚠️ API klíč nikdy nedávej do veřejného repozitáře jako samostatný soubor.
> Je součástí `index.html` jako anon/public klíč — to je bezpečné pro čtení/zápis
> přes Row Level Security (RLS).

---

## Nasazení — postup

### 1. GitHub Pages
1. Nahraj soubory do repozitáře
2. Jdi do **Settings → Pages**
3. Zvol branch `main`, složku `/ (root)`
4. Ulož — appka běží na `https://tvůj-účet.github.io/název-repo/sklad/`

### 2. Supabase
1. Vytvoř účet na [supabase.com](https://supabase.com)
2. Vytvoř nový projekt (heslo si zapiš!)
3. V **Settings → API** najdeš:
   - `Project URL` — adresa databáze
   - `anon public` klíč — pro přístup z appky
4. Spusť SQL skript pro vytvoření tabulek (viz níže)
5. Vlož URL a klíč do `index.html`

### SQL pro vytvoření tabulek (spustit v Supabase → SQL Editor)
```sql
-- Produkty
create table products (
  id bigint primary key generated always as identity,
  name text not null,
  cat text not null default 'Ostatní',
  sklad integer not null default 0,
  automat integer not null default 0,
  min integer not null default 5,
  created_at timestamptz default now()
);

-- Historie pohybů
create table history (
  id bigint primary key generated always as identity,
  product_id bigint references products(id),
  product_name text not null,
  type text not null,
  qty integer,
  note text,
  diff_sklad integer,
  diff_automat integer,
  created_at timestamptz default now()
);

-- Přístup (Row Level Security)
alter table products enable row level security;
alter table history enable row level security;
create policy "public access" on products for all using (true) with check (true);
create policy "public access" on history for all using (true) with check (true);
```

---

## Struktura repozitáře

```
/
└── sklad/
    ├── index.html    ← celá aplikace
    └── README.md     ← tento soubor
```

---

## Changelog

### v1.0.0
- První verze
- 63 produktů přednastaveno
- Příjem, přesun do automatu, inventura, historie, objednávky
- Lokální úložiště (localStorage)

### v1.1.0
- Přechod na Supabase — sdílená data napříč zařízeními

### v1.2.0
- Filtrování produktů podle kategorie (chips nad seznamem)
- Výběr kategorie při přidání produktu (dropdown místo textového pole)
- Záložka Nastavení — správa kategorií (přidání, smazání)

### v1.3.0
- PIN ochrana při otevření appky (4 číslice)
- PIN se pamatuje do zavření záložky (sessionStorage)
- Odhlášení v záložce Nastavení

### v1.4.0
- Odstraněno pole minimální zásoba (nepotřebné)
- Záložka Objednat: stavy Neobjednáno / Objednáno
- Příjem zboží automaticky resetuje stav objednávky

### v1.5.0
- Pole počtu kusů je prázdné při otevření (není třeba mazat 1)
- Vyřazení produktu — skryje ho z celé aplikace
- Vyřazené produkty lze obnovit v záložce Nastavení

### v1.6.0
- Sledování expirace — dávky s datem spotřeby
- Nová záložka Expirace — přehled dávek dle blízkosti expirace
- Barevné označení: červená (expirováno/30 dní), oranžová (60 dní), zelená (ok)
- Přidání dávky ze záložky Expirace i přímo z karty produktu
- Smazání dávky křížkem

### v1.7.0
- Kategorie přesunuty do Supabase — sdílené napříč zařízeními
- Přidání a mazání kategorií se projeví na všech zařízeních okamžitě
