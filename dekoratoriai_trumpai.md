# Dekoratoriai

Python programavimo kalba turi būdą užrašyti ir naudoti specialias funkcijas, rašomas virš kitų funkcijų ar klasių, ir keičiančias arba papildančias jų veikimą. 

Tokios funkcijos vadinamos dekoratoriais. 

Dekoratoriai Python'e turi specialų užrašymo būdą, prieš vardą naudojant simbolį `@`.

Dekoratorių pavyzdžiai:

```python
@property
def visi_balai():
    return self.balas1 + self.balas2 + self.balas3
```

```python
@class_method
def kiek_objektu(cls):
    return cls.objektu_kiekis
```

Dekoratorius savaime yra funkcija, kuri paima funkciją kaip argumentą, ją iškviečia ir grąžina rezultatą. Papildomai gali arba prieš kviečiant, arba po to, kažką padaryti papildomai - su argumentais, arba su rezultatu. 


Daugiau teorijos apie dekoratorius:

[Dekoratoriai - pirma dalis]
