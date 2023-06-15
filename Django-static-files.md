# Django static files

Django turi savo sistemą, kaip tvarkyti su nekiintamais failais, ("static files")

Static files yra failai, kurie yra nekintami. Tai CSS failai svetainės stiliams aprašyti, bendri svetainės dizaino paveikslėliai (logotipai, diziano iliustracijos, social media nuorodos) 
ir JavaScript failai, skirti kai kuriems svetainės funkcionalumams (bet ne frontend frameworks). 

Django projekte, `static files` dažniausiai dedami į šakniniame projekto folderyje esantį folderį pavadinimu `static`. Galima dėti ir kitur, šio folderio pavadinimas nurodomas `settings.py` faile, bet rekomenduojama laikytis šio susitarimo. 

Į `static` folderį galima failus dėti tiesiai, bet rekomenduojama suskirstyti į dar atskirus folderius pagal failų tipą: CSS, JS, paveikslėliai. 

Folderis `static` tada atrodytų taip:

- static
  - css
  - images
  - js

