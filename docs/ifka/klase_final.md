Ovo je gotov program, sa formatiranim ispisom da na nesto izglea, te sa komentarima za tebe



```python
# moramo importirati ovaj radnom jer cemo trebat funkcije iz te klase kasnije kada budemo mijesali listu
import random


# ovo je funkcija convert_to_roman. Vidjet ces da je izvan klase. Razlog tome je logicke naravi. Kad gledas ta
# konverzija i nema bas veze sa klasom, tj. me koristi nista iz klase (nikakvi self). TO je znak da se moze koristiti
# i izvan klase, za bilo koji broj, bez obzira na objekt. Zato je izvan klase.
# Drugi nacin, da bi koristila to bilo kad i bilo gdje, metodu u klasi mozes definirati kao staticno.
# pycharm je preorucio da bude izvan klase, pa evo je izvan
# Jos malo o definicijama. Metoda i funkcija su jedno te isto. Razlika je jedina da funkciju u nekoj klasi zoves metoda
# a funkciju koja nije u klasi, funkcija
def convert_to_roman(num):
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


class Obrada_Stringa:
    # parameterized constructor
    def __init__(self, string):
        # Konstruktor, ova metoda se izvrsava svaki puta kada instanciras now objekt sa npr. recenica = Obrada_Stringa(unos)
        # posto imas pored self, koji je obavezan uvijek, i string varijabli, kada instanciras objekt uvijek moras
        # sa nekim unosom to instancirati

        # ovdje dodajemo varijable koje ce biti poznate samo unutar klase. Znaci ovo self.string znaci to je varijabla
        # string koja je poznata samo toj klasi i nikome drugom.
        self.string = string
        # ovo je lista posebni znakova, znaci ono kaj u zadatku trazi da prebrojis posebne znakove. Ako je znak u toj
        # listi onda je to posebni znak
        self.punctuation = ['!', '"', '#', '$', '%', '&', '\'', '(', ')', '*', '+', ',', '-', '.', '/', ':', ';', '<',
                            '=', '>', '?', '@', '[', '\\', ']', '^', '_', '`', '{', '|', '}', '~']

    # prvo napravimo ovu metodu jer cemo to trebati cesto, mozes jednostavno koristiti i self.string.split.
    def get_word_list(self):
        return self.string.split()

    # a.) svaku rijec u listu
    # trazi nesto kao string to word list
    # https://stackoverflow.com/questions/6181763/converting-a-string-to-a-list-of-words
    # ovdje umjesto ove metode get_word_list mozes jednostavno koristiti self.string.split(). AL evo ja sam koristio
    # metodu
    def string_to_word_list(self):
        return self.get_word_list()

    # b.) broj velikih i malih slova, brojeva i znakova
    # ovu sam metodu malo promijenio tako da ona sama nista ne ispisuje vec vraza jedan dictionary sa vrijednostima
    # pogledaj si sto je dictionary ako si zaboravila ali u biti imas neki naziv pa onda dvotocka i vrijednost koja
    # pripada tom naziv. Kad dobijes taj dictionary onda ne pristupas podatcima sa indexom (npr list[0]) nego sa imenon
    # npr. lista['capital_letters']
    def calculate_characters_in_string(self):
        string_calculation = {'capital_letters': sum(1 for c in self.string if c.isupper()),
                              'normal_letters': sum(1 for c in self.string if c.islower()),
                              'numbers': sum(1 for c in self.string if c.isnumeric()),
                              'punctuation': sum(1 for c in self.string if c in self.punctuation)}
        return string_calculation

    # metoda ide kroz svaku rijec i provjerava dal je broj (isnumerica)
    # ako je doda je u listu i to vraca kao rezultat
    def get_numbers_from_word_list(self):
        brojevi = []
        for word in self.get_word_list():
            if word.isnumeric():
                brojevi.append(word)
        return brojevi

    # metoda opet prolazi kroz sve rijeci i "okrece" ih te ih stavlja u listu "unazad" i tu listu vraca kao rezultat
    def unazad_rijeci(self):
        unazad = []
        for rijec in self.get_word_list():
            unazad.append(rijec[::-1])
        return unazad

    # mislim da ovaj dio nije dobro postavljen jer uppercase cijelog stringa i kapitalizacija svakog slova je jedno te
    # ist0. Kak god, metoda prvo ide preko svakog znaka (i preko space-a " " i napravi iz njega veliko slovo
    # nakon toga "pomijesa" listu i vraca jedan string koji gdje su svi znakovi spojeni
    # to spajannje u jedan string smo mogli napraviti i kod ispisa.
    def preoblikuj_unos(self):
        characters = []
        for char in self.string:
            characters.append(char.upper())
        random.shuffle(characters)
        return ''.join(characters)

    # ova metoda ima jedan dodatan argument izbor. izbor je znak koji se upise kad te program pita dal zelis
    # brojke ili slova (b ili s)
    # ako je izbor bio b (veliko ili malo b) petlja prolazi kroz svaki znak i ako je znak dio abecede dodaje ga u listu
    # characters. Ako je izbor bio s, onda to isto radi samo provjerava dal je znak numericki i ako je dodaje ga u listu
    # na kraju vraca list characters
    def filtriraj(self, izbor):
        characters = []
        if izbor in ["b", "B"]:
            for char in self.string:
                if char.isalpha() or char == ' ':
                    characters.append(char)
        elif izbor in ["s", "S"]:
            for char in self.string:
                if char.isnumeric() or char == ' ':
                    characters.append(char)
        return characters


# Ovdje instanciramo objekt. Znaci iz klase Obrada_Stringa, kaj je blueprint (koja svojstva ce imati objekt,
# koje metode itd), stvorimo, instanciramo objekt ivana. objekt ivana moze imati ime koje hoces. I posto smo u
# konstruktoru definirali da mora imati neki string (u zagradi), neki string je obavezan ( U ovom slcaju "Ifka uc...")
# Ispod maknes komentar da bi te pitao za recenicu. Ja sam to komentirao da imam neku recenicu spremno i da ne trebam ]
# stalno nesto upisivati. A recenica je takva da ima i velika i mala slova, posebne znakove i brojke.
# ivana = input("Upisi recenicu: ")
ivana = "Ifka uci python 23 dana po 6 sati !!!"
recenica = Obrada_Stringa(ivana)

# za a:)
# pozivas metodu klase/objekta. TO radis tak ta iza objekta ivana dodas tocku i onda metodu(funkciju)
# pycharm automatski izlista koje metode mozes dodati tj koje su vidljive. Vjerojatno i vscode
print(f"""
    a.) Lista rijeci, brojeva i posebnih znakova:
        {recenica.string_to_word_list()}
    """)

# b.)
# objasnio sam u metodi kako pristupas vrijednostima, znaci ne indexom nego imenom
print(f"""
    b.) Analiza znakova u recenici:
        Velika slova :  {recenica.calculate_characters_in_string()['capital_letters']}
        Mala slova :    {recenica.calculate_characters_in_string()['normal_letters']}
        Brojevi :       {recenica.calculate_characters_in_string()['numbers']}
        Znakovi :       {recenica.calculate_characters_in_string()['punctuation']}
    """)

# c.)
# ovo je vise formatiranje ispis nego bilo kaj drugo.
# Prvo ispisujemo zaglavlje i nakon toga sa for petljom vadimo brojeve iz objekta,recenice i za svaki izvadjeni
# decimalni broj pozovemo funkciju convert_to_roman. Bitno je da broj bude int
print("    c.) Pronadjeni brojevi u recenici")
for broj in recenica.get_numbers_from_word_list():
    print(f"         Broj: {broj} - Rimski ekvivalent: {convert_to_roman(int(broj))}")

# d.)
# Ovo si ti zapravo sama rijesila pa sve znas
print(f"""
    d.) Rijeci napisane unazadno:
        {recenica.unazad_rijeci()}
        Recenica ispisano unazad:
        {' '.join(recenica.unazad_rijeci())}
""")

# e.)
print(f"""
    e.) Preoblikovan unos (sve rijeci uppercase, spajane stringa i mijesanje slova:
        {recenica.preoblikuj_unos()}
""")

# f.)
# program pita dal zelis filtrirati brojeve ili slova i bilo koje drugo slovo nego s ili b prekida program
# ako si upisala b ili s u drugom if- provjerava koje si tocno slovo upisala i shodno tome poziva metodu "filtriaj"
# sa odgovarajucim parametrom (izbor)
izbor = input("Zelis filtrirat brojeve ili slova ? Za brojeve upisi 'b' a za slova 's': ")
if izbor not in ["s", "S", "b", "B"]:
    print("Krivi unos.")
else:
    if izbor in ["s", "S"]:
        print(f"Unos sa filtrirajim slovima je: {''.join(recenica.filtriraj(izbor))}")
    else:
        print(f"Unos sa filtrirajim brojevima je: {''.join(recenica.filtriraj(izbor))}")

```

Ispis:

```
/usr/bin/python3.8 "/home/bonzo/code/python/the_python_workbook/01 Introduction to Programming/Zivotinje.py"

    a.) Lista rijeci, brojeva i posebnih znakova:
        ['Ifka', 'uci', 'python', '23', 'dana', 'po', '6', 'sati', '!!!']
    

    b.) Analiza znakova u recenici:
        Velika slova :  1
        Mala slova :    22
        Brojevi :       3
        Znakovi :       3
    
    c.) Pronadjeni brojevi u recenici
         Broj: 23 - Rimski ekvivalent: XXIII
         Broj: 6 - Rimski ekvivalent: VI

    d.) Rijeci napisane unazadno:
        ['akfI', 'icu', 'nohtyp', '32', 'anad', 'op', '6', 'itas', '!!!']
        Recenica ispisano unazad:
        akfI icu nohtyp 32 anad op 6 itas !!!


    e.) Preoblikovan unos (sve rijeci uppercase, spajane stringa i mijesanje slova:
        D!3 O!INAA !  IA6 YNIH2FS OT UCTAP KP

Zelis filtrirat brojeve ili slova ? Za brojeve upisi 'b' a za slova 's': b
Unos sa filtrirajim brojevima je: Ifka uci python  dana po  sati 

Process finished with exit code 0
```

 