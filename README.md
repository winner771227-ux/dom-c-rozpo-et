# Měsíční přehled příjmů a výdajů

Statická webová aplikace (čisté HTML/CSS/JS, žádný build, žádný server), která replikuje
logiku tabulky `Měsíční_přehled_příjmů_a_výdajů_červen_2026.xlsx` — listy **Příjmy**,
**Rozpočet** a **Sledování cashflow**.

## Jak to nasadit na GitHub Pages

1. Vytvořte nový repozitář na GitHubu (může být soukromý, pokud chcete data jen pro sebe —
   GitHub Pages pak ale bude veřejné, pokud nemáte GitHub Pro/Team; zvažte to, protože jde
   o osobní finance).
2. Nahrajte do něj soubor `index.html` (přesně tento název, v rootu repozitáře).
3. V repozitáři: **Settings → Pages → Source** zvolte `Deploy from a branch`, branch `main`,
   složka `/ (root)`. Uložte.
4. Po cca minutě bude appka dostupná na `https://<vase-jmeno>.github.io/<nazev-repo>/`.

Žádné další kroky, žádné závislosti, žádný build proces.

## Jak appka funguje

- **Ruční vstup:** jediné pole, které se měsíčně přepisuje záměrně, je **C2** (fakturovaná
  částka bez DPH) — zvýrazněné zlatým rámečkem nahoře v sekci Příjmy.
- Navíc lze upravovat i konkrétní položky fixních plateb (Nájem, Výživné, Apple, …) a denní
  útraty v sekci Sledování cashflow — všechno ostatní (vzorce, vazby mezi listy) je pevně
  zakódované a přepočítává se živě.
- Všechny tři "listy" (Příjmy / Rozpočet / Sledování cashflow) jsou na jedné stránce pod sebou,
  propojené stejně jako v Excelu (named ranges, SUM, IF, atd.).

## Tlačítka v horní liště

- **+ Nový měsíc** — otevře dialog, kde zvolíte měsíc/rok. Vytvoří novou prázdnou šablonu
  (C2 a denní útraty vynulované, fixní platby zachované) a **stáhne ji jako .json soubor**.
  Volitelně appku hned přepne na nový měsíc.
- **Načíst měsíc (JSON)** — načte dříve staženou/zazálohovanou `.json` zálohu a obnoví v ní
  uložený stav (např. abyste pokračovali v rozpracovaném měsíci na jiném počítači).
- **Stáhnout zálohu (JSON)** — uloží aktuální stav (vše, co jste zadali) do `.json` souboru.
- **Obnovit šablonu** — vrátí aktuální měsíc na výchozí hodnoty (užitečné pro test/reset).

## Ukládání dat

Aplikace navíc automaticky ukládá aktuální stav do `localStorage` prohlížeče, takže při
zavření a znovuotevření karty na stejném počítači/prohlížeči se nic neztratí. `localStorage`
ale **není zálohou** (je vázaný na konkrétní prohlížeč a zařízení) — pro skutečné uložení mezi
měsíci nebo přenos mezi zařízeními používejte tlačítka Stáhnout/Načíst JSON.

## Struktura JSON souboru

```json
{
  "year": 2026,
  "month": 6,
  "c2": 44320,
  "fixedCosts": [50, 1700, 5000, ...],
  "cfPaidFlags": [true, true, false, ...],
  "dailySpend": [703, 423, null, ...],
  "rozpocetExtra": {"PC": 500, "PM": 1500, "MP": 5000, "z května": 3500},
  "savingsActual": [5000, 2000, 0, 500, 500, 1000, 500, 1000]
}
```

## Co dělat, když si chcete upravit i jiné neměnné buňky

Otevřete `index.html` v textovém editoru a najděte sekci `FIXED_COST_LABELS` /
`FIXED_COST_DEFAULTS` / `SAVINGS_PLAN` / `ROZPOCET_EXTRA_INCOME` ve `<script>` na konci souboru —
tam jsou všechny výchozí "neměnné" hodnoty pojmenované přesně jako v Excelu.
