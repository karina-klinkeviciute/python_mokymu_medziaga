# Django static files

Django turi savo sistemą, kaip tvarkyti su nekiintamais failais, ("static files")

Static files yra failai, kurie yra nekintami. Tai CSS failai svetainės stiliams aprašyti, bendri svetainės dizaino paveikslėliai (logotipai, diziano iliustracijos, social media nuorodos) 
ir JavaScript failai, skirti kai kuriems svetainės funkcionalumams (bet ne frontend frameworks). 

Django projekte, `static files` dažniausiai dedami į appsuose esančius folderius pavadinimu`static`. 

Šių failų nustatymai gali būti keičiami `settings.py` faile nurodant `STORAGES` nustatymus: https://docs.djangoproject.com/en/4.2/ref/settings/#storages (tik nuo Django 4.2 versijos)

Į `static` folderyje rekomenduojama pirmiausia sukurti folderį tuo pačiu pavadinimu, kaip appsas, o jame - folderius `css`, `images`, `js`. Pavyzdžiui, jei mūsų appsas vadinasi `my_app`, tai folderių struktūra būtų tokia:

- my_app
  - static
    - my_app
      - css
      - images
      - js

