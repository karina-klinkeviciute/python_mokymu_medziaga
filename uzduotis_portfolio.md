# Portfolio užduotis

Su Django padaryti savo darbų portfolio puslapį.

## Puslapio turinys

Portfolio puslapis galėtų atrodyti taip:

1. Bendra informacija. Šiai informacijai Django modelio kurti nebūtina (bet galima).

- Jūsų nuotrauka
- Vardas, pavardė
- Trumpas prisistatymas
- Jūsų, kaip profesionalo, logotipas
- Kontaktiniai duomenys
- Nuorodos į socialinius tinklus (LinkedIn, Github). Gali būti su socialinių tinklų logotipais (juos galite rasti pixabay, pexels ar panašiose svetainėse, paieškoje vesti “social media logos”, “github logo” ir panašiai). 
- Kita, jei dar ką nors papildomai sugalvosit.

Visa ši informacija gali būti sudėta tiesiai į pagrindinio puslapio template ir gražiai apipavidalinta.


2. Jūsų įgyvendintų projektų/užduočių aprašymai. Čia reikia Django modelio, kuriame būtų saugoma kiekvieno projekto informacija, ir taip pat admin, kad galima būtų juos įvesti. Atitinkamai, tada reikia views (ListView, DetailView) ir šablono bei url.

Tai būtų jūsų įgyvendintų projektų sąrašas. Galit įdėti ir šiame kurse atliktas Django užduotis. Kiekvienas projektas gali turėti šią informaciją:

- Kelios iliustracijos (pavyzdžiui, iki 3)
- Pavadinimas
- Aprašymas
- Naudotų technologijų sąrašas
- Nuoroda į projekto svetainę
- Nuoroda į projekto kodą github
- Kita, jei ką nors papildomo sugalvosit


## Vizualus išdėliojimas	

Projektus galit išdėlioti vienu iš 3 būdų: 

- Pagrindiniame puslapyje yra tik sąrašas su projektų pavadinimais, ir galbūt trumpa kita informacija (pavyzdžiui viena iliustracija ar panašiai). Kita informacija parodoma paspaudus ant nuorodos į išsamesnį projekto aprašymą
- Visa kiekvieno projekto informacija sudėta į sąrašą pirmame puslapyje. Kiekvienas projektas yra savo didesnėje atskiroje skiltyje.
- Iš pagrindinio puslapio veda nuoroda į projektų sąrašą. Toliau, projektų sąrašą galit daryti kaip a arba kaip b variante. 

Galima išdėlioti ir kitaip, jei sugalvosit kaip aiškiai, gražiai ir įdomiai tai padaryti.


## Pavyzdžiai:


https://www.koysor.me/ - tik be animacijų

https://www.edgardeiner.com/ - ta dalis, kur patys projektai (iki My skills). Tik paprasčiau, tik patį išdėliojimą pasižiūrėt. 

https://caferati.me/portfolio - čia tik iliustracijos, o paskui atsidarius galima pažiūrėti kiekvieną

https://www.kalebmckelvey.com/ 

https://chaseohlson.com/#work


## Informacija:

Django projekto kūrimas: https://github.com/karina-klinkeviciute/python_mokymu_medziaga/blob/main/django-projekto-kurimas.png 

Django templates sistema: https://github.com/robotautas/kursas/blob/master/Django/MDs4/django4.md

Django static files https://github.com/karina-klinkeviciute/python_mokymu_medziaga/blob/main/Django-static-files.md


