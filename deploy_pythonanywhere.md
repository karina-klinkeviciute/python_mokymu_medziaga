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

.env
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
tik joje vietoje <your-github-username> ir <your-repo-name> reikia surašyti 

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
  
Kad galėtume prisijungti prie administravimo aplinkos ir ten kelti suomenis, reikia susikurti vartotoją:
  
```python manage.py createsuperuser```

## Aplinkos kintamųjų surašymas
  

Jei norime pasižiūrėti, kaip atrodo mūsų svetainė ir prie jos prisijungti, reikia eiti į "Web" skiltį. Ten rasim savo svetainės adresą ir jos informaciją. 
  
## Saugumas 
  
  Norėdami, kad mūsų 
  
  .env
  
  https
  
  Paspaudę ant jo, atsidarysim svetainę, o prie nuorodos pridėję /admin/ - administravimo aplinką. Gali būti, kad rodys, kad svetainė nesaugi. Taip yra todėl, d paspaudus ant nuorodos, ją atidaro `http` protokolu. 
  


