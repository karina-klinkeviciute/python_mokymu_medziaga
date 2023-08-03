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

```sudo su karina```

## Projekto įkėlimas


