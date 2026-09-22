---
name: doporuceni-kalorii
description: Doporuč jídelníček na zbytek dne tak, aby uživatel udržel svůj denní kalorický cíl. Spusť se, když se uživatel ptá, co má ještě dnes jíst, kolik kalorií mu zbývá, nebo chce doporučení jídelníčku na zbytek dne.
---

# Doporučení jídelníčku na zbytek dne

## Zadání
Na základě denního kalorického cíle uživatele a toho, co už dnes snědl, navrhni **vlastním úsudkem** (na základě znalostí o výživě) konkrétní, realistický jídelníček na zbytek dne — ne generický seznam bez čísel a ne výpočet z pevné databáze.

## Postup

1. **Zjisti denní kalorický cíl**:
   - Pokud ho uživatel v konverzaci zadal (číslo nebo jasný cíl typu "hubnutí o X kg/týden"), použij ho.
   - Jinak vypočti orientační udržovací cíl z výchozího profilu v [CLAUDE.md](../../../CLAUDE.md) (muž, 50 let, 190 cm, 105 kg, sedavé zaměstnání) pomocí Mifflin-St Jeor BMR × koeficient sedavé aktivity (~1,2) — viz vzorec a výsledné číslo přímo v `CLAUDE.md`.

2. **Zjisti, kolik už bylo dnes snědeno**:
   - Podívej se do konverzace, jestli tam jsou dřívější odhady ze skillu `odhad-kalorii`, a sečti je.
   - Pokud v konverzaci nic není, zeptej se uživatele, co dnes už jedl (případně to nech odhadnout skillem `odhad-kalorii`).

3. **Spočítej zbývající rozpočet**: `zbývající kalorie = denní cíl − již zkonzumované kalorie`. Pokud je zbývající rozpočet už teď nízký nebo záporný, na to uživatele upozorni a přizpůsob doporučení (lehčí jídla, případně doporuč cíl na daný den nepřekračovat výrazně, ne drastické hladovění).

4. **Navrhni konkrétní jídelníček na zbytek dne**:
   - Zohledni aktuální denní dobu (dopoledne → oběd + odpolední svačina + večeře; večer → jen lehčí večeře apod.).
   - Navrhni konkrétní jídla (ne jen "zeleninu a bílkoviny"), s odhadem porce a kalorií u každého, tak aby součet seděl do zbývajícího rozpočtu.
   - Dbej na vyváženost (dostatek bílkovin a vlákniny pro sytost, rozumný poměr tuků/sacharidů) vhodnou pro sedavého 50letého muže.
   - Pokud je zbývající rozpočet hodně nízký, řekni to narovinu a navrhni spíš lehčí variantu než jídelníček, který cíl stejně překročí.

5. **Sečti navržený jídelníček** a ukaž, že se vejde do zbývajícího rozpočtu (případně s malou rezervou).

## Výstupní formát

```
Denní cíl: ~2670 kcal
Již zkonzumováno: ~1010 kcal
Zbývá: ~1660 kcal

Návrh na zbytek dne:
| Jídlo                          | Odhadovaná porce | Kalorie   |
|---------------------------------|------------------|-----------|
| Odpolední svačina (jogurt + ořechy) | ...          | ~250 kcal |
| Večeře (kuřecí prsa, rýže, zelenina)| ...          | ~650 kcal |
| **Celkem navrženo**              |                  | **~900 kcal** |

Zbývající rezerva: ~760 kcal (prostor na drobnou svačinu nebo nápoj navíc)
```

## Poznámky
- Nepoužívej pevnou databázi jídel ani výpočetní skript — jídelníček i kalorie u návrhů odhaduje model.
- Vždy uveď, z jakého denního cíle a z jaké dosavadní spotřeby vycházíš, ať je doporučení pro uživatele srozumitelné a ověřitelné.
- Pokud uživatel nemá žádné dietní omezení uvedené, navrhuj běžně dostupná, jednoduše připravitelná jídla.
