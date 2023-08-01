# Projekto deployinimas 

Jei norime, kad mūsų sukurtas puslapis būtų pasiekiamas per internetą, turim jį įkelti į serverį, ten sukonfigūruoti ir paleisti (angliškai "deploy", lietuviško termino nežinau, tai čia naudosiu anglišką).

## Projekto paruošimas

Norint deployinti projektą į serverį reikia pirmiausiai jį paruošti tam. 

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

##### `.env_example`

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

Nustatymų faile `settings.py` aukščiau minėtų kintamųjų reikšmės turi būti užkraunamos iš `.env` failo. 

Tam reikia naudoti biblioteką `dotenv`

Ją pirmiausiai reikia suinstaliuoti:

```pip install python-dotenv```

Suinstaliavę naują biblioteką, nepamirškime atnaujinti `requirements.txt`:

```pip freeze > requirements.txt```


`settings.py` failo pradžioje, prie kitų `import` sakinių, reikia pridėti ir tokį sakinį:

```python
from dotenv import load_dotenv
```

Po `import` sakinių, reikia įrašyti tokią eilutę: 

```python
load_dotenv()
```

Tada reikia surasti nustatymus, kuriuos norime padaryti slaptais įdedant į `.env` failą ir juos pakeisti taip:

```python
SECRET_KEY = os.getenv("SECRET_KEY")
DEBUG = True if os.getenv("DEBUG") == "True" else False
```

Sąlyginį sakinį naudojam todėl, kad eilutė "False", jei bandytume paversti tiesiogiai į Boolean, taptų `True` reikšme.

Jei yra kitų nustatymų, kurių taip pat nenorim, kad kiti pamatytų, juos irgi reikia surašyti į `.env` failą.

#### Debug nustatymų paaiškinimas

Kai dirbote su Django savo kompiuteriuose, matėt, kad padarius klaidą, ar įvedus netinkamą kelią, kartais Django svetainėje parodo daug informacijos, padedančios klaidas surasti ir ištaisyti. Tačiau serveryje mes nenorim rodyti šios informacijos, nes ji gali atskleisti įsilaužėliams daug informacijos apie mūsų projektą ir taip padaryti jį nesaugiu. 

Šios informacijos rodymas/nerodymas valdomas aplinkos kintamuoju `DEBUG`. Kai kintamasis `DEBUG=True`, tai visą šią informaciją apie klaidas mums rodys. Jei `DEBUG=False`,
ši klaidų informacija bus nerodoma, o jei klaidų paliksim, bus tiesiog rodomas puslapis su klaidos pranešimu (404, 500). 

Yra skirtingų būdų valdyti šį nustatymą, pavyzdžiui, skirtingi settings failai serveriui ir vietiniams kompiuteriams. 

Vienas iš būdų yra taip pat įkelti šį kintamąjį į aplinkos kintamuosius, kaip ir padarėm viršuje. 


### Requirements failas

Tam, kad serveryje galėtume suinstaliuoti visus reikalingus framework'us ir bibliotekas, reikia failo `requirements.txt`. 
Savo projekte paleiskit šią komandą: `pip freeze > requirements.txt`

Bus sukurtas failas `requirements.txt`, jį reikia pridėti į git, komanda:
```git add requirements.txt```. 

Tada reikia padaryti git commitą: 
```git commit -m "pridėta requirements.txt"```


Toliau reikia įkelti šį pakeitimą, ir kitus, jei yra neįkeltų, į github: 
```git push``` 

(taip pat galima naudotis PyCharm įrankiais).



Viskas, projektas paruoštas. 


## Serverio pusė 
  
Prisijunkite prie serverio su Linux operacine sistema. 

Prisijungsite tikriausiai su `root` naudotoju, bet kelti puslapio su šiuo naudotoju nėra saugu. Todėl pirmiausiai reikia susikurti naują naudotoją. 

### Naujo naudotojo sukūrimas

Naudotojo sukūrimo komanda: 

`sudo adduser karina`

Paleidus šią komandą, paprašys sukurti slaptažodį ir, jei norit, įvesti naudotojo duomenis (galima palikti tuščius)

Prisijunkim šiuo naudotoju:

`sudo su karina`

### Projekto įkėlimas

Projekto geriausiai nekelti į šakninę direktoriją, o jam sukurti naują direktoriją. Vienas iš variantų yra sukurti naują direktoriją, skirtą projektams, `/usr/local/` direktorijoje:

`cd /usr/local`

`mkdir projects`

Įeinam į sukurtą direktoriją: 

`cd projects`


Šioje direktorijoje `git` pagalba įkelsim savo projektą:

`git clone https://github.com/karina-klinkeviciute/CodeAcademyDjango.git`

Projektas bus sukeltas iš github į serverį. 

### Aplinkos kintamųjų surašymas
  
Prieš tai pasiruošėm nustatymus taip, kad slapti ir/ar nuo serverio priklausantys nustatymai turi būti surašyti į .env failą. Kadangi .env failo į git nekėlėm, serveryje reikia jį susikurti iš naujo. 

Tą taip pat padarysim konsolėje. 

Pasitikrinam, kokie failai ir direktorijos pas mus yra:

```ls```

Pirmiausia, persijungiam į projekto direktoriją:
```cd CodeAcademyDjango``` (vietoj "CodeAcademyDjango" - jūsų projekto pavadinimas)

Tada patikrinam, kokie joje yra failai:
```ls -al```

Parametras `-al` reikalingas tam, kad rodytų ir paslėptus failus (kad galėtume rasti .env ir .env_example).

Jei šioje direktorijoje yra `.env_example` failas, perkopijuojam jį į `.env` failą:

```cp .env_example .env```

Su teksto redaktoriumi `nano` (galima naudoti ir vim, jei jums patogu) atsidarom šį failą:

``` nano .env```

Jame šiuo metu bus šie kintamieji:

```bash
SECRET_KEY=musu_slaptas_raktas
DEBUG=False
```

Mums reikia sugeneruoti slaptą raktą, kuris būtų unikalus šiam serveriui. 

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

### Virtualios aplinkos sukūrimas

Kaip ir savo kompiuteryje, taip ir serveryje, projektą vykdyti rekomenduojama virtualioje aplinkoje (venv)

Tam paleidžiam komandą: 

`python -m venv venv`

Tada šią virtualią aplinką aktyvuojam:

`source venv/bin/activate`

### Django ir kitų bibliotekų instaliavimas

requirements.txt

  
### Pagrindinio naudotojo (super user) sukūrimas
  
Kad galėtume prisijungti prie administravimo aplinkos ir ten kelti duomenis, reikia susikurti naudotoją:
  
```python manage.py createsuperuser```

Atsidariusiame dialoge reikia įrašyti naudotojo vardą ir kitus duomenis, kaip darėte ir savo kompiuteryje.

### gunicorn ir nginx diegimas

gunicorn yra Web serverio programinė įranga, skirta Python. Ji naudojama "production" serveriuose. Mūsų kompiuteriuose jos darbą atlieka `wsgi.py` failas, bet jis skirtas tik developinimui. 




## Saugus jungimasis (SSL, HTTPS)

Šiais laikais nėra priežasčių svetainės nedaryti saugios (`https`). Tam, kad svetainė būtų saugi, reikia joje įdiegti saugumo sertifikatą (SSL certificate).

Tačiau šį sertifikatą galėsit susidiegti tik tada, kai užsiregistruosit savo domeną. 

Yra daug saugumo sertifikatų tiekėjų. Dauguma jų yra mokami. Bet yra ir nemokamas:Let's Encrypt

### Let's encrypt diegimas

https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal

## Svetainės atnaujinimas po pakeitimų

### git 

Jei atnaujinam svetainės kodą ir norim, kad atsinaujintų ir mūsų svetainė, reikia atnaujintą kodą įkelti į GitHub, o iš ten - pasiimti į serverį. 

Pakeitus kodą, savo kompiuteryje reikia atlikti šiuos veiksmus savo kompiuteryje:

`git add pakeistas_failas` arba `git add .` jei pakeitėm daugiau failų. Taip pridedami failai kėlimui į github.
`git commit -m "pakeitimai"` - užtvirtinam pakeitimus
`git push` - nusiunčiam į GitHub

Serveryje reikia atlikti šiuos veiksmus:

* nueinam į savo projekto direktoriją
* parsiunčiam atnaujinimus iš git: `git pull`
* perkraunam gunicorn (įdėti komandą)


[How to generate Django Secret Key](https://codinggear.blog/django-generate-secret-key/)
