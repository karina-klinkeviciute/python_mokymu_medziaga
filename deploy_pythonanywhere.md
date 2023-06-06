# Projekto deployinimas PythonAnywhere

Jei norime, kad mūsų sukurtas puslapis būtų pasiekiamas per internetą, turim jį įkelti į serverį.

Vėliau mokysimės, kaip įkelti į įvairius serverius, kur reikia daugiau konfigūravimo, bet šiuo metu pasirinksim vieną paprasčiausių būdų - įkelti į PythonAnywhere. 

Į PythonAnywhere galima deployinti failą tiesiai iš GitHub git repozitorijos. Taip mes ir darysim. Tam PythonAnywhere turi specialų įrankį, apie kurį bus žemiau.



## Projekto paruošimas

Norint deployinti projektą į PythonAnywhere, reikia pirmiausiai jį paruošti tam. 

### Requirements failas

Tam, kad PythonAnywhere instaliavimo scriptas ne tik sukeltų mūsų projektą, bet ir suinstaliuotų visus reikalingus framework'us ir bibliotekas, reikia failo `requirements.txt`. 
Šis failas turi būti šakninėje git direktorijoje. 
Susiraskit, kokia jūsų projekto direktorija yra šakninė git direktorija (kartais gali būti šakninė pačio projekto direktorija, o kartais viena aukščiau). 

Šioje, šakninėje direktorijoje paleiskit šią komandą: `pip freeze > requirements.txt`

Bus sukurtas failas `requirements.txt`, jį reikia pridėti į git, su `git add requirements.txt`. Tada reikia padaryti git commitą (`git commit -m "pridėta requirements.txt"`). 
Toliau reikia įkelti šį pakeitimą, ir kitus, jei yra neįkeltų, į github: `git push` (taip pat galit naudotis PyCharm įrankiais).

### Saugumas

.env - nedėti į git. Įdėti į .gitignore (pasirašyti .env_example)
secret key
debug - false



Viskas, projektas paruoštas. 

## PythonAnywhere registracija

- nueikit į PythonAnywhere svetainę: https://www.pythonanywhere.com/
- Ten paspauskit "Plans and Pricing"
- Paspauskit "Create a Beginner account"
- Užsiregistruokit

## PythonAnywhere API Token

Tam, kad galėtume susikurti savo App ir jame deployinti kodą, reikia susikurti API token. 

Tam reikia eiti į Account -> API Token ir spausti "Create a new API token". Token'as bus sukurtas.

## PythonAnywhere deployinimo įrankio instaliavimas

PythonAnywhere svetainė mums leidžia priėjimą prie Linux konsolės. 

Viršutiniame meniu reikia paspausti "Consoles". Atsidariusiame lange yra išvardintos įvairios Python, Bash, duomenų bazių konsolės. Mums reikia pasirinkti `Bash` konsolę sąraše `Other`. 

Konsolėje pirmiausiai reikia suinstaliuoti deployinimo įrankį, kuris padės mums sukelti failus iš GitHUb ir paleisti serverį:

```
pip3.10 install --user pythonanywhere
```

## Deployinimas (kėlimas į serverį ir paleidimas)

Suinstaliavus, reikia paleisti šį įrankį, kad parsiųstų Django kodą iš GitHub į serverį ir įdiegtų mūsų projektą. Tam reikia paleisti žemiau esančią komandą, 
tik joje vietoje `<your-github-username>` ir `<your-repo-name>` reikia surašyti savo Github vartotojo vardą ir repozitorijos pavadinimą (arba tiesiog nukopijuoti visą nuorodą iš Github)

```
pa_autoconfigure_django.py --python=3.10 https://github.com/<your-github-username>/<your-repo-name>.git
```

Šis automatinis deployinimo scriptas atliks šiuos veiksmus:
  
  - parsiųs kodą iš github
  - sukurs ir aktyvuos virtualią aplinką PythonAnywhere serveryje
  - papildys settings failą savo papildomais nustatymais
  - sukurs duomenų bazę ir migruos jos struktūrą su `python manage.py migrate`
  - sutvarkys static failus (paleis collectstatic)
  - sukonfigūruos PythonAnywhere, kad jis būtų pasiruošęs rodyti jūsų puslapį

Dabar jau mūsų puslapis įkeltas į serverį ir jį galima pasiekti naršyklėje. Bet jame dar nėra nei naudotojų, nei duomenų.
  
  
## Pagrindinio naudotojo (super user) sukūrimas
  
Kad galėtume prisijungti prie administravimo aplinkos ir ten kelti duomenis, reikia susikurti vartotoją:
  
```python manage.py createsuperuser```

## Aplinkos kintamųjų surašymas
  
Prieš tai pasiruošėm nustatymus taip, kad slapti ir/ar nuo serverio priklausantys nustatymai turi būti surašyti į .env failą. Kadangi .env failo į git nekėlėm, PythonAnywhere reikia jį susikurti iš naujo. 

Tą taip pat padarysim konsolėje. 

Pasitikrinam, kokie failai ir direktorijos pas mus yra:

```ls```

Pirmiausia, persijungiam į projekto direktoriją:
```cd svetaines_pavadinimas.pythonanywhere.com```

Tada patikrinam, kokie joje yra failai:
```ls -al```

Parametras `-al` reikalingas tam, kad rodytų ir paslėptus failus (kad galėtume rasti .env ir .env_example).

Jei šioje direktorijoje yra `.env_example` failas, perkopijuojam jį į `.env` failą:

```cp .env_example .env```

Su teksto redaktoriumi `nano` atsidarom šį failą:

``` nano .env```

Pakeičiam aplinkos kintamųjų reikšmes į tikras, mums reikalingas serveryje:

```bash
SECRET_KEY=musu_slaptas_raktas
DEBUG=False
```

Slapto rakto sugeneravimui iš atsitiktinių simbolių, terminale (galima ir savo kompiuteryje, turi būti įjungta virtuali aplinka, į kurią instaliavom Django) turime paleisti šį kodą:

Pirmiausiai įjungiam python shell aplinką:

```python```

Tada importuojam bibliotekas ir sugeneruojam kodą:

```python
from django.core.management.utils import get_random_secret_key
print(get_random_secret_key())
```

Šis kodas atspausdins mums kažką panašaus į šitai:

```m*epo+((q9t4h-g1zf9-nclc80-mthneqryca)0by@9=vgna6k```

Šį simbolių rinkinį ir reikia įkopijuotį į .env failą, prie SECRET_KEY aplinkos kintamojo:

```
SECRET_KEY="m*epo+((q9t4h-g1zf9-nclc80-mthneqryca)0by@9=vgna6k"
DEBUG = False
```


## Programėlės informacija

Savo svetainės svetainės adresą ir jos informaciją rasim paspaudę meniu punktą "Web". Ten kairėje bus programėlių sąrašas, kuriame bus mūsų ši vienintelė programėlė (nemokamam plane galima turėti tik vieną). Paspaudę ant jos, rasim jos informaciją.
  
## Saugus jungimasis (SSL, HTTPS)

Norėdami, kad jungimasis prie mūtų tinklalapio visada būtų saugus (būtų visada nukreipiama į HTTPS protokolą, kuris užšifruoja siunčiamą informaciją), reikia įjungti tai mūsų programėlei žemiau. 

Paėję kiek žemiau, rasime skiltį "Security". Ten yra nustatymas "Force HTTPS" ir iš pradžių jis yra išjungtas (Disabled). Mums reikia paspausti pele ant žodžio "Disabled" tam, kad jį įjungtume. Dabar turėtų rodyti "Enabled".

Tai padarius, reikės grįžti į puslapio viršų ir paspausti "reload naudotojo_vardas.pythonanywhere.com". Tada mūsų svetainė persikraus ir šie nustatymai įsigalios.
  
## Svetainės atnaujinimas po pakeitimų



#### Credits:
Ši mokomoji medžiaga sukurta naudojantis šiais šaltiniais:

[Djanog Girls tutorial](https://tutorial.djangogirls.org/en/deploy/#setting-up-our-blog-on-pythonanywhere)

[How to generate Django Secret Key](https://codinggear.blog/django-generate-secret-key/)

