# Gidas baigiamiesiems darbams
Kadangi baigiamasis darbas turi būti jūsų įgytų žinių ir gebėjimų demonstravimas, ši instrukcija specialiai bus mažai detali.

## Projekto pradžia

### Git repozitorijos sukūrimas

Jei dar neturim git repozitorijos savo projektui, reikia ją susikurti vienu iš mūsų išmoktų būdų.
[Git pagrindai](https://github.com/DonatasNoreika/python1lygis/wiki/Versij%C5%B3-valdymo-sistemos-(GIT))

## Django projekto sukūrimas

Sukuriam Django projektą:
https://github.com/robotautas/kursas/blob/master/Django/MDs1/django1.md

Taip pat susikuriam vieną ar daugiau programėlių (apps).


## Modeliai

Dirbant su Django, geriausia pradėti nuo modelių, kurie aprašo mūsų nuodojamus duomenis ir taip duoda mūsų projektui "griaučius".

### Modelių pavyzdys pagal Įrankių nuomos projekto aprašymą:

[Įrankių nuomos projektas](https://github.com/karina-klinkeviciute/irankiu_nuoma)

Sukuriam appsą "Įrankis"

Jame atsiras "models.py" failas

Šiame faile mums reikia kurti savo modelius. Pateiksiu pavyzdį, kaip pagal turimą aprašymą sukurti modelį:

Aprašymas:

- įrankis
    - pavadinimas
    - aprašymas
    - nuotrauka
    - naudotojas
    - kategorija 
    - galia
    - ar su pristatymu, ar reikia pasiimt


Modelis:

```python
class Irankis(models.Model):
    pavadinimas = models.CharField(max_length=255)
    aprasymas = models.TextField(blank=True, null=True)
    galia = models.IntegerField(blank=True, null=True)
    pristatymas = models.BooleanField()
    kategorijos = models.ManyToManyField(Kategorija)
    naudotojas = models.ForeignKey(NaudotojoProfilis, on_delete=models.CASCADE)
    nuotrauka = models.ImageField(upload_to="irankis", null=True, blank=True)
    
``` 
[Modelių kūrimo aprašymas](https://github.com/robotautas/kursas/blob/master/Django/MDs2/django2.md)

### Naudotojo modelis

Naudotojo (User) modelį Django turi jau paruoštą. Bet dėl įvairių priežasčių mums gali būti patogiau susikurti savo vartotojo modelį. Kaip tai padaryti, parašyta čia: https://docs.djangoproject.com/en/4.2/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project  To daryti nėra būtina (savo naudotojo modelio kūrimas yra kiek sudėtingesnis), galit pasitarti su dėstytojais, ar jūsų atveju geriau tai daryti, ar ne.

Savo naudotojo modelio kūrimo privalumai:

- galima naudoti email prisijungimui, vietoj naudotojo vardo (username)
- naudotojo modeliui vietoje įprasto `id` galima naudoti `uuid`. Čia argumentai, kodėl geriau naudoti `uuid`: https://djangoandy.com/2020/10/14/should-i-use-uuid-as-my-model-id-in-django/ 


## Administravimo aplinka

Po to, kaip susikuriam modelius, užregistruojam juos [administravimo aplinkoje](https://github.com/robotautas/kursas/blob/master/Django/MDs3/django3.md)

Taip pat susikuriam pagrindinį vartotoją (superuser)

Sukonfigūravę admin aplinką, galim pradėti vesti duomenis.

## Views, urls ir templates

Kiekvienam duomenų atvaizdavimui (daiktų/dalykų/straipsnių sąrašams, vieno daikto/dalyko atvaizdavimui, turinio suvedimui) reikia šios trijulės - view, url, template. Nėra labai svarbu, kuria tvarka juos kuriam. Kiekvienas atrasit sau patogiausią būdą. 

Nuoroda (url) rašoma į programėlės urls.py failą ir susieja užklausą (kelią), kurią naudotojas įveda naršyklėje, ir view. View, gavęs šia užklausą, pagal ją atrenka, kokius duomenis mums reikia pateikti, "surašo" juos į paruoštą "blanką" - template ir pateikia atgal naudotojui. 

[Daugiau apie templates ir Views](https://github.com/robotautas/kursas/blob/master/Django/MDs4/django4.md)

## Registracija/prisijungimas

Ne visuose projektuose bus reikalinga registracija. Bet jei jūsų projekte ji reikalinga, [čia yra apie vartotojų registraciją](https://github.com/robotautas/kursas/blob/master/Django/MDs7/django7.md)

Jei norim, kad vartotojai galėtų registruotis/prisijungti per kitas tapatybės nustatymo sistemas (Google, Facebook, Twitter, AppleID, Github ir kt.), tam galima naudoti šią biblioteką: https://django-allauth.readthedocs.io/en/latest/

## Autorizuotas turinys

Jei pas jus projekte yra naudotojų registracija, ir jei kai kurį turinį rodot tik registruotiems, ar tik daliai registruotų naudotojų, [čia galit rasti, kaip tai padaryti](https://github.com/robotautas/kursas/blob/master/Django/MDs8/django8.md)

## Formos

Jei jūsų svetainės naudotojai turi turėti galimybę įvesti turinį - [tam reikia formų](https://github.com/robotautas/kursas/blob/master/Django/MDs8/django8.md)

## Duomenų bazės 

Rimtesniems, ir realiai naudojamiems projektams verta pasirinkti ne SQLite, bet kokią nors kitą duomenų bazių valdymo sistemą. 

Su Django rekomenduojama naudoti Postgres, bet taip pat Django turi palaikymą ir kitoms duomenų bazių valdymo sistemoms

[Django duomenų bazių konfigūravimai](https://docs.djangoproject.com/en/4.2/ref/databases/)

## Django saugumas

**.env failas:** https://codinggear.blog/django-environment-variables/

**Django Secret Key** - jį būtina įdėti į `.env` failą (žiūrėti aukščiau)

**Debug nustatymai** - serveryje nustatynas `DEBUG`, esantis `settings.py` faile, turi būti nustatytas į `False` (`DEBUG=False`). Gali būti nustatomas per atskirą nustatymų failą arba per `.env` failą.

**2FA:** https://django-otp-official.readthedocs.io/en/stable/ 

**uuid:** https://djangoandy.com/2020/10/14/should-i-use-uuid-as-my-model-id-in-django/ 

**https:** https://letsencrypt.org/ 

**Private storage:** Jei reikia kelti failus, kurie turi būti prieinami tik autorizuotiems naudotojams, ir galbūt netgi tik tam tikriems naudotojams (pavyzdžiui, įkelti dokumentai, kaip sąskaitos, sutartys ar kiti slapti/jautrūs dokumentai), reikia naudoti biblioteką, kuri tikrintų šiuos leidimus ir failus pateiktų tik tiems vartotojams, kuriems galima. Viena iš tam skirtų bibliotekų: https://pypi.org/project/django-private-storage/ 

**hoenypots** - bandymų įsilaužti pagavimu: https://pypi.org/project/django-honeypot/ 


## Frontend (HTML, CSS, JS)

[HTML tutorial](https://www.w3schools.com/html/)

[CSS tutorial](https://www.w3schools.com/css/)

[Bootstrap](https://getbootstrap.com/)

## REST API kūrimas

Jei mes norim iš mūsų projekto kam nors pateikti vien duomenis, be šablonų, HTML, CSS, mes galim tai daryti su REST API. 

REST API naudojamas, jei: 
- be Django dar naudojam ir frontend JavaScript framework'ą, pavyzdžiui Vue, React, Ember, Angular, Svelte ar kitą. 
- kuriam mobiliąją programėlę, kuri keičiasi duomenimis su mūsų backend'u. 
- norim leisti kitomis programomis bendrauti su mūsų projektu.

Django turi biblioteką Django REST Framework, skirtą bendrauti su REST API. 

[Kurso medžiaga apie REST API Django](https://github.com/robotautas/kursas/tree/master/Django_REST)

[Django REST Framework dokumentacija](https://www.django-rest-framework.org/)

## GraphQL

Kitas būdas bendrauti su frontend frameworkais ar mobiliosiomis programėlėmis yra [GraphQL](https://graphql.org/). Jam Python turi [kelias bibliotekas](https://graphql.org/code/#python)


## Talpinimas į serverį

***Vėliau paruošiu medžiagą, kol kas gal pavyks pasileisti viską iš to, kas sudėta čia***

Talpinimui į serverį geriausia pasirinkti VPS. Yra daug variantų, kur tai galima padaryti:

Jei jūsų projektas nedidelis, yra keli variantai, kur galite jį talpinti nemokamai. Tik ten, jei viršijami limitai, pradeda skaičiuotis pinigai ir gali būti, kad vis tiek teks susimokėti:

- AWS
- Oracle
- Azure

Kiti variantai yra mokami:
- DigitalOcean (pigiausias variantas - $4 per mėnesį. Gana populiarus Hosting provideris, patogus valdymas)
- Heroku
- Google Cloud Services
- Hostinger
- Serveriai.lt

Kartais geriau netgi pasirinkti mokamą servisą, nes nemokamam, viršijus limitą, gali kainuoti daugiau :) Be to, daugeliu atvejų, norint užregistruoti domeną, vis tiek reikia imti mokamą planą. Bet reikia patiems pasidaryti tyrimą, kas jums geriausiai tinka, ir pasirinkti.

Projekto paruošimas talpinimui: https://github.com/karina-klinkeviciute/python_mokymu_medziaga/blob/main/Deployinimas.md 

Prisijungus prie savo serverio (Digital Ocean tai būtų droplet), reikia ten sukonfigūruoti `gunicorn` ir `nginx` (yra ir alternatyvų, apache, uwsgi, bet populiariausia ir turbūt patogiausia yra gunicorn+nginx)

Digital Ocean tutorial, kaip paruošti projektą su gunicorn, nginx ir postgres:  https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-22-04 

Šiame tutoriale Django projektas kuriamas serveryje. Jums reikės įkelti savo sukurtą projektą į serverį, naudojantis git. 

Taip pat galima užregistruoti domeną. Užregistravus domeną, reikia įdiegti SSL sertifikatą, kad galima būtų jungtis prie svetainės per `https`. Yra įvairių SSL sertifikatų teikėjų, siūlau naudoti Let's Encryot, kuris yra Open Source ir nemokamas: https://letsencrypt.org/

Kaip minėjau, vėliau paruošiu medžiagą, ir geriau jos palaukti, bet jei kas nors neturit kantrybės, galit bandyti patys :) 


## Kiti naudingi dalykai

**Paveikslėliai, puslapiavimas, paieška.** Jei jūsų projekte bus keliami paveikslėliai, naudojamas puslapiavimas ar norėsit naudotojams leisti ieškoti, [apie tai rasit informacijos čia](https://github.com/robotautas/kursas/blob/master/Django/MDs6/django6.md)

**TinyMCE tekstų redaktorius.** Jei norit padaryti, kad jūsų vartotojai galėtų rašyti gražiai suformatuotus tekstus, [galit įdiegti TinyMCE](https://github.com/robotautas/kursas/blob/master/Django/MDs8/django8.md#html-laukai-modeliuose)

**Sudėtingesnės užklausos su Q**: Kartais, kai norim Views'e išrinkti reikiamus objektus pagal kelis kriterijus, paprastos užklausos gali neužtekti. Tam galima naudoti Q užklausas, kurių pagalba galima rašyti sudėtingesnius loginius sakinius su AND ir OR: https://docs.djangoproject.com/en/3.2/topics/db/queries/#complex-lookups-with-q-objects

**Email siuntimas.** Apie tai, kaip siųsti emailus iš Django, galima rasti čia: https://docs.djangoproject.com/en/4.2/topics/email/ Nustatymai ir konfigūravimai, jei norim siųsti iš Gmail, čia: https://dev.to/abderrahmanemustapha/how-to-send-email-with-django-and-gmail-in-production-the-right-way-24ab  ir čia: https://opensource.com/article/22/12/django-send-emails-smtp 

Siunčiamiems iš projekto laiškams geriausia susikurti naują, atskirą el. pašto dėžutę, o nenaudoti savo.

Jei mums reikia siųsti emailus daug ir dažnai, geriausiai naudoti tam skirtus servisus, kaip Sendgrid, Omnisend, Mailchimp ir panašiai.

**Field choices.** Jei norim, kad modelyje kokio nors lauko reikšmes vartotojai galėtų pasirinkti tik iš mūsų nustatytų leidžiamų reikšmių, galim naudoti Field choices: https://docs.djangoproject.com/en/4.2/ref/models/fields/#choices 

**Duomenų importavimas/eksportavimas.** - Jei reikės per admin importuoti ar eksportuoti duomenis iš modelių ar į modelius, įvairiais formatais: Excel, CSV, JSON, tam yra ši biblioteka: https://django-import-export.readthedocs.io/en/latest/ 

**Prisijungimas su tapatybės nustatymo teikėjais (authentication providers) (Google, Facebook, Twitter, AppleID, Github ir kt.).** Jei norim, kad vartotojai galėtų registruotis/prisijungti per kitas tapatybės nustatymo sistemas, tam galima naudoti šią biblioteką: https://django-allauth.readthedocs.io/en/latest/

**Django management commands**

Kartais gali reikėti kokių nors komandų, kurias mes paleistume serveryje, iš command line, kurios padarytų kokius nors darbus mūsų projekte (skaičiavimai, valymai, duomenų tvarkymai). Tam naudojamos Django management komandos: https://docs.djangoproject.com/en/4.2/howto/custom-management-commands/

**Periodiniai darbai** Kartais gali reikėti preiodinių užduočių. Pavyzdžiui, kas mėnesį perskaičiuoti biudžetus, nustatytu laiko siųsti el. laiškus. 

Tam galima naudoti celery: https://pypi.org/project/django-celery/ 

Kitas būdas - cron kartu su Django Management Commands. 

**Django admin actions** Kai Django administravimo aplinkoje matom objektų sąrašą, tuos objektus pažymėjus, galima juos visus ištrinti. Tam naudojama Django admin action `delete`. Jei yra poreikis atlikti kokius nors kitus veiksmus per admin su vienu ar keliais pažymėtais objektais, mes galim susikurti savo Django admin actions: https://docs.djangoproject.com/en/4.2/ref/contrib/admin/actions/ 

**Private storage:** Jei reikia kelti failus, kurie turi būti prieinami tik autorizuotiems naudotojams, ir galbūt netgi tik tam tikriems naudotojams (pavyzdžiui, įkelti dokumentai, kaip sąskaitos, sutartys ar kiti slapti/jautrūs dokumentai), reikia naudoti biblioteką, kuri tikrintų šiuos leidimus ir failus pateiktų tik tiems vartotojams, kuriems galima. Viena iš tam skirtų bibliotekų: https://pypi.org/project/django-private-storage/ 


## Trumpa Django projekto kūrimo santrauka (cheat sheet)

![Santrauka](django-projekto-kurimas.png)
