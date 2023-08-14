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

Pirmiausiai `models.py` faile sukursim modelį, kuriame nustatysim email 
naudojimą vietoje username bei uuid naudojimą vietoje id.

```python
class MyUser(AbstractUser):

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

    # Tam, kad naudotojo ID neitų iš eiles, t.y. nebūtų 1, 2, 3... mums reikia pridėti savo id lauką, 
    # kuris būtų UUID formatu. Čia reikia naudoti UUID4 variantą.
    # Šis laukas turi būti nustatytas kai Primary Key. 
    # Taip pat jis turi turėti numatytąją reikšmę uuid.uuid4, kas reiškia, kad bus sugeneruota nauja atsitiktinė UUID4 reikšmė
    uuid = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    
    # Kadangi naudotojo vardas mums nebėra aktualus, reikia perrašyti jo lauką. 
    # Klasėje AbstractUser jis yra privalomas ir unikalus. Mums dabar tai tik trukdys. 
    # Todėl reikia iš naujo mūsų modelyje aprašyti šį lauką, kad galėtume padaryti jį neunikaliu, 
    # o taip pat, kad galėtume leisti, kad jis būtų tuščias.
    username = models.CharField(
        'username',
        max_length=1000,
        null=True,
        blank=True
    )
    
    # Reikia pakeisti ir model manager mūsų naujam naudotojo modeliui. 
    # Tai reikia padaryti todėl, kad model manager šiuo atveju rūpinasi naudotojų sukūrimu
    # Pas mus nauotojų kūrimas pasikeitė, nes nebenaudojam username lauko identifikacijai,
    # o naudojam email lauką.
    objects = MyUserManager()

    # Tam, kad naudotojas galėtų jungtis su el. paštu, o ne username, reikia nurodyti, 
    # kad laukas, pagal kurį naudotojas bus identifikuojamas, yra email
    USERNAME_FIELD = "email"
    
    # Tėvinėje klasėje AbstractUser email buvo nurodytas, kaip required field. 
    # Bet kai jį padarėm, kad jis būtų USERNAME_FIELD, tada jis savaime tapo privalomas (required).
    # Tokiu atveju jis dubliuojasi. Kad nesidubliuotų, reikia perrašyti atributą REQUIRED_FIELDS taip,
    # kad jame nebūtų email lauko. 
    REQUIRED_FIELDS = []

    def __str__(self):
        return self.email
```

### Standartinio User modelio pakeitimas savu `settings.py` faile

Sukūrus savo naudotojo modelį, Django reikia pasakyti, kad naudotų jį, o ne standartinį. Tai reikia padaryti įdedant šią eilutę į `settings.py` fail`:

```python
AUTH_USER_MODEL = "user.MyUser"
```

### Modelio tvarkyklės (model manager) aprašymas

Tam, kad galima būtų sukurti naudotojus neįvedant naudotojo vardo, 
bet įvedant email, mums reikia pakeisti ir naudojtojo bei supernaudotojo sukūrimo 
metodus. Jie yra klasėje BaseUserManager, todėl iš jos mums reikia ir paceldėti savo klasę MyUserManager,
kurią nurodėme modelyje.

Joje pakeičiam metodų aprašus bei naodotojo sukūrimo eilutes pagal pavyzdį. Šią klasę galim įdėti į tą patį 
failą, kaip ir modelius, bet tada *klasė MyUserManager* turi eiti prieš klasę *MyUser*

```python
class MyUserManager(BaseUserManager):
    def create_user(self, email, password=None):
        """
        Creates and saves a User with the given email, date of
        birth and password.
        """
        if not email:
            raise ValueError("Users must have an email address")

        user = self.model(
            email=self.normalize_email(email),
        )

        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, password=None):
        """
        Creates and saves a superuser with the given email, date of
        birth and password.
        """
        user = self.create_user(
            email,
            password=password
        )
        user.is_admin = True
        user.is_superuser = True
        user.is_staff = True
        user.save(using=self._db)
        return user
```

Jei į `MyUser` modelį nepridedam papildomai savų privalomų laukų, šito turėtų užtekti, 
kad galėtume naudotis savo naudotojo modeliu. 

