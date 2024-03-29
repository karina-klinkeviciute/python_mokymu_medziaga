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


`.gitignore` turinys:

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

```bash
git add .env_example
```

```bash
git commit -m "pridejom .env_example"
```

```bash
git push
```

Žemiau bus parašyta, kokius veiksmus su šiais failais reikės atlikti serveryje.

##### `settings.py` failas

Nustatymų faile `settings.py` aukščiau minėtų kintamųjų reikšmės turi būti užkraunamos iš `.env` failo. 

Tam reikia naudoti biblioteką `dotenv`

Ją pirmiausiai reikia suinstaliuoti:

```bash
pip install python-dotenv
```

Suinstaliavę naują biblioteką, nepamirškime atnaujinti `requirements.txt`:

```bash
pip freeze > requirements.txt
```


`settings.py` failo pradžioje, prie kitų `import` sakinių, reikia pridėti ir tokį sakinį:

```python
from dotenv import load_dotenv
```

Po `import` sakinių, reikia įrašyti tokią eilutę: 

```python
load_dotenv()
```

Tada reikia surasti nustatymus, kuriuos norime padaryti slaptais, ir juos pakeisti taip:

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
```bash
git add requirements.txt
```

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

```sudo adduser karina```

Sukūrus naudotoją, reikia nustatyti jam slaptažodį:

```passwd karina```

Vartotoją pridedam į sudoers:

`nano /etc/sudoers`

Čia, po naudotojo `root` teisių, pridedam teises savo naudotojui:

```bash
# User privilege specification
root    ALL=(ALL:ALL) ALL
karina      ALL=(ALL:ALL) ALL
```

Prisijunkim šiuo naudotoju:

```sudo su karina```

### Projekto įkėlimas

Projekto geriausiai nekelti į šakninę direktoriją, o jam sukurti naują direktoriją. Vienas iš variantų yra sukurti naują direktoriją, skirtą projektams, `/usr/local/` direktorijoje. Ši direktorija neliečiama darant sistemos atnaujinimus, todėl patogu tokius dalykus laikyti joje.

```cd /usr/local```

```sudo mkdir projects```

Pakeičiam folderio `projects` savininką į savo naują vartotoją:

`sudo chown -R karina projects`

Įeinam į sukurtą direktoriją: 

```cd projects```


Šioje direktorijoje `git` pagalba įkelsim savo projektą:

```git clone https://github.com/karina-klinkeviciute/CodeAcademyDjango.git```

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

Python Ubuntu operacinėje sistemoje jau bus sudiegtas, bet `venv` ir `pip` mes joje dar neturėsime. Jį mums reikia susiinstaliuoti. Tam naudosim ocmandą `apt install`

Rekomenduojama pirmiausiai atnaujinti `apt` programą ir jos šaltinius:

```sudo apt update && apt upgrade```

Tada suinstaliuojam `venv` ir `pip`:

```sudo apt install python3.10-venv```

```sudo apt install python3-pip```

Tada reikia susikurti virtualią aplinką.

Tam paleidžiam komandą: 

```python3 -m venv venv```

Tada šią virtualią aplinką aktyvuojam:

```source venv/bin/activate```

### Django ir kitų bibliotekų instaliavimas

Kai aktyvuojam virtualią aplinką, į ją reikia suinstaliuoti visa mūsų projektui reikalingas bibliotekas. Projekto paruošimo dalyje mes jas surašėm į failą `requirements.txt`. Paleidžiam komandą, kuris suinstaliuos šias bibliotekas iš to failo:

```pip install -r requirements.txt```



### Migracijos

Į serverį savo duomenų bazės neįkėlėm, todėl čia reikia paleisti migracijas, kad sukurtų duomenų bazę ir lenteles pagal mūsų modelius.

paleidžiam migracijas:

`python manage.py migrate`

### Static failai

Static failai serveryje veikia kiek kitaip, nei mūsų kompiuteryje, todėl reikia paleisti komandą, kad juos visus sukopijuotų į `static` direktoriją:

`python manage.py collectstatic`


### Pagrindinio naudotojo (super user) sukūrimas
  
Kad galėtume prisijungti prie administravimo aplinkos ir ten kelti duomenis, reikia susikurti naudotoją:
  
```python manage.py createsuperuser```

Atsidariusiame dialoge reikia įrašyti naudotojo vardą ir kitus duomenis, kaip darėte ir savo kompiuteryje.


### gunicorn ir nginx diegimas

#### gunicorn

gunicorn yra Web serverio programinė įranga, skirta Python. Ji naudojama "production" serveriuose. Mūsų kompiuteriuose jos darbą atlieka `wsgi.py` failas, bet jis skirtas tik developinimui. 

Savo projekte, esant įjungtai virtualiai aplinkai (jei j1 išjungėt, reikia įjungti vėl su `source venv/bin/activate`) reikia suinstaliuoti gunicorn:

```bash 
pip install gunicorn
```

Pabandykim pasileisti gunicorn ir pažiūrėti, kaip veikia:

```bash
gunicorn irankiai.wsgi
```

Jei nerodo klaidų, reiškia viskas tvarkoje. Bet svetainės pamatyti vis dar negalėsim

Jei norim pamatyti svetainę, reikia paleisti šią komandą:

`gunicorn irankiai.wsgi:application --bind 0.0.0.0:8000` - tik `irankiai` reikia pakeisti savo projekto pavadinimu.

##### gunicorn paleidimas kaip serviso, naudojant systemd

Pirmiausiai reikia susikurti naują `service` failą folderyje `/etc/systemd/system/`

Failą pavadinam, pavyzdžiui `irankiai_gunicorn.service` (jūs duokit jam savo pavadinimą vietoj `irankiai` dalies)

```sudo nano /etc/systemd/system/irankiai_gunicorn.service```

Į šį failą idedam šį turinį:

```bash
[Unit]
Description=Gunicorn instance to serve myproject
After=network.target

[Service]
User=karina
Group=karina
WorkingDirectory=/usr/local/projektai/CodeAcademyDjango/irankiai
ExecStart=/usr/local/projektai/CodeAcademyDjango/venv/bin/gunicorn irankiai.wsgi:application --bind 127.0.0.1:8000

[Install]
WantedBy=multi-user.target
```

Perkraunam Systemctl, kad pakeitimai suveiktų:

`sudo systemctl daemon-reload`

Paleidžiam gunicorn ir nustatom, kad jis pasileistų automatiškai kraunantis serveriui:

```bash
sudo systemctl start irankiai_gunicorn
sudo systemctl enable irankiai_gunicorn
```

#### nginx

Suinstaliuojam nginx:

`sudo apt install nginx`

Surašom `nginx` nustatymus:

Sukuriam ir atsidarom šį failą:

`sudo nano /etc/nginx/sites-available/irankiai`

Jame surašom šiuos duomenis:

```bash
server {
    listen 80;
    server_name 209.38.218.242;

    location /static/ {
        alias /usr/local/projektai/CodeAcademyDjango/irankiai/static/;
    }

    location /media/ {
        alias /usr/local/projektai/CodeAcademyDjango/irankiai/media/;
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Čia sukūrėm konfigūracijos failą folderyje `sites_available`, kuriame sudedam visas mūsų galimas svetaines. Dabar reikia jį pridėti su simboline nuoroda (symlink) į įjungtų svetainių folderį `sites_enabled`:

`sudo ln -s /etc/nginx/sites-available/irankiai /etc/nginx/sites-enabled`

pasitikrinam nginx konfigūraciją:

`sudo nginx -t`

Paleidžiam nginx:

`sudo service nginx restart`




https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-22-04 


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

```git add pakeistas_failas``` arba ```git add .``` jei pakeitėm daugiau failų. Taip pridedami failai kėlimui į github.
```git commit -m "pakeitimai"``` - užtvirtinam pakeitimus
```git push``` - nusiunčiam į GitHub

Serveryje reikia atlikti šiuos veiksmus:

* nueinam į savo projekto direktoriją
* parsiunčiam atnaujinimus iš git:
  ```bash
  git pull
  ```
* perkraunam gunicorn:
  ```bash
  sudo systemctl restart gunicorn
  ```


[How to generate Django Secret Key](https://codinggear.blog/django-generate-secret-key/)
