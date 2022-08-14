```python
# Klasa zivotinje, shvati to kao skup atributa i metoda (kaj mozes koristiti da sve zivotinje opises i kaj mozes
# napraviti s tim, npr ispis svojstva)
# Takve opce klase se drze na opcih stvari. Npr. klasa auto. Pitsas se koja svojstva imaju svi auti zajdnicko
# Prema meni to bi bilo vrata a svojstvo ili atribut bi bio, broj vrata. ILi kotac, svaki auto ima kotac. Atribut
# bi bio mozda broj kotaca
# neki imaju 4, kamioni (koji isto spadaju u grupu automobila imaju nekad 6 i vise)

class Zivotinja:
    def __init__(self, naziv, tip, masa, vrsta_ishrane):
        self.naziv = naziv
        self.tip = tip
        self.masa = masa
        self.vrsta_ishrane = vrsta_ishrane

    # zadatak trazi metoda ispisa atributa(svojstva)
    # tu napravis samo metodu koja uzima te varijable koje imas i ispise ih
    def ispisi(self):
        print("-------------------------------------------")
        print(f"Naziv: {self.naziv}")
        print(f"Tip: {self.tip}")
        print(f"Masa: {self.masa}")
        print(f"Vrsta ishrane: {self.vrsta_ishrane}")
        print("-------------------------------------------")


# to je klasa, tu je kraj kaj se tice klase

# ovdje pocinje zapravo program
# prvi "instanciras" klasu. To znaci iz klase napravis objekt. Klasa je zapravo samo blueprint
pas = Zivotinja("pas", "sisavac", 15, "svejed")
# I tu koristis metodut ispisi za ispis
pas.ispisi()

vrabac = Zivotinja("vrabac", "ptica", 0.005, "biljojed")
vrabac.ispisi()
```





Drugi zadatak

```python
import string


class Obrada_Stringa:
    # parameterized constructor
    def __init__(self, string):
        self.recenica = string  # :D

    # a, svaka rijec u string
    # trazi nesto kao string to word list
    # https://stackoverflow.com/questions/6181763/converting-a-string-to-a-list-of-words
    def string_to_word_list(self):
        return self.recenica.split()

    # i sad radsi dalje metode za ostalo
    def calculate_characters_in_string(self):
        punctuation = 0
        capital_letters = sum(1 for c in self.recenica if c.isupper())
        normal_letters = sum(1 for c in self.recenica if c.islower())
        numbers = sum(1 for c in self.recenica if c.isnumeric())
        for c in self.recenica:
            if c in string.punctuation:
                punctuation += 1
        print(f"""
        Velika slova: {capital_letters}
        Mala slova: {normal_letters}
        Brojevi: {numbers}
        Posebni znakovi: {punctuation}
        """)


# ivana = input("Upisi recenicu: ")
ivana = "Ifka uci python 23 dana!!!"
recenica = Obrada_Stringa(ivana)

# za a:)
print(recenica.string_to_word_list())

# b.)
print(recenica.calculate_characters_in_string())
```

Sa brojanjem znakova

```python
def calculate_characters_in_string(self):
    punctuation = 0
    capital_letters = sum(1 for c in self.recenica if c.isupper())
    normal_letters = sum(1 for c in self.recenica if c.islower())
    numbers = sum(1 for c in self.recenica if c.isnumeric())
    for c in self.recenica:
        if c in string.punctuation:
            punctuation += 1
    print(f"""
    Velika slova: {capital_letters}
    Mala slova: {normal_letters}
    Brojevi: {numbers}
    Posebni znakovi: {punctuation}
    """)
```

Cijeli zadatak, al treba jos popravljati, ispis i te stvari

```python
import string
import random

class Obrada_Stringa:
    # parameterized constructor
    def __init__(self, sentence):
        self.recenica = sentence


    def get_word_list(self):
        return self.recenica.split()

    # a, svaka rijec u string
    # trazi nesto kao string to word list
    # https://stackoverflow.com/questions/6181763/converting-a-string-to-a-list-of-words
    def string_to_word_list(self):
        return self.get_word_list()

    # i sad radsi dalje metode za ostalo
    def calculate_characters_in_string(self):
        capital_letters = sum(1 for c in self.recenica if c.isupper())
        normal_letters = sum(1 for c in self.recenica if c.islower())
        numbers = sum(1 for c in self.recenica if c.isnumeric())
        punctuation = sum(1 for c in self.recenica if c in string.punctuation)
        print(f"""
        Velika slova: {capital_letters}
        Mala slova: {normal_letters}
        Brojevi: {numbers}
        Posebni znakovi: {punctuation}
        """)

    def get_numbers_from_word_list(self):
        brojevi = []
        for word in self.get_word_list():
            if word.isnumeric():
                brojevi.append(word)
        return brojevi

    def pretvorba_u_rimske_brojeve(self, num):
        # Storing roman values of digits from 0-9
        # when placed at different places
        m = ["", "M", "MM", "MMM"]
        c = ["", "C", "CC", "CCC", "CD", "D",
             "DC", "DCC", "DCCC", "CM "]
        x = ["", "X", "XX", "XXX", "XL", "L",
             "LX", "LXX", "LXXX", "XC"]
        i = ["", "I", "II", "III", "IV", "V",
             "VI", "VII", "VIII", "IX"]

        # Converting to roman
        thousands = m[num // 1000]
        hundreds = c[(num % 1000) // 100]
        tens = x[(num % 100) // 10]
        ones = i[num % 10]

        ans = (thousands + hundreds +
               tens + ones)

        return ans

    def unazad_rijeci(self):
        for rijec in self.get_word_list():
            print(f"{rijec[::-1]} ", end='')

    def preoblikuj_unos(self):
        characters = []
        for char in self.recenica:
            characters.append(char.upper())

        random.shuffle(characters)
        print(''.join(characters))

    def filtriraj(self, izbor):
        characters = []
        if izbor in ["b", "B"]:
            for char in self.recenica:
                if char.isalpha() or char == ' ':
                    characters.append(char)
        elif izbor in ["s", "S"]:
            for char in self.recenica:
                if char.isnumeric() or char == ' ':
                    characters.append(char)
        return characters
        

# ivana = input("Upisi recenicu: ")
ivana = "Ifka uci python 23 dana po 6 sati !!!"
recenica = Obrada_Stringa(ivana)

# za a:)
print(recenica.string_to_word_list())

# b.)
recenica.calculate_characters_in_string()

# c.)
for broj in recenica.get_numbers_from_word_list():
    print(recenica.pretvorba_u_rimske_brojeve(int(broj)))

# d.)
recenica.unazad_rijeci()

# e.)
recenica.preoblikuj_unos()

izbor = input("Zelis filtrirat brojeve ili slova ? Za brojeve upisi 'b' a za slova 's': ")
if izbor not in ["s", "S", "b", "B"]:
    print("Krivi unos.")
else:
    if izbor in ["s","S"]:
        print(f"Unos sa filtrirajim slovima je: {''.join(recenica.filtriraj(izbor))}")
    else:
        print(f"Unos sa filtrirajim brojevima je: {''.join(recenica.filtriraj(izbor))}")
```

