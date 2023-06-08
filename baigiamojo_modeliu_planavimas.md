# Baigiamojo darbo pradžia 

Kadangi baigiamasis darbas turi būti jūsų įgytų žinių ir gebėjimų demonstravimas, ši instrukcija specialiai bus mažai detali.

## Projekto pradžia

### Git repozitorijos sukūrimas

Jei dar neturim git repozitorijos savo projektui, reikia ją susikurti vienu iš mūsų išmoktų būdų.
[Git tema kurso medžiagoje](https://github.com/DonatasNoreika/python1lygis/wiki/Versij%C5%B3-valdymo-sistemos-(GIT))

## Django projekto sukūrimas

Sukuriam Django projektą:
https://github.com/robotautas/kursas/blob/master/Django/MDs1/django1.md




## Modeliai

Dirbant su Django, geriausia prad4ti nuo modelių, kurie aprašo mūsų nuodojamus duomenis ir taip duoda mūsų projektui "griaučius".

### Modelių pavyzdys pagal Įrankių nuomos projekto aprašymą:

[Įrankių nuomos projektas](https://github.com/karina-klinkeviciute/irankiu_nuoma)

Sukuriam appsą "Įrankis"

Jame atsiras "models.py" failas

Šiame faile mums reikia kurti savo modelius. Pateiksiu pavyzdį, kaip pagal turimą aprašymą sukurti modelį:

Aprašymas:

- įrankis
    - pavadinimas
    - aprašymas
    - nuotrauka
    - naudotojas
    - kategorija 
    - galia
    - ar su pristatymu, ar reikia pasiimt


Modelis:

```python
class Irankis(models.Model):
    pavadinimas = models.CharField(max_length=255)
    aprasymas = models.TextField(blank=True, null=True)
    galia = models.IntegerField(blank=True, null=True)
    pristatymas = models.BooleanField()
    kategorijos = models.ManyToManyField(Kategorija)
    naudotojas = models.ForeignKey(NaudotojoProfilis, on_delete=models.CASCADE)
    nuotrauka = models.ImageField(upload_to="irankis", null=True, blank=True)
    
``` 
[Modelių kūrimo aprašymas](https://github.com/robotautas/kursas/blob/master/Django/MDs2/django2.md)

## Administravimo aplinka

Po to, kaip susikuriam modelius, užregistruojam juos [administravimo aplinkoje](https://github.com/robotautas/kursas/blob/master/Django/MDs3/django3.md)

Taip pat susikuriam pagrindinį vartotoją (superuser)

## Views, urls ir templates
## Registration, forms
