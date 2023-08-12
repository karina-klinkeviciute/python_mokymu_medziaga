# Savas naudotojo modelis (User model)

Darant didesnį projektą, kur registruosis naudotojai, ir kur bus svarbus jų patogumas ir saugumas, rekomenduojama naudoti savo naudotojo modelį vietoje standartinio. 

## Savo naudotojo modelio privalumai

Savo naudotojo modelį verta turėti dėl šių priežasčių:

- Galima prisidėti savo laukų prie naudotojo modelio (User model). Taip nereikės daryti atskiro modelio naudotojo profiliui, bus viskas vienoj vietoj, kas bus patogiau programuojant. Tai ypač patogu, jei kurie nors iš tų laukų yra privalomi - tada be jų neleis sukurti naudotojo.
- Galima padaryti, kad prisiregistruoti ir prisijungti galima būtų naudojant el. pašto adresą ir slaptažodį, nereikėtų susigalvoti naudotojo vardo.
- Galima įprastą ID lauką, kur naudotojo identifikatorius parenkamas iš eilės einantis skaičius nuo 1, pakeisti į UUID tipo lauką. Tai padeda išspręsti vieną iš saugumo spragų: iš eilės einančius skaičius įsilaužėliai gali pakeisti į kitokius, ir taip pasiekti kito naudotojo informaciją. UUID identifikatorius yra per daug sudėtingas, kad pavyktų tai padaryti. Šiuo atveju geriausia naudoti UUID4 variantą. [Daugiau apie UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)

## Naudotojo modelio keitimas savu

Django turi savo naudotojo modelį `auth.User` iš app `auth`, kuris naudojamas naudotojo autentifikacijai. Jame yra laukai pagrindiniams duomenims, kaip naudotojo vardas, slaptažodis, vardas, pavardė, el. pašto adresas. 

Šis modelis labai patogus, nes mums nereikia viso to kurti patiems. Bet šio modelio negalim keisti, nes jis yra pačio Django dalis. 

Jei mums reikia kiek kitokio funkcionalumo mūsų naudotojams, galima pasidaryti savo naudotojo modelį. 

Tam, kad mūsų pačių naudotojo modelis gerai veiktų su visa Django autentifikacijos sistema, geriausia jį paveldėti iš vieno iš šių modelių: `AbstractUser` arba `AbstractBaseuser`. Dažniausiai mums užteks paveldėti iš `AbstractUser`, ir tai ir rekomenduojama daryti. Paveldėti iš `AbstractBaseUser` geriausia tik tada, jei neužtenka funkcionalumo paveldint iš `AbstractUser`. 

### Naujo app sukūrimas

Savo naudotojo modeliui kurti patogiausia yra susikurti tam atskirą app. Šiame pavyzduje pavadinsiu jį `user`. 

`python manage.py startapp user`

Pirmiausiai sukursim modelį, kuriame pakeisim 

```python
class Naudotojas(AbstractUser):

    # kadangi norim, kad naudotojas jungtųsi ne su naudotojo vardu (username),
    # bet su el. paštu (email), t.t. el. paštas naudojamas naudotojo identifikavimui,
    # todėl email pas kiekvieną naudotoją turi būti skirtingas.
    # Standartiniam naudotojo modely, ir AbstractUser modely jis nėra unikalus.
    # Todėl reikia perrašyti email lauką ir nurodyti, kad jis privalo būti unikalus, t.y. nesikartoti.
    email = models.EmailField(
        verbose_name="email address",
        max_length=255,
        unique=True,
    )

    # Tam, kad naudotojo ID neitų iš eiles, t.y. nebūtų 1, 2, 3... mums reikia pridėti savo id lauką, kuris būtų UUID formatu. Čia reikia naudoti UUID4 variantą.
    uuid = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    ivertinimas = models.IntegerField(null=True, blank=True)
    miestas = models.CharField(max_length=255, null=True, blank=True)
    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)
    username = models.CharField(
        'username',
        max_length=1000,
        # unique=False,
        # help_text=_('Required. 150 characters or fewer. Letters, digits and @/./+/-/_ only.'),
        # validators=[username_validator],
        # error_messages={
        #     'unique': _("A user with that username already exists."),
        # },
        null=True,
        blank=True
    )

    objects = MyUserManager()

    USERNAME_FIELD = "email"
    REQUIRED_FIELDS = ["miestas"]

    def __str__(self):
        return self.email
```
