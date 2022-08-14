```python
# Kugla
# Oplosje(area, povrsina?) = O = 4 * R^2 * pi
# Obujam(volumen?) = V = (4/3) * r^3 * pi
# r = radius ili polumjer
# Valjak
# Oplosje = O =2 * r * pi * (r + h)
# Obujam = V = r^2 * pi * h
# r = radius = polumjer
# h = visina valjka
# Kvadar
# Oplosje = O = 2 * (ab + bc +ac)
# Obujam = V = a*b*c
# verzija bez apstrakne glavne klase
import math


# Ovo je takozvana apstraktna klasa, ona definira koje sve atribute i metode mora imati klasa koja se inheritira(???)
# tj nasljeduje od nje. Znaci klasa ima metode ali nema implementacije nikakve. Implementacija se radi u svakoj podklasi ]
# zasebno. Dodas samo 'pass' ispod metode i to je to
class Geometrijski_lik(object):
    def oplosje(self):
        pass

    def obujam(self):
        pass


# Svaki geometrijski like ima svoju klasu i posto se poziva na glavnu klasu "Geometrijski_lik), svaka ta klasa mora
# implementirati metode koje postoje u toj klaso
# Znaci svaka klasa (nek je primjer klasa Kugla) koju napravis sa glavnom klasom u zagradi
# (class Kugla(Geometrijski_lik):) mora implementirat metode koje postoje u klasi Geometrijski_lik a to su obujam i
# oplosje. TO je vazi i za klasu KVadar i Valjak i svaku drugu koju bi dodala
# ostatak sigurno kuzis. Jedino novo je self.__radius = radius. Ovo __radius znaci da je ta varijabla samo lokalna i ne
# moze ju mijenjat izvana sa npr. kugla.radius nego moras pozvat neku metodu da ju mijenjas naknadno.
# To se zove enkapsulacija.
# Npr imas klasu Racun_U_Banci koji ima varijablu/atribut stanje_racuna. Nebi bilo dobro da mozes direkno pristupiti toj
# varijabli jer mozes upisivati kaj hoces. Bolje je preko neke metode koja prije nego promjeni stanje, provjeri neke
# stvari dal stvarno mozes promijeniti stanje
class Kugla(Geometrijski_lik):
    def __init__(self, radius):
        self.__radius = radius

    def oplosje(self):
        return 4 * (self.__radius ** 2) * math.pi

    def obujam(self):
        return (4 / 3) * self.__radius ** 3 * math.pi


class Valjak(Geometrijski_lik):
    def __init__(self, radius, duzina):
        self.__radius = radius
        self.__duzina = duzina

    def oplosje(self):
        return 2 * self.__radius * math.pi * (self.__radius + self.__duzina)

    def obujam(self):
        return (4 / 3) * self.__radius ** 3 * math.pi


class Kvadar(Geometrijski_lik):
    def __init__(self, duzina, visina, sirina):
        self.__duzina = duzina
        self.__visina = visina
        self.__sirina = sirina

    def oplosje(self):
        return 2 * (self.__duzina * self.__visina + self.__visina * self.__sirina + self.__duzina * self.__sirina)

    def obujam(self):
        return self.__duzina * self.__visina * self.__sirina


# Ovo je kao mali meni. to je beskonacna petlja koja se vrti dok je answer true
# IZaberes broj menija i na osnovu toga izracuna podatke za krug, valjak ili kvadar (na 2 decimale)
answer = True
while answer:
    print("""
    Izracun oplosja i opsega - Izaberi geometrijski lik
    1. Krug
    2. Valjak
    3. Kvadar
    4. Izlaz
    """)
    answer = input("Izaberi opciju? ")
    if answer == "1":
        radius = float(input("Upisi radius kugle: "))
        kugla = Kugla(radius)
        print(f"""
            Oplosje kugle : {kugla.oplosje():.2f}
            Obujam kugle  : {kugla.obujam():.2f}  
        """)
    elif answer == "2":
        radius = float(input("Upisi radius valjka: "))
        duzina = float(input("Upisi duzinu valjka: "))
        valjak = Valjak(radius, duzina)
        print(f"""
            Oplosje valjka : {valjak.oplosje():.2f}
            Obujam valjka  : {valjak.obujam():.2f}  
        """)
    elif answer == "3":
        duzina = float(input("Upisi duzinu kvadra: "))
        visina = float(input("Upisi visinu kvadra: "))
        sirina = float(input("Upisi sirinu kvadra: "))
        kvadar = Kvadar(duzina, visina, sirina)
        print(f"""
            Oplosje kvadra : {kvadar.oplosje():.2f}
            Obujam kvadra  : {kvadar.obujam():.2f}  
        """)
    elif answer == "4":
        answer = False
    elif answer != "":
        print("\n Izbor nije valjan")
```