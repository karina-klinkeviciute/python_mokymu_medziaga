# Dekoratoriai

Python programavimo kalba turi būdą užrašyti ir naudoti specialias funkcijas, rašomas virš kitų funkcijų ar klasių, ir keičiančias arba papildančias jų veikimą. 

Tokios funkcijos vadinamos dekoratoriais. 

Dekoratoriai Python'e turi specialų užrašymo būdą, prieš vardą naudojant simbolį `@`.

Dekoratorių pavyzdžiai:

Dekoratorius `property`. 

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

Dekoratorius savaime yra funkcija, kuri paima funkciją kaip argumentą, ją iškviečia ir grąžina rezultatą. Taip pat dažniausiai arba prieš kviečiant, arba po to, kažką padaro papildomai - su argumentais, arba su rezultatu. 


Daugiau teorijos apie dekoratorius:

[Dekoratoriai - pirma dalis](https://github.com/robotautas/kursas/wiki/Dekoratoriai-I)

[Dekoratoriai - antra dalis](https://github.com/robotautas/kursas/wiki/Dekoratoriai-II)

