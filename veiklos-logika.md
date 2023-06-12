# Veiklos logika (Business logic)

Veiklos logika programavime vadinama visa tai, ką reikia suprogramuoti, kas priklauso nuo veiklos, kuriai projektas daromas, o ne grynai technologiniai dalykai.

Pavyzdžiui, įrankių nuomos projekte veiklos logika bus viskas, kas susiję būtent su įrankių nuoma, su jai skirtu funkcionalumu. 

Į veiklos logiką, Django projekte, gali įeiti daug dalykų. Visų pirmiausia, pats modelių suplanavimas, laukai, ryšiai tarp modelių - tai jau yra veiklos logikos dalis. 

Bet tai ne tik tai, tai daug plačiau. Įvairūs skaičiavimai, veiksmai, kurie priklauso būtent nuo veiklos srities, vadinami veiklos logika. 

Tarp programuotojų dažnai kyla diskusijos, kur projekte turi būti aprašoma veiklos logika. Techniškai, Django atveju, tai gali būti atliekama ir modeliuose, ir views'uose. Dalis veiklos logikos būna ir formose. Kai kas prideda ir atskirus failus, kur deda veiklos logiką, pavyzdžiui, pavadindami juos "services".

## Veiklos logikos pavyzdžiai

Pavyzdžiui, įrankių nuomos projekte mes galim norėti patikrinti, ar kažkokį įrankį galima išsinuomoti nustatytoms datoms. Tam mums reikia tikrinti ne tik patį įrankį, bet visus jo vienetus. 
Tam reikia parašyti kodą, kuris skaitysis veiklos logika. 

Taip pat, bendros nuomos kainos suskaičiavimas yra veiklos logika. 

Naudotojo išsinuomotų įrankių sąrašas taip pat yra veiklos logika.

Saugant, pavyzdžiui, naują nuomos faktą, mes galim norėti išsiųsti email nuomotojui, kad galėtų pasiruošti perduoti įrankį. Tai taip pat veiklos logika. 

## Veiklos logika Django

Django priimta veiklos logiką, kiek įmanoma, rašyti prie modelių - "Fat models, thin views". Kaip minėjau, kai kurie tai rašo visai atskirai nuo šių dalykų (pavyzdžiui, rašo "services"). 

Rekomendacija kuo daugiau veiklos logikos rašyti prie models yra dėl to, kad tada ją lengviausia pernaudoti, nes tų pačių skaičiavimų gali reikėti keliose vietose. 

## Kodo pavyzdžiai

### 1 pavyzdys
Turime modelį `Event` ir modelį `EventRegistration`, kuris saugo atskirų vartotojų registracijas į renginius.
```python
class Event( models.Model ):
    date = models.DateTimeField( null = True )
    title = models.CharField( max_length = 100 )
    conference = models.ForeignKey( Conference, on_delete = models.CASCADE )
    # ...

class EventRegistration( models.Model ):
    event = models.ForeignKey( Event, on_delete = models.CASCADE )
    # ...
```
Norėdami apskaičiuoti į renginį prisiregistravusių vartotojų skaičių, šią logiką galime pridėti į Event modelį:
```python
class Event( models.Model ):
    date = models.DateTimeField( null = True )
    title = models.CharField( max_length = 100 )
    conference = models.ForeignKey( Conference, on_delete = models.CASCADE )
    
    def count_registrations( self ):
        return self.eventregistration_set.count()
```
Funkciją `count_registration()` dabar galime naudoti savo view, o norėdami pakeisti registracijų skaičiavimo logiką (pvz norime skaičiuoti tik patvirtintas registracijas) kodą užteks keisti tik vienoje vietoje.
```python
renginys = Event.objects.get( id=1 )
renginys.count_registrations() # Suskaičiuos renginio registracijų kiekį
```
