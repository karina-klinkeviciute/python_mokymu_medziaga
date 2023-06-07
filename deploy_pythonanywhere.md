# Projekto deployinimas PythonAnywhere

Jei norime, kad mūsų sukurtas puslapis būtų pasiekiamas per internetą, turim jį įkelti į serverį ir ten paleisti (angliškai "deploy").

PythonAnywhere yra vienas iš svetainių talpinimo paslaugų tiekėjų, orientuotas būtent į su Python sukurtas svetaines. 

Vėliau mokysimės, kaip įkelti į įvairius serverius, kur reikia daugiau konfigūravimo, bet šiuo metu pasirinksim vieną paprasčiausių būdų - įkelti į PythonAnywhere. PythonAnywhere turi nemokamus talpinimo planus, todėl jis patogus mums mokantis. 

https://www.pythonanywhere.com/


Į PythonAnywhere galima deployinti failą tiesiai iš GitHub git repozitorijos. Deployinimas iš git repozitorijos yra gana įprastas būdas - ir rankiniams deployinimams, ir automatiniams. Tam PythonAnywhere turi specialų įrankį, kurį mes ir naudosim.


## Projekto paruošimas

Norint deployinti projektą į PythonAnywhere, reikia pirmiausiai jį paruošti tam. 

### Migracijų failai

Pasitikrinkit, kad migracijų failai būtų irgi sukelti į GitHub. Į github nekeliam duomenų bazės - ji bus sukuriama serveryje iš naujo, nes serveryje norim turėti produkcijos, ne testavimo duomenis. Taip pat, realybėje daugelis duomenų gali būti slapti, 

### Saugumas

#### Secret Key ir slaptažodžiai

Kai kurie Django ar mūsų projekto nustatymai neturėtų būti keliami į GitHub. 

Vienas iš jų yra SECRET_KEY. SECRET_KEY naudojamas užšifruoti sesijų "cookies", tai pat užšifruojant 
slaptažodžius ar kitus duomenis. Todėl jis labai svarbus, ir jo negalima niekam atskleisti (pavyzdžiui, įkelti į GitHub), nes tada kiti gali užšifruoti jo duomenis ir taip apsimesti jūsų svetaine. 

Kartais būna kitų duomenų, kurie taip pat yra slapti ir jų negalima niekam atskleisti, kas reiškia - ir kelti į GitHub. Pavyzdžiai:

* Duomenų bazių prisijungimo duomenys
* Trečiųjų šalių programėlių ar servisų API slapti raktai
* Elektroninio pašto slaptažodžiai

Visi šie duomenys turi būti nekeliami į GitHub, o suvedami tiesiai serveryje.

##### `.env` failas

Vienas iš dažniausiai naudojamų būdų, dirbant su Django, o ir ne tik (su kitais framework'ais, kaip laravel - irgi) yra aplinkos kintamieji (Environment variables). 

Environment variables būna saugomi kompiuterio atmintyje. Jei dirbam su Django (ar kitu frameworku), ten juos sukeliam iš specialaus `.env` failo, naudodami specialias bibliotekas. 

Norint naudoti `.env` failą, reikia pasiruošti patį failą, ir taip pat pakeisti nustatymų failą taip, kad kai kuriuos nustatymus imtų iš `.env` failo. 

`.env` failas yra rašomas `bash` formatu. 

Pasiruoškime sau `.env` failą:

```
SECRET_KEY="m*epo+((q9t4h-g1zf9-nclc80-mthneqryca)0by@9=vgna6k"
DEBUG=True
```

**`.env` failas negali būti keliamas į GitHub, nes tokiu būdu atskleistume visus savo slaptažodžius. Todėl jį reikia įdėti į `.gitignore` failą.**


`.gitignore turinys`:

```
...
.env
.idea
venv
...
```

Tam, kad nepamirštume, kokius kintamuosius turim surašyti į `.env` failą, galim pasidaryti kitą failą, pavyzdžiui, `.env_example` ir į jį surašyti visus šuos kintamuosius su pavyzinėmis reikšmėmis:

`.env_example` failo pavyzdys:

```bash
SECRET_KEY=musu_slaptas_raktas
DEBUG=False
```

Šį `.env_example` failą jau galim dėti į git:

```git add .env_example```

```git commit -m "pridejom .env_example"```

```git push```

Žemiau bus parašyta, kokius veiksmus su šiais failais reikės atlikti serveryje.

##### `settings.py` failas

Nustatymų faile `settings.py` turi būti užkraunamos aukščiau minėtų kintamųjų reikšmės iš `.env` failo. 

Tam reikia naudoti biblioteką `dotenv`

Ją pirmiausiai reikia suinstaliuoti:

```pip install python-dotenv```

Suinstaliavę naują biblioteką, nepamirškime atnaujinti `requirements.txt`:

```pip freeze > requirements.txt```


`settings.py` failo pradžioje, prie kitų `import` sakinių, reikia pridėti ir tokį sakinį:

```python
from dotenv import load_dotenv
```

Po `import` sakinių, reikia įrašyti toki` eilutę: 

```python
load_dotenv()
```

Tada reikia surasti nustatymus, kuriuos norime padaryti slaptais įdedant į `.env` failą ir juos pakeisti taip:

```python
SECRET_KEY = os.getenv("SECRET_KEY")
DEBUG = os.getenv("DEBUG")
```
Jei yra kitų nustatymų, kurių taip pat nenorim, kad kiti pamatytų, 

#### Debug nustatymai

Kai dirbote su Django savo kompiuteriuose, matėt, kad padarius klaidą, ar įvedus netinkamą kelią, kartais Django svetainėje parodo daug informacijos, padedančios klaidas surasti ir ištaisyti. Tačiau serveryje mes nenorim rodyti šios informacijos, nes ji gali atskleisti įsilaužėliams daug informacijos apie mūsų projektą ir taip padaryti jį nesaugiu. 

Šios informacijos rodymas/nerodymas valdomas aplinkos kintamuoju `DEBUG`. Kai kintamasis `DEBUG=True`, tai visą šią informaciją apie klaidas mums rodys. Jei `DEBUG=False`,
ši klaidų informacija bus nerodoma, o jei klaidų paliksim, bus tiesiog rodomas puslapis su klaidos pranešimu (404, 500). 

Yra skirtingų būdų valdyti šį nustatymą, pavyzdžiui, skirtingi settings failai serveriui ir vietiniams kompiuteriams. 

Vienas iš būdų yra taip pat įkelti šį kintamąjį į aplinkos kintamuosius. 

`DEBUG` reikšmę aukščiau esančiame pavyzdyje įdėjom į `.env` failą. 

`settings.py` faile jį reikia pasiimti taip:

```python
DEBUG = True if os.getenv("DEBUG") == "True" else False
```

Sąlyginį sakinį naudojam todėl, kad eilutė "False", jei bandytume paversti tiesiogiai į Boolean, taptų `True` reikšme.

Padarę šiuos pakeitimus, vėl viską sukeliam į GitHub:

```git commit -m "pakeitėm nustatymus"```

```git push```

### Requirements failas

Tam, kad PythonAnywhere instaliavimo scriptas ne tik sukeltų mūsų projektą, bet ir suinstaliuotų visus reikalingus framework'us ir bibliotekas, reikia failo `requirements.txt`. 
Šis failas turi būti šakninėje git direktorijoje. 
Github svetainėje susiraskit, kokia jūsų projekto direktorija yra šakninė git direktorija (kartais gali būti šakninė pačio projekto direktorija, o kartais viena aukščiau). 

Šioje, šakninėje direktorijoje paleiskit šią komandą: `pip freeze > requirements.txt`

Bus sukurtas failas `requirements.txt`, jį reikia pridėti į git, komanda:
```git add requirements.txt```. 

Tada reikia padaryti git commitą: 
```git commit -m "pridėta requirements.txt"```


Toliau reikia įkelti šį pakeitimą, ir kitus, jei yra neįkeltų, į github: 
```git push``` 

(taip pat galit naudotis PyCharm įrankiais).



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

Konsolėje pirmiausiai reikia suinstaliuoti deployinimo įrankį, kuris padės mums sukelti failus iš GitHub ir paleisti serverį:

```
pip3.10 install --user pythonanywhere
```

## Deployinimas (kėlimas į serverį ir paleidimas)

Suinstaliavus, reikia paleisti šį įrankį, kad mūsų Django projekto kodą parsiųstų iš GitHub į serverį, ir įdiegtų mūsų projektą. Tam reikia paleisti žemiau esančią komandą, 
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
  
Kad galėtume prisijungti prie administravimo aplinkos ir ten kelti duomenis, reikia susikurti naudotoją:
  
```python manage.py createsuperuser```

Atsidariusiame dialoge reikia įrašyti naudotojo vardą ir kitus duomenis, kaip darėte ir savo kompiuteryje.

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

Jame šiuo metu bus šie kintamieji:

```bash
SECRET_KEY=musu_slaptas_raktas
DEBUG=False
```

Mums reikia sugeneruoti slaptą raktą, kuris būtų unikalus šiam PythonAnywhere serveriui. 

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

### git 

Jei atnaujinam svetainės kodą ir norim, kad atsinaujintų ir mūsų svetainė, reikia atnaujintą kodą įkelti į GitHub, o iš ten - pasiimti į serverį. 

Pakeitus kodą, savo kompiuteryje reikia atlikti šiuos veiksmus savo kompiuteryje:

`git add pakeistas_failas` arba `git add .` jei pakeitėm daugiau failų. Taip pridedami failai kėlimui į github.
`git commit -m "pakeitimai"` - užtvirtinam pakeitimus
`git push` - nusiunčiam į GitHub

Serveryje reikia atlikti šiuos veiksmus:

* Jei nesame pasileidę `Bash` konsolės, pasileidžiam ją. 
* nueinam į savo projekto direktoriją: `cd naudotojo_vardas.pythonanywhere.com`
* parsiunčiam atnaujinimus iš git: `git pull`

### svetainės perkrovimas

Parsiuntus atnaujinimus iš git, reikia perkrauti mūsų svetainę. Tam reikia nueiti į meniu punktą `Web`, ten vėl susirasti reload naudotojo_vardas.pythonanywhere.com" ir jį paspausti. 

Mūsų svetainė persikraus, ir nuėję į ją galėsime pamatyti ją su atnaujinimais. 

#### Credits:
Ši mokomoji medžiaga sukurta naudojantis šiais šaltiniais:

[Djanog Girls tutorial](https://tutorial.djangogirls.org/en/deploy/#setting-up-our-blog-on-pythonanywhere)

[How to generate Django Secret Key](https://codinggear.blog/django-generate-secret-key/)

