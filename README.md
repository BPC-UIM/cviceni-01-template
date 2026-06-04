# Cvičení 1: Explorativní analýza dat, předzpracování a metriky vzdálenosti

Vítejte na prvním cvičení! V tomto týdnu se posuneme od rychlého skriptování v Google Colab k profesionálnímu psaní kódu v čistém Pythonu (`.py`). Budeme pracovat objektově, využijeme typování (Type Hinting) a ukážeme si, jak psát kód, který je robustní a snadno testovatelný.

## 🎯 Cíle cvičení
1. **Osvojit si objektový přístup** a základy defenzivního programování (validace vstupů pomocí `assert`).
2. **Porozumět struktuře dat** pomocí základních statistických ukazatelů a vizualizace.
3. **Naučit se čistit data** od odlehlých hodnot (outliers) dvěma různými metodami.
4. **Zvládnout transformaci dat** (standardizaci a normalizaci), která je klíčová pro většinu algoritmů strojového učení.
5. **Pochopit geometrický význam vzdáleností** mezi objekty v mnohorozměrném prostoru.

---

## 📘 Stručné teoretické okénko

### 1. Detekce odlehlých hodnot (Outliers)
Odlehlé hodnoty mohou fatálně zkreslit výsledky analýzy nebo chování modelů. V tomto cvičení implementujete dva přístupy k jejich detekci:

* **Z-skóre:** Vyjadřuje, o kolik směrodatných odchylek ($\sigma$) se konkrétní hodnota liší od průměru ($\mu$). 
    $$Z = \frac{x - \mu}{\sigma}$$
    Standardně se za odlehlé považují hodnoty s $|Z| > 3$. Předpokládá, že data mají přibližně normální rozdělení.
* **IQR (Mezikvartilové rozpětí):** Robustní metoda založená na kvartilech ($IQR = Q_3 - Q_1$), která není citlivá na extrémní hodnoty. Odlehlé hodnoty leží mimo tzv. hradby:
    $$\text{Dolní mez} = Q_1 - \text{threshold} \times IQR$$
    $$\text{Horní mez} = Q_3 + \text{threshold} \times IQR$$
    (Typicky se používá `threshold = 1.5` pro odlehlé a `3.0` pro extrémní hodnoty).

### 2. Transformace dat (Scaling)
Algoritmy strojového učení často selhávají, pokud má jedna proměnná rozsah $0-1$ a druhá $0-100000$. Proto data transformujeme:

* **Z-score (Standardizace):** Transformuje data tak, aby měla průměr 0 a směrodatnou odchylku 1.
* **Min-Max (Normalizace):** Stlačí veškeré hodnoty pevně do rozsahu $[0, 1]$. Je velmi citlivá na odlehlé hodnoty.
* **Procentuální normalizace:** Přepočítá hodnoty jako podíl na celkové sumě daného sloupce ($x / \sum x$).

### 3. Metriky vzdálenosti
Představte si dva pacienty jako dva body v mnohorozměrném prostoru příznaků. Jak moc jsou si podobní? To nám říká vzdálenost (čím menší vzdálenost, tím podobnější objekty):

* **Euklidovská vzdálenost:** Geometrická vzdálenost "vzdušnou čarou" (přepona trojúhelníku).
* **Manhattanská vzdálenost:** Vzdálenost měřená pouze pravoúhlými pohyby (jako chůze v blocích Manhattanu).
* **Cosinová vzdálenost:** Neměří délku, ale úhel mezi dvěma vektory. Používá se tam, kde nás zajímá spíše trend/poměr hodnot než jejich absolutní velikost.

---

## 💻 Pokyny k vypracování

Vaším úkolem je otevřít soubor `cviceni_01.py` a postupně nahradit všechna klíčová slova `raise NotImplementedError` a komentáře `# TODO` funkčním kódem.

### Krok 1: Načtení a validace dat
* Implementujte funkci `load_data()`.
* Ve třídě `BasicStatistics.__init__` doplňte podmínky do příkazů `assert`. Zkontrolujte, zda jsou data typu `np.ndarray`, zda jsou dvourozměrná (tabulka) a zda jsou číselná.

### Krok 2: Statistika a Vizualizace
* Implementujte výpočty statistik a unifikované výpisy. Text zarovnávejte ideálně pomocí f-strings (např. `| {promenna:<15} | ...`).
* Vygenerované histogramy ukládejte do složky `graphs`. Pokud složka neexistuje, váš kód by si ji měl ideálně sám vytvořit (pomocí modulu `os` nebo `pathlib`).

### Krok 3: Scaler a Distanční matice
* Dokončete třídu `Scaler` pro transformaci matic.
* Implementujte jednotlivé dceřiné třídy pro vzdálenosti a doplňte funkci `create_distance_matrix`. Výsledná matice musí být čtvercová a symetrická s nulami na hlavní diagonále.

---

## 🛠️ Jak kód lokálně testovat?

Přímo v souboru na konci najdete blok `if __name__ == "__main__":`. Ten slouží pro vás. Můžete si do něj dopisovat libovolné `print()` příkazy a spouštět skript z terminálu:

```bash
python cviceni_01.py
```
Pokud kód neskončí chybou NotImplementedError a vypíše požadované textové výstupy, máte vyhráno.

## 🚀 Odevzdání úlohy

Úloha se odevzdává automaticky pomocí systému GitHub Classroom.

Jakmile provedete git commit a push svých změn do vašeho repozitáře, na pozadí se spustí automatické testy, které ověří správnost vašich výpočtů. Výsledek (zelená fajfka / červený křížek) uvidíte přímo v rozhraní GitHubu u vašeho repozitáře.
