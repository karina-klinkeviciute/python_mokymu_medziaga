# Saugumas kuriant Python ir Django projektus

## Slapti Django duomenys

Kai kuriuos dalykus Django reikia laikyti slaptais. Ypač, jei mūsų github repozitorija yra Public. Bet net ir privačioje repozitorijoje, ypač, jei daugiau žmonių turi priėjimą prie jos, geriau nelaikyti slaptų duomenų, tokie, kaip secret keys, slaptažodžiai ar API raktai. Juos reikėtų surašyti į `.env` failą.

.env failas: https://codinggear.blog/django-environment-variables/

Django Secret Key - jį **būtina** įdėti į .env failą (žiūrėti aukščiau)

## Debug nustatymai

Serveryje nustatynas DEBUG, esantis settings.py faile, turi būti nustatytas į False (DEBUG=False). Gali būti nustatomas per atskirą nustatymų failą arba per .env failą.

Serveryje:

`DEBUG=False`

## UUID vietoje 5prasto ID


Pagal [OWASP top 10 saugumo rizikų sąrašą](https://owasp.org/API-Security/editions/2023/en/0x11-t10/), dažniausiai pasitaikanti rizika yra ta, kad parodomi objekto ID, o per tai galima pasiekti objektus, kurių neturėtume pasiekti, pakeitus ID:

APIs tend to expose endpoints that handle object identifiers, creating a wide attack surface of Object Level Access Control issues. Object level authorization checks should be considered in every function that accesses a data source using an ID from the user.

Ypač svarbu UUID vietoje ID naudoti naudotojo modeliui. Bet taip pat gerai tai daryti ir bet kuriam kitam modely. 

Kaip pakeisti Primary key į uuid: https://djangoandy.com/2020/10/14/should-i-use-uuid-as-my-model-id-in-django/

## 2FA (Two factor authentication)

Biblioteka darbui su 2FA : https://django-otp-official.readthedocs.io/en/stable/

Ši biblioteka skirta darbui būtent su OTP (One Time Password). Jungimuisi šiuo būdu reikalingos specialios programėlės, tokios, kaip Google Authenticator arba Microsoft Authenticator. Jungiantis šiuo būdu, nesiunčiamos SMS, todėl tai nieko nekainuoja, tai galima jį naudoti ir mažo biudžeto projektams. 

## SSL/TLS ir HTTPS

https: https://letsencrypt.org/

## Failai, kurie turi būti prieinami tik tam tikriems žmonėms

Private storage: Jei reikia kelti failus, kurie turi būti prieinami tik autorizuotiems naudotojams, ir galbūt netgi tik tam tikriems naudotojams (pavyzdžiui, įkelti dokumentai, kaip sąskaitos, sutartys ar kiti slapti/jautrūs dokumentai), reikia naudoti biblioteką, kuri tikrintų šiuos leidimus ir failus pateiktų tik tiems vartotojams, kuriems galima. Viena iš tam skirtų bibliotekų: https://pypi.org/project/django-private-storage/

## Honeypots

honeypots - bandymų įsilaužti pagavimui. Tai yra netikri prisijungimo ar kiti svarb8s puslapiai, kurie "sugaudo" bandymus 5silau=ti ir tada galima blokuoti tuos IP ar panašiai.  https://pypi.org/project/django-honeypot/

## Django svetain4s saugumo patikrinimas

Django svetainės saugumui patinkrinti: https://djcheckup.com/ 


## Interneto saugumas bendrai

[OWASP](https://owasp.org/API-Security/editions/2023/en/0x11-t10/) yra organizacija, kuri rūpinasi interneto ir kompiuterio programų saugumu.  

[OWASP top 10 saugumo rizikų 2023 metais](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)


## github saugumo įrankiai

Github įskiepis, tikrinantis Django projekto saugumą pačiame kode, esančiame github: https://github.com/marketplace/django-doctor 

Yra ir kitų įskiepių, skirtų saugumui: https://github.com/marketplace?category=security&type=apps&query= 

Taip pat github turi ir kitų įrankių, skirtų analizuoti kodo kokybę ir saugumą. Juos galima rasti prie projekto pasirinkus "Actions" ir ten susiradus kategoriją "Security"

Dar vienas patogus įrankis dependabot. Jis skirtas stebėti bibliotekų, naudojamų jūsų projekte, saugumo atnaujinimus: https://docs.github.com/en/code-security/dependabot/dependabot-security-updates/configuring-dependabot-security-updates


## Kita

Video, paaiškinantis kai kuriuos saugumo aspektus Django: https://www.youtube.com/watch?v=CN6zJlqdxt0 


