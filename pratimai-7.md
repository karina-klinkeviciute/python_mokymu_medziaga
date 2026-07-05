# Praktinės užduotys – darbas su eilutėmis (string)

Šiose užduotyse naudokite tik tai, ką jau išmokote:

- indeksavimą;
- slicing;
- `len()`;
- `lower()`, `upper()`, `capitalize()`, `title()`;
- `strip()`;
- `replace()`;
- `split()`;
- `join()`;
- `startswith()`, `endswith()`;
- `find()`, `count()`;
- operatorių `in`.

---

# 1. Pirmoji ir paskutinė raidė

Duota:

```python
zodis = "programavimas"
```

Išveskite pirmą ir paskutinę raidę.

---

# 2. Sutrumpintas miesto pavadinimas

Duota:

```python
miestas = "Vilnius"
```

Išveskite pirmus tris simbolius.

Rezultatas:

```text
Vil
```

---

# 3. Atvirkščias tekstas

Duota:

```python
tekstas = "Python"
```

Išveskite tekstą atvirkščiai.

---

# 4. El. pašto naudotojas

Duota:

```python
el_pastas = "jonas@gmail.com"
```

Išveskite tik naudotojo vardą.

Rezultatas:

```text
jonas
```

---

# 5. Sutvarkykite vartotojo įvestį

Duota:

```python
vardas = "   jOnAs   "
```

Rezultatas:

```text
Jonas
```

---

# 6. HTTPS tikrinimas

Duota:

```python
url = "https://python.org"
```

Patikrinkite, ar naudojamas HTTPS protokolas.

---

# 7. PDF failas

Duota:

```python
failas = "ataskaita.pdf"
```

Patikrinkite, ar tai PDF failas.

---

# 8. Pakeiskite datą

Duota:

```python
data = "2026-07-05"
```

Pakeiskite visus `-` į `/`.

Rezultatas:

```text
2026/07/05
```

---

# 9. Vaisių sąrašas

Duota:

```python
vaisiai = "obuolys,kriaušė,slyva"
```

Paverskite eilutę į sąrašą.

---

# 10. Sukurkite sakinį

Duota:

```python
zodziai = ["Python", "yra", "smagus"]
```

Sujunkite į vieną sakinį.

Rezultatas:

```text
Python yra smagus
```

---

# 11. Kiek raidžių?

Duota:

```python
tekstas = "banana"
```

Suskaičiuokite, kiek kartų pasikartoja raidė `"a"`.

---

# 12. Ar yra @ simbolis?

Duota:

```python
el_pastas = "jonas@gmail.com"
```

Patikrinkite, ar tai panašu į el. pašto adresą.

---

# 13. Sukurkite vartotojo vardą

Duota:

```python
vardas = "Ada"
pavarde = "Lovelace"
```

Rezultatas:

```text
adalovelace
```

---

# 14. Failo plėtinys

Duota:

```python
failas = "nuotrauka.jpg"
```

Išveskite tik failo plėtinį.

---

# 15. Hashtag

Duota:

```python
fraze = "Dirbtinis intelektas"
```

Rezultatas:

```text
#dirbtinis_intelektas
```

---

# ⭐ 16. Registracijos forma

Duota:

```python
vardas = "   jOnAs   "
pavarde = " jOnAiTis "
el_pastas = " JONAS.JONAITIS@GMAIL.COM "
```

Paruoškite duomenis saugojimui.

Rezultatas:

```text
Jonas Jonaitis
jonas.jonaitis@gmail.com
Naudotojo vardas: jonas.jonaitis
```

Naudokite bent:

- `strip()`;
- `lower()`;
- `title()` arba `capitalize()`;
- `split()`;
- `join()`.

---

# ⭐ 17. Failų analizė

Duota:

```python
failai = [
    "ataskaita.pdf",
    "programa.py",
    "nuotrauka.jpg",
    "server.py",
    "duomenys.csv",
]
```

Išveskite:

- kiek yra Python failų;
- kiek yra PDF failų;
- visų Python failų pavadinimus **be plėtinio**.

---

# ⭐ 18. Serverio log'o analizė

Duota:

```python
log = "2026-07-05 14:22:18 ERROR User authentication failed"
```

Išveskite:

```text
Data: 2026-07-05
Laikas: 14:22:18
Lygis: ERROR
Pranešimas: User authentication failed
```

Papildomai nustatykite, ar tai klaidos pranešimas.

---

# ⭐ 19. CSV → sakinys

Duota:

```python
eilute = "jonas,jonaitis,25,kaunas"
```

Rezultatas:

```text
Jonas Jonaitis yra 25 metų ir gyvena Kaune.
```

Naudokite:

- `split()`;
- `capitalize()` arba `title()`;
- f-string.

---

# ⭐⭐ 20. Mini duomenų valymas

Duota:

```python
duomenys = """
Jonas, JONAITIS, Kaunas
  Ada , lovelace , London
Grace,HOPPER, New York
"""
```

Reikia:

- suskaidyti tekstą į eilutes;
- pašalinti nereikalingus tarpus;
- sutvarkyti raidžių dydį;
- išvesti rezultatą tokiu formatu:

```text
Jonas Jonaitis (Kaunas)
Ada Lovelace (London)
Grace Hopper (New York)
```

**Papildomas iššūkis:** surūšiuokite rezultatą pagal pavardę abėcėlės tvarka.
