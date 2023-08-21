Video, paaiškinantis kai kuriuos saugumo aspektus Django: https://www.youtube.com/watch?v=CN6zJlqdxt0

Allowed hosts https://zerotobyte.com/use-django-allowed-hosts-to-prevent-security-threats/

.env failas: https://codinggear.blog/django-environment-variables/

Django Secret Key - jį būtina įdėti į .env failą (žiūrėti aukščiau)

Debug nustatymai - serveryje nustatynas DEBUG, esantis settings.py faile, turi būti nustatytas į False (DEBUG=False). Gali būti nustatomas per atskirą nustatymų failą arba per .env failą.

2FA: https://django-otp-official.readthedocs.io/en/stable/

uuid: https://djangoandy.com/2020/10/14/should-i-use-uuid-as-my-model-id-in-django/

https: https://letsencrypt.org/

Private storage: Jei reikia kelti failus, kurie turi būti prieinami tik autorizuotiems naudotojams, ir galbūt netgi tik tam tikriems naudotojams (pavyzdžiui, įkelti dokumentai, kaip sąskaitos, sutartys ar kiti slapti/jautrūs dokumentai), reikia naudoti biblioteką, kuri tikrintų šiuos leidimus ir failus pateiktų tik tiems vartotojams, kuriems galima. Viena iš tam skirtų bibliotekų: https://pypi.org/project/django-private-storage/

honeypots - bandymų įsilaužti pagavimu: https://pypi.org/project/django-honeypot/

