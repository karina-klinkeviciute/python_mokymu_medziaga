# Paruošimas savo kompiuteryje

## Migracijos

`python manage.py migrate`

Surandam migracijų failus, pasitikrinam, kad jie prid4ti prie git ir įkelti į github.

## .env failas ir Django settings.py failas

Susikuriam šakninėje git direktorijoje `.env` failą

Į jį įkeliam šiuos nustatymus:

```
SECRET_KEY="m*epo+((q9t4h-g1zf9-nclc80-mthneqryca)0by@9=vgna6k"
DEBUG=True
```

Taip pat kitus, jei yra kas nors slapto mūsų nustatymuose.

`.env` failą įdeda į `.gitignore` 

iš `.env` failo nustatymų pavadinimus perkopijuojam į `.env_example`. Slaptas reikšmes ištrinam arba pakeičiam. 

```
SECRET_KEY="slaptas_raktas"
DEBUG=True
```

Suinstaliuojam python-dotenv biblioteką:

```bash
pip install python-dotenv
```

`settings.py` faile pridedam šitas eilutes:

```python
from dotenv import load_dotenv
```

```python
load_dotenv()
```

Ir pakeičiam šitas (bei kitas, kur yra slaptų nustatymų):

```python
SECRET_KEY = os.getenv("SECRET_KEY")
DEBUG = True if os.getenv("DEBUG") == "True" else False
```

## requirements failas

`pip freeze > requirements.txt`

## sukeliam viską į github

pridedam visus naujus failus su `git add` (arba per Pycharm)

Tada:

```git commit -am "paruošta deploynimui```

```git push```

# Projekto paruošimas serveryje

## Naujo naudotojo sukūrimas

```sudo adduser karina```

```passwd karina```

`nano /etc/sudoers`

Čia, po naudotojo `root` teisių, pridedam teises savo naudotojui:

```bash
# User privilege specification
root    ALL=(ALL:ALL) ALL
karina      ALL=(ALL:ALL) ALL
```

```sudo su karina```

## Projekto įkėlimas

```cd /usr/local```

```sudo mkdir projects```

```sudo chown -R karina projektai```

```cd projects```

```git clone https://github.com/karina-klinkeviciute/CodeAcademyDjango.git```


## Virtualios aplinkos sukūrimas

```sudo apt update && apt upgrade```

```sudo apt install python3.10-venv```

```sudo apt install python3-pip```

```python3 -m venv venv```

```source venv/bin/activate```




## Aplinkos kintamieji

```cd CodeAcademyDjango``` (vietoj "CodeAcademyDjango" - jūsų projekto pavadinimas)

```ls -al```

Jei šioje direktorijoje yra `.env_example` failas, perkopijuojam jį į `.env` failą:

```cp .env_example .env```

``` nano .env```

Sugeneruojam slaptą raktą:

```python```

```python
from django.core.management.utils import get_random_secret_key
print(get_random_secret_key())
```

Šis kodas atspausdins mums kažką panašaus į šitai:

```m*epo+((q9t4h-g1zf9-nclc80-mthneqryca)0by@9=vgna6k```

Nukopijuojam šį kodą. Išeinam iš Python (rašom `exit()`)

Atsidarom .env failą:

`nano .env`

Įkopijuojam kodą, kad būtų panašiai, kaip čia:

```
SECRET_KEY="m*epo+((q9t4h-g1zf9-nclc80-mthneqryca)0by@9=vgna6k"
DEBUG = False
```

Išsaugom ir išeinam (Ctrl+X)


## Django ir kitų bibliotekų instaliavimas


```pip install -r requirements.txt```

## Persijungiam 5 projekto direktorij1 (kur yra manage.py failas)

`cd irankiai`


## Migracijos


`python manage.py migrate`

## Static failai

`python manage.py collectstatic`

## Pagrindinio naudotojo (super user) sukūrimas

  
```python manage.py createsuperuser```

