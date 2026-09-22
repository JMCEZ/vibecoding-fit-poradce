# Fit Poradce

## Cíl projektu
Fit Poradce je konverzační nutriční asistent postavený na Claude Code. Uživatel popíše, co během dne snědl nebo vypil, a aplikace mu:

1. **odhadne, kolik kalorií tím zkonzumoval** (skill `odhad-kalorii`),
2. **na požádání doporučí jídelníček na zbytek dne** tak, aby udržel svůj denní kalorický cíl (skill `doporuceni-kalorii`).

## Klíčové pravidlo: vše počítá model
Odhad kalorií i sestavení jídelníčku **musí vždy odpracovat model** na základě svých znalostí výživy a zdravého úsudku — **nikdy** pevná vyhledávací tabulka, databáze potravin ani výpočetní skript. Cílem je, aby Claude uvažoval o porcích, ingrediencích a nutriční hodnotě stejně, jako by to dělal zkušený nutriční poradce, včetně transparentního přiznání předpokladů a nejistoty odhadu.

Jedinou výjimkou je `index.html` — statická vstupní/marketingová stránka pro reklamu, která s výpočetní logikou nijak nesouvisí.

## Výchozí profil modelového uživatele
Pokud uživatel v konverzaci neuvede jiné údaje, aplikace pracuje s tímto výchozím profilem:

- pohlaví: muž
- věk: 50 let
- výška: 190 cm
- hmotnost: 105 kg
- pohybová aktivita: sedavé zaměstnání (minimum pohybu přes den)

**Orientační výpočet denního kalorického cíle** (výchozí, pokud uživatel nezadá vlastní cíl):

1. Bazální metabolismus (BMR) podle Mifflin-St Jeor:
   `BMR = 10 × hmotnost(kg) + 6,25 × výška(cm) − 5 × věk(roky) + 5`
   → přibližně `10×105 + 6,25×190 − 5×50 + 5 ≈ 2223 kcal/den`
2. Denní energetický výdej (TDEE) = BMR × koeficient aktivity. Pro sedavé zaměstnání se používá koeficient cca **1,2**:
   → `TDEE ≈ 2670 kcal/den` (udržovací kalorický cíl)

Toto číslo slouží jako výchozí udržovací cíl. Pokud uživatel uvede jiný cíl (redukce, nabírání, konkrétní číslo, jiný profil), použije se vždy jeho zadání místo výpočtu výše.

## Jak se aplikace používá
1. Uživatel v konverzaci popíše snězené jídlo/nápoj (např. "k obědu jsem měl guláš s houskovým knedlíkem a colu 0,5l").
2. Zavolá se skill **`odhad-kalorii`**, který vrátí odhad kalorií po položkách i celkem.
3. Průběžně během dne se odhady sčítají (v rámci konverzace).
4. Když chce uživatel vědět, co má jíst zbytek dne, zavolá se skill **`doporuceni-kalorii`**, který na základě zbývajícího kalorického rozpočtu navrhne konkrétní jídelníček.

## Struktura projektu
- `CLAUDE.md` — tento soubor.
- `.claude/skills/odhad-kalorii/SKILL.md` — skill pro odhad kalorií z popisu jídla.
- `.claude/skills/doporuceni-kalorii/SKILL.md` — skill pro doporučení jídelníčku na zbytek dne.
- `index.html` — statická vstupní stránka pro reklamu, publikovaná jako GitHub Pages. Nesouvisí s výpočetní logikou výše.
