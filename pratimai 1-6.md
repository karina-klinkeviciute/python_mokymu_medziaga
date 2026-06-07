# 🐍 Python pratimų rinkinys
### Pagal Karinos mokymų medžiagą – 1, 2, 3, 4, 5, 6 pamokos

---

## 1 pamoka: Kintamieji, tipai ir operacijos

### 🛒 Kavos kioskas
Espresso kainuoja 1.80 €, latte – 3.20 €, arbata – 2.00 €.  
Sukurk kintamuosius kiekvienos gėrimo kainai.  
Apskaičiuok, kiek kainuoja vienas espresso ir du latte.  
Apskaičiuok grąžą, jei mokėjai 10 €.

---

### 🏃 Bėgikas
Sukurk kintamuosius:
- bėgiko vardas (`str`)
- nubėgtas atstumas kilometrais (`float`)
- bėgimo laikas minutėmis (`int`)
- ar jis finišavo (`bool`)

Apskaičiuok vidutinį greitį km/min ir atspausdink visus duomenis su `print()`.

---

### 🎂 Gimtadienis
Šiuo metu yra 2026-ieji metai. Sukurk kintamąjį su savo gimimo metais.  
Apskaičiuok:
- kiek tau metų
- kiek dienų iš viso gyvenai (1 metai ≈ 365 dienos)
- kiek valandų gyvenai

---

### 🔢 Liekanos mįslė
Išbandyk šias operacijas Python konsolėje ir užsirašyk rezultatus:
```python
print(17 // 5)
print(17 % 5)
print(2 ** 8)
print(100 // 24)   # Kiek pilnų parų yra 100 valandų?
print(100 % 24)    # Kiek valandų lieka?
```
Pagalvok: kur gyvenime pasitaiko „dalyba be liekanos"?  
*(Užuomina: laikrodis, kalendorius, grąža)*

---

## 2 pamoka: Reikšmių įvedimas ir spausdinimas

### 👋 Asmeninis sveikinimas
Parašyk programą, kuri paklausia vartotojo vardo ir atspausdina:  
`Labas, [vardas]! Malonu susipažinti.`  
Naudok f-string formatą.

---

### 🧮 Paprastas skaičiuotuvas
Parašyk programą, kuri:
1. Paprašo įvesti du skaičius (paversk į `float`!)
2. Atspausdina jų sumą, skirtumą, sandaugą ir dalmenį

Pvz.:
```
Įveskite pirmą skaičių: 12
Įveskite antrą skaičių: 4
12.0 + 4.0 = 16.0
12.0 - 4.0 = 8.0
12.0 * 4.0 = 48.0
12.0 / 4.0 = 3.0
```

---

### 🌡️ Temperatūros keitiklis
Parašyk programą, kuri paprašo įvesti temperatūrą Celsijaus laipsniais  
ir atspausdina ją Farenheito laipsniais.  
Formulė: `F = C × 9/5 + 32`  
Naudok f-string: `f"{celsius}°C yra {fahrenheit}°F"`

---

### 🎓 Studento kortelė
Parašyk programą, kuri paklausia:
- studento vardą ir pavardę
- studijų programą
- kursą (skaičius!)

Tada atspausdina gražiai suformatuotą „kortelę":
```
=== STUDENTO KORTELĖ ===
Vardas: Jonas Jonaitis
Programa: Informatika
Kursas: 2
```

---

## 3 pamoka: Sąlyginiai sakiniai

### 🎟️ Kino bilieto kaina
Parašyk programą, kuri paklausia amžiaus ir atspausdina bilieto kainą:
- iki 12 metų → 4 €
- 12–17 metų → 6 €
- 18–64 metai → 9 €
- 65 ir daugiau → 6 €

---

### 🌦️ Ką rengtis?
Programa paprašo įvesti lauko temperatūrą (°C) ir pataria:
- virš 25° → „Dėk trumpus marškinėlius!"
- 15–25° → „Tiks lengvas džemperis."
- 5–14° → „Reikės striukės."
- žemiau 5° → „Apsirengk šiltai – kepurė ir šalikas būtini!"

---

### 🏆 Pažymių skaičiuoklė
Mokyklos sistema: pažymiai nuo 1 iki 10.  
Programa paklausia pažymio ir atspausdina:
- 9–10 → „Puikiai! 🌟"
- 7–8 → „Gerai 👍"
- 5–6 → „Patenkinamai"
- 1–4 → „Reikia daugiau pastangų"

Bonus: patikrink, ar įvestas skaičius yra 1–10 ribose, jei ne – atspausdink „Neteisingas pažymys".

---

### 🔐 Slaptažodžio tikrinimas
Programa turi „teisingą" slaptažodį (pvz., `"python123"`).  
Paklausk vartotojo, ką jis įveda.  
Jei teisingas → „Sveiki atvykę! ✅"  
Jei neteisingas → „Klaidingas slaptažodis. ❌"

---

## 4 pamoka: Duomenų struktūros – sąrašai

### 🛍️ Pirkinių sąrašas
Sukurk pirkinių sąrašą su 5 produktais.

- Atspausdink pirmą ir paskutinį elementą pagal indeksą
- Pridėk naują produktą su `.append()`
- Atspausdink tik pirmus 3 produktus (naudok slicing!)
- Patikrink su `in`, ar „duona" yra sąraše

---

### 🏆 Sporto rezultatai
Komandos žaidėjai surinko taškų: `[14, 8, 21, 5, 17, 11, 9]`

- Atspausdink rezultatus surikiuotus nuo mažiausio iki didžiausio (naudok `sorted()` – originalus sąrašas turi likti nepakitęs)
- Surikiuok patį sąrašą su `.sort()`
- Atspausdink rezultatus atvirkštine tvarka naudodamas slicing `[::-1]`
- Atspausdink tik pirmus 3 geriausius rezultatus

---

### 🎵 Grojaraštis
Sukurk mėgstamų dainų sąrašą (bent 6 dainos).

- Atspausdink 2-ą ir 4-ą dainą pagal indeksą
- Atspausdink paskutines 3 dainas naudojant slicing
- Apsuk grojaraštį (naudok `[::-1]`) ir atspausdink
- Patikrink, ar kuri nors konkreti daina yra sąraše

---

### 📊 Temperatūrų savaitė
Duotos savaitės temperatūros: `[12.5, 9.0, 14.2, 18.0, 16.5, 11.0, 8.3]`

- Atspausdink darbo dienų (pirmadienio–penktadienio) temperatūras naudojant slicing
- Atspausdink savaitgalio temperatūras
- Surikiuok temperatūras ir atspausdink – kokia buvo šilčiausia ir šalčiausia diena?
- Apskaičiuok savaitės vidurkį (naudok `sum()` ir `len()`)

---

## 5 pamoka: Žodynai (dictionaries)

### 📇 Kontaktas
Sukurk žodyną su draugo kontakto informacija:
- vardas, pavardė, telefonas, miestas, gimimo metai

Atspausdink kiekvieną lauką atskirai naudodamas raktus.  
Pridėk naują lauką – el. paštą.  
Pakeisk miestą – draugas persikėlė į Vilnių.

---

### 🍽️ Kavinės meniu
Sukurk žodyną, kuriame raktai yra patiekalų pavadinimai, o reikšmės – kainos:
```python
meniu = {"cepelinai": 8.50, "šaltibarščiai": 5.00, "kibinas": 3.20}
```
- Atspausdink visus patiekalus ir jų kainas naudodamas `.items()`
- Atspausdink tik patiekalų pavadinimus naudodamas `.keys()`
- Pridėk naują patiekalą
- Paklausk vartotojo, koks patiekalas jį domina, ir atspausdink jo kainą

---

### 📊 Klasės pažymiai
Sukurk žodyną, kuriame raktai – mokinių vardai, reikšmės – pažymiai:
```python
pazymiai = {"Ona": 9, "Tomas": 7, "Rasa": 10, "Mantas": 6}
```
Naudodamas `.values()`, apskaičiuok klasės vidurkį.  
Atspausdink visų mokinių vardus ir pažymius tvarkingai.

---

### 🌍 Sostinių žodynas
Sukurk žodyną su bent 5 šalimis ir jų sostinėmis:
```python
sostines = {"Lietuva": "Vilnius", "Prancūzija": "Paryžius", ...}
```
Parašyk programą, kuri paklausia šalies pavadinimo  
ir atspausdina jos sostinę.  
Jei tokios šalies žodyne nėra → „Tokios šalies nerandu."

---

## 6 pamoka: Ciklai (for, while)

### 🔢 Daugybos lentelė
Programa paklausia skaičiaus ir atspausdina jo daugybos lentelę nuo 1 iki 10.

Pvz. įvedus `7`:
```
7 x 1 = 7
7 x 2 = 14
...
7 x 10 = 70
```

---

### 💰 Taupomoji sąskaita
Jonas kasmet į banką deda 500 €. Bankas moka 3% metinių palūkanų.  
Parašyk programą su `for` ciklu, kuri atspausdina sąskaitos likutį  
kiekvieno iš 10 metų pabaigoje.

Pvz.:
```
Po 1 metų: 515.00 €
Po 2 metų: 530.45 €
...
```

---

### 🎲 Skaičių spėjimo žaidimas
Parašyk žaidimą su `while True` ciklu:
1. Programa žino slaptą skaičių (pvz., `slaptas = 42`)
2. Vartotojas spėja
3. Programa sako „Per mažai ⬆️", „Per daug ⬇️" arba „Atspėjai! 🎉"
4. Žaidimas baigiasi atspėjus – atspausdink, kiek bandymų prireikė

---

### 🛒 Elektroninė parduotuvė
Parašyk programą su `while True` ciklu – kasos aparatą:
1. Klausia prekės pavadinimo (arba „baigti" norint baigti)
2. Klausia prekės kainos
3. Prideda prie sąskaitos
4. Kai įveda „baigti" – atspausdina visų prekių sąrašą ir bendrą sumą

---

### 📈 FizzBuzz
Klasikinė programuotojų užduotis!  
Atspausdink skaičius nuo 1 iki 50, bet:
- Jei dalijasi iš 3 → spausdink `Fizz`
- Jei dalijasi iš 5 → spausdink `Buzz`
- Jei dalijasi iš 3 ir 5 → spausdink `FizzBuzz`
- Kitu atveju → spausdink patį skaičių

*(Patarimas: tikrink `FizzBuzz` sąlygą pirmiausiai!)*

---

## 🏆 Kombinuoti uždaviniai

### 📋 Mini adresų knygelė
Parašyk programą, kuri su `while True` ciklu leidžia:
- įvesti naują kontaktą (vardas + telefonas → saugoma žodyne)
- ieškoti kontakto pagal vardą
- baigti darbą

---

### 🎓 Klasės registro sistema
Parašyk programą:
1. `for` ciklu surenka 5 mokinių vardus ir pažymius (saugo žodyne)
2. Atspausdina visus mokinius ir pažymius
3. Apskaičiuoja ir atspausdina klasės vidurkį
4. Sąlyginiais sakiniais įvertina: ar klasės vidurkis geras (≥7), vidutinis (5–6), ar prastas (<5)

---

### 🏪 Parduotuvės sandėlis
Sukurk sąrašą su prekių pavadinimais ir atskirą sąrašą su jų kiekiais.  
Su `for` ciklu atspausdink visas prekes ir kiekius.  
Patikrink su `in`, ar konkreti prekė yra sandėlyje.  
Surikiuok prekes abėcėlės tvarka ir atspausdink.

---

*Sėkmės mokantis Python! 🚀*

(Sugeneruota naudojant Claude)
