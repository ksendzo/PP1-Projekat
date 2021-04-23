# PP1 Projekat

## Nivoi projekta
Kao što sigurno već znate projekat je moguće uraditi za tri različita nivoa, A, B i C. 
A je najmanji, C najobimniji. 
Razlika u nivoima je prvenstveno u količini koda koju je potrebno iskucati. 

* A nivo je _"go"_. Alokacija lokalnih i globalnih promenljivih, nizovi, samo main funkcija, izrazi, dodele, print/read. I to bi bilo manje-više to. 
* B nivo je već malo interesantniji. Tu su i druge funkcije, imamo if-else, do-while petlju, kao i && i ||.
* C nivo ove godine sadrži switch i klasično, klase. 

#### Kada i kako izabrati nivo? (A/B/C)
* Najbolje je u startu izabrati nivo za koji ćete da radite. 
Implementacije će biti skroz drugačije zavisnosti od toga koliko različitih stvari treba da obezbedite da rade. 
Možete i kasnije da se predomislite da radite za viši, ali u tom slučaju budite spremi da ćete menjati veliki deo već napravljenog koda. 
* Ako niste u frci sa vremenom i imate dan/dva/tri "viška" uzmite da radite barem za **B** nivo. Za A nivo je dosta stvari odrađeno na tutorijalima. To zvuči primamljivo, ali može se desiti da se kasnije na odbrani ne snađete dovoljno brzo i precizno. Razlog tome nije zato što niste radili sami, nego zato što radeći za A nivo ne provežbate dovoljno neke stvari, posebno skokove. Dok sa druge strane, ako ste napravili lepo uslove i petlje, kakvu god modifikaciju da vam daju, veća je šansa da pre smislite kako nešto da uradite i da to napišete da radi u svakom scenariju. 


## Faze projekta
Postoje 4 faze projekta. Svaka naredna se nadovezuje na prethodnu u nekom obliku, ali nije neophodno znati buduće faze za izradu trenutne. 
Tako da, fazu po fazu. Nema potrebe da sve shvatite i naučite pre nego što krenete da radite. 
Takođe, dosta toga ćete i učiti kroz projekat, to i jeste cilj.

### Leksička analiza
* Pokreće se pomoću lexerGen iz build.xml fajla
* Kucka se **mjlexer.flex**
* JFlex alat generiše klasu **Yylex.java**
* Koristi se pomoćna klasa **sym.java**. Možete da je iskucate sami malo da bi videli kako to radi, ali nema potrebe da kucate nju za sve reči, jer će kasnije alat sintaksne analize to sam generisati.
* Irelevantno je koji nivo radite, podjednako malo truda je potrebno.

Leksička analiza je u projektu mahom odrađena. "Komplikovan" deo koji podrazumeva šta gde treba kucati unutar .flex fajla postoji odrađen unutar tutorijala.
Tako da uzmete taj kod, dodate još par _reči_ koje je potrebno registrovati i to je to. 
Samo pazite da vam registracija identifikatora bude na kraju, a ključne reči pre toga, da se ne desi da vam se npr. *if* protumači kao neki naziv promenljive. 
Takođe, postoji greška baš kod identifikatora. 
Ispravno je 
```([a-z]|[A-Z])[a-zA-Z0-9_]```tj. ne stavlja se | unutar [] jer ga tu tretira ako simbol, 
pa ako napišete a||b neće protumačiti kao a ili b, nego kao jedan identifikator sa nazivom "a||b". 
Tako da to promenite. 

### Sintaksna analiza
* Pokreće se pomoću compile iz build.xml fajla (svaki put odradi i leksičku analizu, u sulučaju da se nešto naknadno menjalo tamo, da ne bude ju što puca)
* Kuca se **mjparse.cup** 
* Ast_cup generiše **MJParser.java** i **sym.java**

Pogledajte tutorijal, da vidite šta gde kako ide. 
Onaj "omot" deo možete da prekopirate. Ali što se tiče kucanja pravila, ***nemojte nadograđivati njihovo rešenje***, nego kucajte od nule, a u onom kodu tražite ideje kad vam budu bile potrebne.
Sve što dodate odmah testirajte. 
Ako ste zaboravili **;** i znate šta ste poslednje menjali, mnogo ćete lakše naći grešku nego ako ste menjali više linija koda koje su idejno ispravne. 
Nije dobar feedback u tom pogledu. 

#### Greška zbog konflikta?
Ako do greške dođe zbog nekog konflikta, alat će vam mnogo fino pokazati gde se desio konflikt. 
Da biste razumeli šta mu smeta i kako to da popravite, potrebno je da razumete kako radi LALR(1) parser. 
Ako znate princip LALR(1) i alat vam je rekao gde je došlo do konflikta, 
sledeće što treba da uradite jeste da pogledate svoj kod i nađete kojim putem kad se ide parser ne zna jednoznačno šta da radi. 
Najčešće je to problem sa epislon delovima. Npr.
```
A = B | ε
B = 'x' | ε
```
U slučaju da se pojavi prazna sekvenca on neće znati da li da prevede kao A po drugoj smeni ili kao A po prvoj pa B po drugoj. Tj. shift/reduce konflikt. 

#### Kako davati imena klasama unutar zagrada? 
Neterminal sa leve strane se ponaša kao natklasa desnim smenama. Svaka smena ima svoju klasu. Ako postoji samo jedna smena, onda se desna klasa može nazvati isto kao i neterminal. U suprotnom, nijedna klasa smene ne sme imati isto ime kao levi neterminal. Kada budete radili semantičku analizu, shvatićete lepo šta će nam sve to, a za sada samo da znate kako lepo da dajete imena, a da se lepo sve prevede. 

#### Problem ternarnog operatora (2020/2021)?
Najprostije rečeno, ne znaju se prioriteti. Ako bismo napisali 
```
a > b ? x : y 
``` 
na osnovu date gramatike parser ne zna koju od ove dve grupacije da iskoristi. 
```
(a > b) ? x : y 
a > (b ? x : y)
```
##### Rešenje
* Condition je većeg prioriteta. Ovako obezbeđujete i ugnježdavanje ternarnih operatora. 
```
CondFact    = ExprNonTern [Relop ExprNonTern]
Expr        = ExprNonTern 
            | ExprTern
ExprNonTern = ["-"] Term {Addop Term}
ExprTern    = Condition "?" Expr ":" Expr
```
* Ternarni operator je većeg prioriteta. 
```
CondtFact   = Expr [Relop Expr]
Expr        = ExprNonTern 
            | ExprTern
ExprNonTern = ["-"] Term {Addop Term}
ExprTern    = Condition "?" ExprNonTern ":" ExprNonTern 
```


### Semantička analiza
* Pokreće se java test koji je napisan. Paravi se SemanticAnalyzer koji se izvodi iz VisitorAdapter (data klasa). Za svaki čvor koji hoćete pišete visit metodu. Ovaj fajl će zavisnosti od nivoa biti veliki do ogroman. Metode nisu toliko velike, ali ih ima mnogo. Tako da nema mnogo pomoći. Imajte neki redosled, grupe kako ih dodajete, pišite dobra imena promenljivama kako biste se kasnije lakše snašli šta je šta. 
* E sada su od koristi one klase što smo pisali u zagradama u sintaksnoj analizi. 

#### Potrebno predznanje: 
* Sintetizovani atributi (vežbe)
* Tabela simbola (vežbe, primeri su podeljeni po nivoima)


#### Greške
Jako lako može da se desi da ne "uvežete" sve atribute. 


### Generisanje koda 
* Svaki put mora da se pokrene java test kako bi se obradile sve 4 faza nad našim mj programom
* Pokretanjem runObj iz build.xml fajla ispisuje se izgenerisani kod (pregledno da se vidi šta je izgenerisao ili gde je imao neki problem)
* Pokretanjem Run klase iz mj-runtime.jar ispisuje se kod kroz koji ide program tokom izvršavanja (ako ima neka petlja više puta će prolaziti korz taj kod).
Run mora imati lokaciju test\program.obj (objekti fajl izgenerisanog koda), a dobra opcija je dodati -debug čime će nakon svake izvršene instrukcije biti ispisan trenutni stek 
(dobro je da kada vam proradi ono što hoćete proverite da li na kraju svake "linije" koda stek ostaje prazan).

#### Potrebno predznanje
* MJVM (vežbe, primeri su podeljeni po nivoima)









