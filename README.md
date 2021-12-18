# **PP1 Projekat** - *Saveti i pomoć za izradu projekta iz Programskih prevodioca 1*

Pokušala da izvučem sve što je potrebno za izradu projekta iz PP1. Za svaku fazu možete naći šta je sve potrebno pogledati pre izrade faze (ne morate poznavati sve sa kursa, već samo par konkretnih lekcija), razjašnjenja nekih stvari koje nisu dovoljno pojašnjene u zvaničnim materijalima, kao i neki saveti kako prići samoj izradi. Takođe, tu su i neke greške koje se mogu javiti, zašto se javljaju i kako ih otkloniti.  
*Srećna rad!*

## Nivoi projekta
**A ⊂ B ⊂ C** <br/>
Razlika u nivoima je prvenstveno u količini koda koju je potrebno iskucati. 

* A nivo - alokacija lokalnih i globalnih promenljivih, nizovi, samo main funkcija, izrazi, dodele, print/read.
* B nivo - funkcije, nizovi, if-else, do-while petlja, && i ||, break, continue.
* C nivo - switch (ove godine), kao i klase i sve što ide sa njima. 

#### Kada i kako izabrati nivo? (A/B/C)
* Najbolje je u startu izabrati nivo za koji ćete da radite. 
Implementacije će biti skroz drugačije zavisnosti od toga koliko različitih stvari treba da obezbedite da rade. 
Možete i kasnije da se predomislite da radite za viši, ali u tom slučaju budite spremi da ćete menjati veliki deo već napravljenog koda. 
* Ako niste u frci sa vremenom i imate dan/dva/tri "viška" uzmite da radite barem za **B** nivo. Za A nivo je dosta stvari odrađeno na tutorijalima. To zvuči primamljivo, ali može se desiti da se kasnije na odbrani ne snađete dovoljno brzo i precizno. Razlog tome nije zato što niste radili sami, nego zato što radeći za A nivo ne provežbate dovoljno neke stvari, posebno skokove. Dok sa druge strane, ako ste napravili lepo uslove i petlje, kakvu god modifikaciju da vam daju, veća je šansa da pre smislite kako nešto da uradite i da to napišete da radi u svakom scenariju.  

## Faze projekta
Postoje 4 faze projekta. Svaka naredna se nadovezuje na prethodnu u nekom obliku, ali nije neophodno znati buduće faze za izradu trenutne. 
Tako da, fazu po fazu. Nema potrebe da sve shvatite i naučite pre nego što krenete da radite. 
Takođe, dosta toga ćete i učiti kroz projekat, to i jeste cilj.  

## Izgled mj programa i traženi zahtevi 
Zvaničan tekst projekta pročitajte da biste stekli okvirnu ideju čemu projekat. A da biste znali šta konkretno treba da radite postoji jedan drugi dokument - specifikacija mikrojave (2. fajl sa sajta). Već na drugoj strani tog fajla možete videti jedan primer zamišljenog jezika. Na dalje ovaj fajl ćete najviše koristiti da biste znali šta sve treba da radi i kako. 

U ovaj repozitorijum dodala sam i **build.xml** fajl sa svim fazama lepo uvezanim.  

# Leksička analiza
* Bildujete **lexerGen** iz build.xml fajla
* Kucka se **mjlexer.flex**
* JFlex alat generiše klasu **Yylex.java**
* Koristi se pomoćna klasa **sym.java**. Možete da je iskucate sami malo da bi videli kako to radi, ali nema potrebe da kucate nju za sve reči, jer će kasnije alat sintaksne analize to sam generisati.
* Irelevantno je koji nivo radite, podjednako malo truda je potrebno.

### Potrebno predznanje
* Manje-više je sve intuitivno.
* Pogledati tutorijal i podesiti okruženje.
* Unutar Eclipse šablona sa sajtu neki jar fajlovi su zastareli, pa ili ih zamenite sve u startu sa najnovijim verzijama sa sajta, ili kasnije ako se desi neka greška probajte da se setite da je to možda problem.  

Leksička analiza je mahom odrađena. "Komplikovan" deo koji podrazumeva šta gde treba kucati unutar .flex fajla postoji odrađen unutar tutorijala.
Tako da uzmite taj kod, dodajte još par _reči_ koje je potrebno registrovati (nabrojane su u specifikaciji) i to je to. 
Samo pazite da vam registracija identifikatora bude na kraju, a ključne reči pre toga, da se ne desi da vam se npr. *if* protumači kao neki naziv promenljive. 
Takođe, postoji greška baš kod identifikatora. 
Ispravno je 
```([a-z]|[A-Z])[a-zA-Z0-9_]```tj. ne stavlja se | unutar [] jer ga tu tretira ako simbol, 
pa ako napišete a||b neće protumačiti kao a ili b, nego kao jedan identifikator sa nazivom "a||b". 
Tako da to promenite. 
<br/> <br/>

# Sintaksna analiza
* Bildujete **compile** iz build.xml fajla (svaki put odradi i leksičku analizu, u sulučaju da se nešto naknadno menjalo tamo, da ne bude ju što puca)
* Kuca se **mjparse.cup** 
* Ast_cup generiše **MJParser.java** i **sym.java**

### Potrebno predznanje
* Treba da razumete, barem idejno, kako radi LALR(1) parser. Značiće vam za razrešavanje konflikata. 

Pogledajte tutorijal, da vidite šta gde kako ide. 
Onaj "omot" deo možete da prekopirate. Ali što se tiče kucanja pravila, ***nemojte nadograđivati njihovo rešenje***, nego kucajte od nule, a u onom kodu tražite ideje kad vam budu bile potrebne.
Sve što dodate odmah testirajte. 
Ako ste zaboravili **;** i znate šta ste poslednje menjali, mnogo ćete lakše naći grešku nego ako ste menjali više linija koda koje su idejno ispravne. 
Nije dobar feedback u tom pogledu. 

## Kako početi? 
Navedite sve terminale koje ste koristili u .flex fajlu. Ako ste slučajno i '=' i '==' nazvali sa EQUAL, javiće vam se neke lude greške, tako da pazite da ništa niste duplirali. Pored toga napravite početni neterminal *Program* i definišite ga kao ```Program ::= (Program) PROG;```  i tesitrajte da to radi. Ako to sve radi, možete krenuti sa izradom pravila.  
Dalje, sebi napišite primer po primer u program.mj i implementirajte tako da vam to prolazi. Npr.  
```
program mojProgram
```
```
program mojProgram
{
}
```
```
program mojProgram
int a;
{
}
```
```
program mojProgram
int a, b, c;
{
}
```
itd. Za svaku sitnicu, napišite primer koji želite da vam radi, izmenite pravila i testirajte. I tako ćete bez stresa razviti svoje rešenje.  
Specifikaciju držite uz sebe da biste znali šta sve mj program treba da podrži. 

## Kako davati imena klasama unutar zagrada? 
O tome šta su ove klase i čemu služe, više u semantičkoj analizi. Za sada je dovoljno da se držite "pravila" - ako postoji samo jedna smena, onda se desna klasa može nazvati isto kao i levi neterminal. U suprotnom, nijedna klasa smene ne sme imati isto ime kao levi neterminal. 

## Dodatni atributi kod terminala/neterminala
Na tutorijalima je pisano nešto tipa ```terminal Integer ...;``` ili ```nonterminal rs.etf.pp1.symboltable.concepts.Obj ili .Struct ...;``` i verovatno nemate pojma čemu sve to i gde staviti, a gde ne. I ne treba da znate, kao što ni ne treba da pišete te stvari tokom izrade 2. faze. Kasnije u 3. i 4. fazi će vam nekada zatrebati da iskoristite te opcije, ali to ćete tada lako da dodate za konkretne (ne)terminale. 

## Greška zbog konflikta?
Ako do greške dođe zbog nekog konflikta, alat će vam veoma fino pokazati gde se desio konflikt. Da biste razumeli šta mu smeta i kako to da popravite, potrebno je da razumete kako radi LALR(1) parser. Ako znate princip LALR(1) i alat vam je rekao gde je došlo do konflikta, sledeće što treba da uradite jeste da pogledate svoj kod i nađete problematičan put parsera kojim kad se ide on ne zna jednoznačno šta treba da radi.  
Najčešće je to problem sa epislon delovima. Npr.
```
A = B | ε
B = 'x' | ε
```
U slučaju da se pojavi prazna sekvenca parser neće znati da li da prevede kao A po drugoj smeni ili kao A po prvoj pa B po drugoj. Tj. desiće se shift/reduce konflikt. Ovo se rešava tako što se opredelite za jednu od ove dve epsilon smene. Nešto ovako verovatno nećete svesno napisati, ali vam se može desiti tokom nadograđivanja postojećeg rešenja.  
*Ako se setim još nekih grešaka, dodaću ih ovde.*


## Problem ternarnog operatora (2020/2021)
Najprostije rečeno, ne znaju se prioriteti. Ako bismo napisali ```a > b ? x : y``` na osnovu date gramatike parser ne zna da li da prevede kao ```(a > b) ? x : y``` ili kao ```a > (b ? x : y)```

### Rešenje
**I način** - Condition je većeg prioriteta. Ovako obezbeđujete i ugnježdavanje ternarnih operatora. 
```
CondFact    = ExprNonTern [Relop ExprNonTern]
Expr        = ExprNonTern 
            | ExprTern
ExprNonTern = ["-"] Term {Addop Term}
ExprTern    = Condition "?" Expr ":" Expr
```
**II način** - Ternarni operator je većeg prioriteta. 
```
CondtFact   = Expr [Relop Expr]
Expr        = ExprNonTern 
            | ExprTern
ExprNonTern = ["-"] Term {Addop Term}
ExprTern    = Condition "?" ExprNonTern ":" ExprNonTern 
```

<br/> 

# Semantička analiza
* Pokreće se java test koji je napisan.
* Paravi se **SemanticAnalyzer.java** koji se izvodi iz VisitorAdapter (data klasa). Ovaj fajl će, zavisnosti od nivoa, biti veliki do ogroman. Metode nisu toliko velike, ali ih ima mnogo. Potrudite se da budete uredni, da lepo grupišete metode i dajete fine nazive promenljivama. 

### Potrebno predznanje: 
* Sintetizovani atributi (**vežbe**)
* Tabela simbola (**vežbe**, primeri su podeljeni po nivoima)
* *(Eventualno probajte da dođete do Žižinih snimaka iz 2020. i za ovu i za sledeću fazu)*

Ponovo, tutorijale gledajte više informativno, a svoj kod je bolje da kucate od nule. Ako smatrate da je neki deo koda njihovog rešenja višak, bolje probajte bez toga. Kasnije ako se ispostavi da vam za nešto drugo ipak treba, tek onda dodajte. 

## Klase iz sintaksne analize
 ```
 S  = (S1) X a Y
    | (S2) Z b;
 ```
* Naziv neterminala sa leve strane smene je naziv generisane natklase. ```S```
* Nazivi klasa desne strane smene su izgenerisani kao izvedena klase iz leve. ```S1 i S2```
* Svaka klasa ima atribute koji su reference na objekte svojih neterminalnih "delova". S1 ima reference na jedan X i jedan Y objekat. S2 ima referencu na Z objekat. 
* Moguće je dodati još atributa. Obično će vam trebati ili jedan Obj ili jedan Struct objekat. Koji ćete koristiti zavisi od toga šta vam treba. To ostavljam na vama da razmišljate koji vam kada treba. 

Savet: Na jednom listu papira popišite koje atribute ima jedan Obj čvor, koje vrednosti svaki od tih atributa ima, šta predstavljaju i koji tipovi ih koriste. Isto i za Struct čvorove. Biće vam mnogo lakše nego da pamtite, ili svaki put tražite po dokumentaciji.  

## Kako koristiti Obj i Struct objekte
Nakon sintaksne analize, vama se izgenerisalo apstraktno sintaksno stablo (AST). Sada u semantičkoj analizi mi pomoću Visitor-a obilazimo čvor po čvor tog stabla po postorder redosledu. Mi ćemo informacije o konkretnim tipovima i nazivima promeljivih naći tek u listovima. Da bismo to sve uvezali i proveravali/koristili te informacije i u nekim višim unutrašnjim čvorovima, potrebno je da određene informacije ispopagiramo na gore (*sintetizovani atributi*). Naš sintetizovan atribut će obično biti Obj ili Struct tipa (predefinisani tipovi).  
Sada dolazi trenutak kada u .cup fajlu kod neterminala treba da dodate ...Obj ili ...Struct delove za neke neterminale. Time će određena Java klasa konkretnog neterminala imati polje .obj ili .struct kao moguću referencu na konkretan objekat. Paziti da su sve te reference na počentku null.  
Nema potrebe uvek koristiti Obj atribute, nekada vam je Struct objekat dovoljan da prenese sve neophodne informacije. 


## Greške
Jako lako može da se desi da ne "uvežete" sve atribute. Npr. 
```
A = B x    {a.struct = a.b.struct}
B = C y    {}        // ne generišete ništa, jer u ovom koraku nije potrebno izvršavati nikakve provere
C = z      {c.struct = new Struct(c.z)}      
```
U visit(C) registrujete neku promenljivu/tip i izgenerišete odgovarajući objekat. U A imate sve potrebne elemente da proverite da li se npr. tipovi poklapaju. 
Međutim kako u visit(B) nije rečeno ```b.struct = b.c.struct``` u visit(A) stiže ```null``` i program ne radi lepo. Ovo se lako uoči kada se debaguje.  

Pazite da ne kucate visit metodu od natklase, jer se ona nikada neće obići (osim ako niko nije izveden iz nje).  

## Metoda getLine() nekada vraća broj kolone umesto broja linije 
To je bug u alatu. Provereno neće nikome zamerati. Ako ste baš željni, možete da probate da nabudžite da vam se ispisuje broj linije od nekog drugog elementa, ali nema potrebe za tim. Nadam se da niste već izgubili dane tražeći šta je problem. Ali i ako jeste, barem ste naučili šta kako radi u pozadini. 

<br/> 

# Generisanje koda *- doradiće se uskoro :)*
* Pravite **CodeGenerator.java** tj. još jedog vizitora, samo što se sada ne bavite tabelom simbola, nego generisanjem koda.
* Svaki put pokrećete Java test kako bi se obradile sve 4 faza nad našim mj programom.
* Pokretanjem **runObj** iz build.xml fajla ispisuje se izgenerisani kod. Pregledno je. 
* Pokretanjem Run klase iz **mj-runtime.jar** ispisuje se kod kroz koji ide program tokom izvršavanja (ako ima neka petlja više puta će prolaziti korz taj kod).
Run mora imati lokaciju ```test\program.obj``` (objekti fajl izgenerisanog koda), a fina opcija je dodati ```-debug``` čime će nakon svake izvršene instrukcije biti ispisan trenutni stek. Dobra praksa je da kada vam proradi neki deo proverite da stek ostaje prazan nakon svakog logičkog seta instrukcija.


### Potrebno predznanje
* MJVM (vežbe, primeri su podeljeni po nivoima)

Ova faza najduže traje, ali se ne sećam da su se javljali neki veći problemi. Bitno je da dobro shvatite kako rade instrukcije koje vam trebaju. Šta su im ograničenja. Kako ih pozivate. Dosta ideja se pokupi na primerima na vežbama. Za B nivo, osmislite prvo na papiru kakav ples skokova ćete praviti da biste lepo obradili uslove, kasnije se to lako ukuca. Za C nivo, kada se dođe do pozivanja atributa, metoda, generisanje tabele virtuelnih funkcija, lepo ispratite na vežbama šta se sve i kojim redosledom pakuje na stek i nećete imati nikakvih problema! 
