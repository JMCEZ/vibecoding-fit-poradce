---
name: odhad-kalorii
description: Odhadni kalorickou hodnotu snězeného jídla nebo vypitého nápoje z volného textového popisu od uživatele (např. "k obědu jsem měl guláš s knedlíkem a colu"). Spusť se vždy, když uživatel popisuje, co snědl nebo vypil, a chce vědět, kolik to mělo kalorií.
---

# Odhad kalorií

## Zadání
Uživatel v přirozeném jazyce (typicky česky, hovorově) popíše jednu nebo více snězených/vypitých položek. Tvým úkolem je z tohoto popisu odhadnout kalorickou hodnotu, a to **vlastním úsudkem na základě znalostí o výživě** — ne dohledáváním v pevné tabulce ani spouštěním výpočetního skriptu.

## Postup

1. **Rozpoznej položky**: rozděl popis na jednotlivé potraviny/nápoje (např. "guláš", "houskový knedlík", "cola 0,5l").

2. **Odhadni porci u každé položky**:
   - Pokud uživatel uvedl množství (g, ml, kusy, "malý/velký talíř" apod.), vycházej z něj.
   - Pokud množství neuvedl, použij typickou/obvyklou porci pro danou položku (např. běžná porce guláše v restauraci ~350 g) a **tento předpoklad výslovně uveď ve výstupu**.
   - Při výrazně nejednoznačném popisu (např. jen "měl jsem oběd" bez upřesnění) se raději nejdřív doptej na konkrétní jídlo, než abys hádal naslepo.

3. **Odhadni kalorie u každé položky**: použij své znalosti o typickém složení a energetické hodnotě daného jídla/nápoje (suroviny, způsob přípravy — smažené vs. vařené, tučnost apod.). Uveď rozumné zaokrouhlené číslo (např. "~450 kcal"), ne falešně přesná čísla na jednotky kcal.

4. **Volitelně makroživiny**: pokud to má pro uživatele smysl nebo o to požádá, doplň orientační rozpad na bílkoviny / tuky / sacharidy.

5. **Sečti celkem**: uveď součet kalorií za všechny položky z aktuálního popisu.

## Výstupní formát

Stručná tabulka nebo odrážkový seznam:

```
| Položka                  | Odhadovaná porce | Kalorie   |
|---------------------------|------------------|-----------|
| Guláš                     | ~350 g           | ~450 kcal |
| Houskový knedlík (3 plátky)| ~150 g          | ~350 kcal |
| Cola                       | 0,5 l           | ~210 kcal |
| **Celkem**                 |                  | **~1010 kcal** |
```

Pod tabulku vždy krátce napiš, jaké předpoklady o porcích jsi udělal(a), aby uživatel mohl číslo opravit, pokud snědl jinak.

## Poznámky
- Buď transparentní ohledně nejistoty odhadu (je to odhad, ne laboratorní analýza).
- Nepoužívej žádnou pevnou vyhledávací databázi kalorických hodnot ani externí skript/nástroj pro výpočet — odhad vždy prováděj vlastním uvažováním.
- Pokud konverzace obsahuje předchozí odhady z dnešního dne, na požádání je dokážeš sečíst do denního součtu (užitečné jako vstup pro skill `doporuceni-kalorii`).
