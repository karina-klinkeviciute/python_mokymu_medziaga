# Django static files

Kai kuriam puslapį, mums reikia ne tik funkcionalumo, bet ir dizaino. 

Dizainą darom su CSS pagalba, taip pat pridedam įvairių iliustracijų, logotipų, ikonėlių. 

Taip pat, galima pridėti judančių elementų ar frontend funkcionalumo - kaip, pavyzdžiui, "like" ir naudojant JavaScript.

CSS stiliai dažniausiai saugomi atskiruose CSS failuose (su `.css` plėtiniu). JavaScript scriptai - JS failuose (su `.js` plėtiniu). 

Šie failai nepriklauso nuo Python kodo, jie į naršyklę visada siunčiami tokie, kokie yra, nekintami (kitaip, nei pavyzdžiui, templates, kur Python įrašo reikšmes, ir tada siunčia). Todėl jie vadinasi "statiniai (nekintantys) failai" - `static files`.

Django projekte, `static files` dažniausiai dedami į appsuose esančius folderius pavadinimu`static`. 

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


