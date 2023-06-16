# Django static files

## Static files sukūrimas ir struktūra

Kai kuriam puslapį, mums reikia ne tik funkcionalumo, bet ir dizaino. 

Dizainą darom su CSS pagalba, taip pat pridedam įvairių iliustracijų, logotipų, ikonėlių. 

Taip pat, galima pridėti judančių elementų ar frontend funkcionalumo - kaip, pavyzdžiui, "like" ir naudojant JavaScript.

CSS stiliai dažniausiai saugomi atskiruose CSS failuose (su `.css` plėtiniu). JavaScript scriptai - JS failuose (su `.js` plėtiniu). 

Šie failai nepriklauso nuo Python kodo, jie į naršyklę visada siunčiami tokie, kokie yra, nekintami (kitaip, nei pavyzdžiui, templates, kur Python įrašo reikšmes, ir tada siunčia). Todėl jie vadinasi "statiniai (nekintantys) failai" - `static files`.

Django projekte, `static files` dažniausiai dedami į appsuose esančius folderius pavadinimu `static`. 

Jei mūsų `app` reikalingi static failai, reikia juose susikurti folderį `static`. 


Į `static` folderyje rekomenduojama pirmiausia sukurti folderį tuo pačiu pavadinimu, kaip appsas. Pavyzdžiui, jei mūsų appso pavadinimas yra `my_app`, tai `static` folderio viduje taip pat sukuriam folderį `my_app`. To reikia tam, kad jeigu skirtinguose appsuose turėsim statinius failus tais pačiais pavadinimais, Django neužrašytų vieno ant kito, nes vėliau, kai juos pateikia į naršyklę, Django juos pirmiausiai sudeda į vieną vietą. 

Tada folderyje `my_app` reikia sukurti folderius `css`, `images`, `js`. Pavyzdžiui, jei mūsų appsas vadinasi `my_app`, tai folderių struktūra būtų tokia:

- my_app
  - static
    - my_app
      - css
      - images
      - js

Static failų saugojimo nustatymai gali būti keičiami `settings.py` faile nurodant `STORAGES` nustatymus: https://docs.djangoproject.com/en/4.2/ref/settings/#storages (tik nuo Django 4.2 versijos)

Dabar kiekviename iš tų folderių galime susikurti savo failus. Pavyzdžiui, CSS failą galim pavadinti `style.css`, galim įkelti kelis paveikslėlius, pavyzdžiui logotipą `logo.png`, ir galbūt JavaScript failą `scripts.js`.

Tada failų struktūra atrodytų taip:

- my_app
  - static
    - my_app
      - css
        - style.css
      - images
        - logo.png
      - js
        - scripts.js


Tam, kad mūsų šitie failai būtų pasiekiami naršyklėje, settings faile yra toks nustatymas:

`STATIC_URL = 'static/'`

Tai reiškia, kad kai naršyklė kraus mūsų svetainę, ir kartu su ja kraus šiuos statinius failus, jie bus prieinami per kelią `/static/`. T.y., jei mūsų svetainė bus https://example.com, tai statiniai failai bus pasiekiami per kelią https://example.com/static. Pavyzdžiui, mūsų appso css failas bus pasiekiamas par https"example.com/static/my_app/css/style/css iš kur naršyklė jį ir užkraus, kad galėtų jį naudoti mūsų svetainės dizaino stilių sudėjimui.

## Static files naudojimas šablonuose

Kadangi static files yra dizaino dalis, skirta padaryti mūsų HTML puslapius gražesniais, jie kraunami iš Django html šablonų. 

Jei norim šablone naudoti static failus, pirmiausiai turim užkrauti Django Templates biblioteką, skirtą darbui su jais. 

Tam HTML šablono failo pradžioje reikia įrašyti šią komandą:

`{% load static %}`

Vėliau, norėdami įdėti šiuos failus į template'us, turėsim naudoti Django templates tag'ą `{% static %}`

CSS failai yra kraunami HTML failo `head` dalyje:

```html
{% load static %}
...
<head>
  ...
  <link rel="stylesheet" href="{% static 'irankis/css/style.css' %}">
  ...
</head>
```

Paveikslėliai gali būti dedami įvairiose HTML `body` dalies vietose - ten, kur jų mums reikai pagal dizainą.

Jie dėsis taip:

```html
{% load static %}
...
<img src="{% static 'irankis/images/logo.png' %}">
```

JavaScript failai dažniausiai dedami pačioje HTML failo pabaigoje, prieš `</body>` tag'ą. Toje vietoje jie dedami todėl, kad JS failai gali būti ilgi ir ilgai krautis, todėl norim, kad mums pirmiausia užkrautų visą HTML'ą. 

JavaScript įdėjimas į šabloną:

```html
{% load static %}
...
<body>
  ...
  kažkoks turinys
  ...
  <script src="{% static "irankis/js/script.js" %}"></script>
</body>
```

## Static files serveryje

Mūsų kompiuteryje Django pats surenka static files iš apps'ų ir pateikia juos į naršyklę. Bet serveryje tai neveiks. 

Todėl, prieš keliant failus į serverį, reikia atlikti šiuos dalykus:

### STATIC_ROOT nustatymai

Reikia įrašyti kompiuteryje/serveryje esančio folderio, į kurį bus surinkti visi static files, pavadinimą:

`STATIC_ROOT = Path(BASE_DIR / 'static')`

Čia, naudojant Path bilioteką, sujungiamas mūsų projekto folderis - `BASE_DIR` ir mūsų folderio `static` pavadinimas.

### `collectstatic`

Tam, kad Django surinktų static failus iš visų appsų ir sudėtų į aukščiau aprašytą `static` folderį, esantį šakninėje projekto direktorijoje, reikia paleisti šią komandą:

`python manage.py collectstatic`

Ši komanda sukurs direktoriją `static` jei ji dar nebuvo sukurta, ir sukels į ją static failus iš visų appsų. Šio folderio struktūra tada atrodys maždaug taip:

- static
  - my_app
    - css
      - style.css
    - images
      - logo.png
    - js 
      - scripts.js
  - another_app
    - css
      - style.css
    - images
      - image.png
      - background.jpg
      - social_media.svg
    - js
      - likes.js

Taip pat serveryje reikės nustatyti `nginx` arba `apache` nustatymus taip, kad šis folderis būtų pasiekaimas per naršyklę. Bet apie tai bus [Deployment dalyje](https://github.com/karina-klinkeviciute/python_mokymu_medziaga/blob/main/Deployinimas.md) (kol kas neparuošta)

