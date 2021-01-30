# PP1 Projekat

Dosta ljudi me je pitalo kako da krene kako da radi projekat iz PP1. Da se ne bih ponavljala i da bi još neko mogao da pročita, ako mu nešto znači, kuckam sada to sve ovde. 

## Nivoi projekta
Kao što sigurno već znate projekat je moguće uraditi za tri različita nivoa, A, B i C. 
A je najmanji, C najobimniji. 
Razlika u nivoima je prvenstveno u količini koda koju je potrebno iskucati. 

* A nivo je _"go"_. Alokacija lokalnih i globalnih promenljivih, nizovi, samo main funkcija, izrazi, dodele, print/read. I to bi bilo manje-više to. 

* B nivo je već malo interesantniji. Tu su i druge funkcije, imamo if-else, do-while petlju, kao i && i ||.

* C nivo ove godine sadrži switch i klasično, klase. 

## Leksička analiza
* Kucka se **mjlexer.flex**
* JFlex alat generiše klasu **Yylex.java**
* Koristi se pomoćna klasa **sym.java**. Možete da je iskucate sami malo da bi videli kako to radi, ali nema potrebe da kucate nju za sve reči, jer će kasnije alat sintaksne analize to sam generisati.
* Irelevantno je koji nivo radite, podjednako malo truda je potrebno.

Leksička analiza je u projektu mahom odrađena. "Komplikovan" deo koji podrazumeva šta gde treba kucati unutar .flex fajla postoji odrađen unutar tutorijala.
Tako da uzmete taj kod, dodate još par _reči_ koje je potrebno registrovati i to je to. 
Samo pazite da vam registracija identifikatora bude na kraju, a ključne reči pre toga, da se ne desi da vam se npr. if protumači kao neki naziv. 
Takođe, postoji greška baš kod identifikatora. 
Ispravno je ***([a-z]|[A-Z])[a-zA-Z0-9_]\**** tj. ne stavlja se | unutar [] jer ga tu tretira ako simbol, 
pa ako napišete a||b neće protumačiti kao a ili b, nego kao jedan identifikator sa nazivom "a||b". 
Tako da to promenite. 

## Sintaksna analiza
* Kuca se **mjparse.cup** 
* Ast_cup generiše **MJParser.java** i **sym.java**

Pogledajte tutorijal, da vidite šta gde kako ide. 
Onaj "omot" deo možete da iskopirate. Ali što se tiče kucanja pravila, nemojte nadograđivati ono rešenje, nego kucajte od nule, a u onom kodu tražite ideje kad vam budu bile potrebne.
Sve što dodate odmah testirajte. 
Ako ste zaboravili ; i znate šta ste poslednje menjali, mnogo ćete lakše naći grešku nego ako ste menjali više linija koda koje su idejno ispravne. 
Nije dobar feedback u tom pogledu. 

#### Greška zbog konflikta?
Ako do greške dođe zbog nekog konflikta, alat će vam mnogo fino pokazati gde se desio konflikt. 
Da biste razumeli šta mu smeta i kako to da popravite, potrebno je da razumete kako radi LALR(1) parser. 
Ako znate princip LALR(1) i alat vam je rekao gde je došlo do konflikta, 
sledeće što treba da uradite jeste da pogledate svoj kod i nađete kojim putem kad se ide parser ne zna jednoznačno šta da radi. 
Najčešće je t








