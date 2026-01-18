## Teorija
### Uvod

Web aplikacije danas predstavljaju temeljnu komponentu informacijskih sustava u gotovo svim područjima poslovanja i svakodnevnog života. Korištenjem web tehnologija omogućena je dostupnost usluga u stvarnom vremenu, neovisno o lokaciji korisnika, što donosi brojne prednosti, ali istovremeno otvara i nove sigurnosne izazove. Kako su web aplikacije javno dostupne putem interneta, one postaju izložene velikom broju potencijalnih prijetnji i napada.

Razvoj web aplikacija obilježen je sve većom složenošću – moderne aplikacije koriste različite programske okvire, baze podataka, API-je, sustave za autentifikaciju i upravljanje sesijama. Uz to, često obrađuju osjetljive podatke poput osobnih informacija korisnika, financijskih zapisa ili povjerljivih poslovnih podataka. Upravo ta kombinacija dostupnosti, složenosti i osjetljivosti podataka čini web aplikacije posebno atraktivnom metom za napadače.

Tradicionalni sigurnosni mehanizmi, kao što su mrežni vatrozidi, sustavi za detekciju i prevenciju upada te osnovne mrežne sigurnosne politike, prvenstveno su usmjereni na zaštitu mrežne infrastrukture. Iako su takvi mehanizmi nužni za cjelokupnu sigurnost informacijskog sustava, oni nisu dizajnirani za razumijevanje logike web aplikacija niti za analizu sadržaja HTTP/HTTPS prometa na razini aplikacijskog sloja. Zbog toga često nisu u stanju učinkovito spriječiti napade koji ciljaju ranjivosti same aplikacije, a ne mrežnu komunikaciju.

Dodatni problem predstavlja činjenica da se velik broj web aplikacija nalazi u produkcijskom okruženju, gdje su promjene izvornog koda složene, vremenski zahtjevne ili čak nemoguće. U takvim situacijama sigurnosne ranjivosti mogu ostati prisutne dulje vrijeme, izlažući sustav povećanom riziku od kompromitacije. Potreba za mehanizmom koji omogućuje brzu i učinkovitu zaštitu web aplikacija bez zadiranja u njihov izvorni kod postaje sve izraženija.

U tom kontekstu pojavljuje se potreba za specijaliziranim sigurnosnim rješenjima usmjerenima isključivo na zaštitu web aplikacija. Takva rješenja moraju biti sposobna analizirati aplikacijski promet, prepoznati sumnjive obrasce ponašanja i pravovremeno reagirati na potencijalne prijetnje. Osim tehničke zaštite, od suvremenih sigurnosnih mehanizama očekuje se i podrška usklađenosti s industrijskim standardima i regulatornim zahtjevima, poput zaštite osobnih i financijskih podataka.

Web Application Firewall (WAF) razvijen je upravo kao odgovor na navedene izazove sigurnosti web aplikacija. Njegova uloga nije zamjena za siguran razvoj aplikacija, već dodatni sloj obrane koji povećava otpornost sustava na napade iz vanjskog okruženja. Korištenjem WAF-a moguće je smanjiti rizik od sigurnosnih incidenata, zaštititi osjetljive podatke te osigurati stabilnost i dostupnost web aplikacija u dinamičnom i prijetnjama izloženom internetskom okruženju.

U ovom radu Web Application Firewall promatra se kao ključna komponenta sigurnosne arhitekture web aplikacija. Kroz sljedeća poglavlja detaljnije će se obraditi njegova uloga, arhitektura, vrste, funkcionalnosti i ograničenja, s ciljem pružanja jasnog i strukturiranog uvida u značaj WAF-a u suvremenoj zaštiti web aplikacija.

---

### Web Application Firewall (WAF)

Web Application Firewall (WAF) predstavlja specijalizirani sigurnosni mehanizam namijenjen zaštiti web aplikacija od prijetnji koje djeluju na aplikacijskoj razini. Za razliku od tradicionalnih mrežnih vatrozida, koji filtriraju promet na temelju IP adresa, portova i protokola, WAF je usmjeren na analizu HTTP i HTTPS komunikacije te razumije strukturu i značenje web zahtjeva i odgovora.

WAF djeluje kao posrednički sloj između klijenta i web aplikacije, pri čemu nadzire sav promet koji dolazi prema aplikaciji i koji iz nje izlazi. Tijekom tog procesa, WAF analizira elemente web komunikacije kao što su URL-ovi, HTTP zaglavlja, parametri zahtjeva, tijelo poruke, kolačići i podaci o sesiji. Na temelju unaprijed definiranih pravila i sigurnosnih politika, WAF donosi odluku hoće li određeni zahtjev biti dopušten, blokiran ili dodatno nadziran.

Osnovna svrha Web Application Firewalla jest sprječavanje iskorištavanja ranjivosti u web aplikacijama bez potrebe za izmjenama njihova izvornog koda. Zbog toga se WAF često koristi kao dodatni zaštitni sloj u situacijama kada aplikacija već radi u produkcijskom okruženju ili kada promjene aplikacijskog koda nisu odmah izvedive. Na taj način WAF omogućuje brzu reakciju na sigurnosne prijetnje i smanjuje izloženost aplikacije napadima.

Važno je naglasiti da WAF ne zamjenjuje siguran razvoj softvera niti uklanja potrebu za pravilnim dizajnom i testiranjem web aplikacija. Umjesto toga, on nadopunjuje postojeće sigurnosne mjere te povećava ukupnu razinu zaštite sustava. WAF se uklapa u koncept višeslojne sigurnosti (defence in depth), gdje se sigurnost ne oslanja na jednu zaštitnu mjeru, već na kombinaciju različitih mehanizama.

Osim zaštitne funkcije, WAF često ima i nadzornu ulogu. Analizom i zapisivanjem web prometa omogućuje uvid u ponašanje korisnika i potencijalnih napadača, što može biti korisno za sigurnosnu analizu, otkrivanje obrazaca napada te poboljšanje sigurnosnih politika. Time WAF doprinosi ne samo prevenciji napada, već i boljem razumijevanju sigurnosnog stanja web aplikacije.

Zbog svoje sposobnosti analize aplikacijskog prometa i fleksibilnosti u implementaciji sigurnosnih pravila, Web Application Firewall danas se smatra jednim od ključnih alata za zaštitu web aplikacija, posebno u okruženjima s povećanim sigurnosnim zahtjevima i visokom razinom izloženosti internetu.

---

### Uloga WAF-a u zaštiti web aplikacija

Web Application Firewall ima važnu ulogu u cjelokupnoj sigurnosnoj strategiji web aplikacija jer djeluje kao dodatni sloj obrane između korisnika i aplikacijskog sustava. Njegova osnovna zadaća nije zamjena postojećih sigurnosnih mehanizama ili sigurnog razvoja softvera, već nadopuna sigurnosne arhitekture s ciljem smanjenja rizika od uspješnih napada na aplikacijsku razinu.

Jedna od ključnih uloga WAF-a jest zaštita web aplikacija koje su već u produkciji. U takvim okruženjima izmjene izvornog koda često su vremenski zahtjevne, podložne procedurama testiranja i odobravanja ili tehnički ograničene. WAF omogućuje brzo uvođenje sigurnosnih mjera bez potrebe za izmjenama aplikacijskog koda, čime se značajno skraćuje vrijeme reakcije na otkrivene ranjivosti. Zbog te karakteristike WAF se često koristi kao privremeno ili trajno rješenje za smanjenje izloženosti aplikacije poznatim prijetnjama.

WAF također ima važnu ulogu u zaštiti osjetljivih podataka koje web aplikacije obrađuju i pohranjuju. Filtriranjem i nadzorom web prometa smanjuje se mogućnost neovlaštenog pristupa podacima te se povećava razina povjerljivosti i integriteta informacijskog sustava. Time WAF doprinosi ispunjavanju sigurnosnih zahtjeva i očuvanju povjerenja korisnika u web usluge.

U okviru koncepta višeslojne sigurnosti, WAF se pozicionira između mrežne zaštite i same aplikacije. Dok mrežni sigurnosni mehanizmi štite infrastrukturu, a sigurni razvoj aplikacije smanjuje broj ranjivosti u kodu, WAF pruža dodatni sloj kontrole koji nadzire stvarno ponašanje korisnika i zahtjeva u realnom vremenu. Takav pristup smanjuje vjerojatnost da će pojedina sigurnosna slabost dovesti do potpunog kompromitiranja sustava.

Osim zaštitne funkcije, WAF ima i preventivnu te nadzornu ulogu. Kontinuiranim praćenjem prometa i bilježenjem sumnjivih aktivnosti omogućuje ranije otkrivanje sigurnosnih prijetnji i neobičnih obrazaca ponašanja. Ti podaci mogu poslužiti kao temelj za daljnje unapređenje sigurnosnih politika, analizu incidenata i donošenje informiranih odluka o sigurnosnim poboljšanjima.

Važan aspekt uloge WAF-a odnosi se i na usklađenost s regulatornim i industrijskim standardima. U mnogim sigurnosnim okvirima i standardima zaštite podataka, WAF se navodi kao prihvatljiva ili preporučena mjera zaštite web aplikacija. Time WAF ne doprinosi samo tehničkoj sigurnosti sustava, već i smanjenju pravnih, financijskih i reputacijskih rizika za organizaciju.

Uloga Web Application Firewalla u zaštiti web aplikacija očituje se kroz povećanje otpornosti sustava na napade, omogućavanje brze reakcije na ranjivosti te jačanje ukupne sigurnosne posture aplikacije. WAF predstavlja važnu kariku između preventivnih i reaktivnih sigurnosnih mjera te ima ključnu ulogu u suvremenom pristupu zaštiti web aplikacija.

---

### Arhitektura WAF-a

Arhitektura Web Application Firewalla odnosi se na način na koji je WAF integriran u komunikacijski tok između korisnika i web aplikacije te na logičku strukturu obrade web prometa. WAF je pozicioniran tako da ima potpun uvid u HTTP i HTTPS komunikaciju, što mu omogućuje nadzor, analizu i kontrolu zahtjeva i odgovora na aplikacijskoj razini.

U osnovnom arhitektonskom modelu, sav promet koji dolazi od klijenata prema web aplikaciji prolazi kroz WAF prije nego što dosegne web poslužitelj. Na isti način, promet koji aplikacija vraća korisnicima također može biti obrađen od strane WAF-a. Takav položaj omogućuje WAF-u da djeluje kao kontrolna točka kroz koju se provode sigurnosne politike i pravila.

Logički gledano, arhitektura WAF-a sastoji se od nekoliko uzastopnih faza obrade prometa. Prva faza uključuje prihvat i normalizaciju HTTP/HTTPS zahtjeva, pri čemu se promet prilagođava standardiziranom obliku kako bi se spriječile tehnike zaobilaženja sigurnosnih mehanizama. Nakon toga slijedi analiza sadržaja zahtjeva, gdje se ispituju URL-ovi, parametri, zaglavlja, kolačići i tijelo poruke. Na temelju rezultata analize, WAF donosi odluku o daljnjem postupanju sa zahtjevom.

Odluka koju WAF donosi može uključivati prosljeđivanje zahtjeva prema aplikaciji, njegovo blokiranje ili dodatno bilježenje i nadzor. U nekim slučajevima moguće je i modificiranje zahtjeva ili odgovora kako bi se uklonili potencijalno opasni dijelovi ili sakrile osjetljive informacije. Takav način rada omogućuje fleksibilnu kontrolu prometa bez izravnog utjecaja na funkcionalnost aplikacije.

Važan dio arhitekture WAF-a čini i sustav za upravljanje pravilima i sigurnosnim politikama. Pravila definiraju kako se pojedini elementi web prometa trebaju obrađivati, dok sigurnosne politike određuju opće smjernice zaštite aplikacije. Upravljanje tim pravilima često se odvija centralizirano, što omogućuje dosljednu primjenu sigurnosnih mjera na više web aplikacija unutar istog sustava.

Arhitektura WAF-a također uključuje komponente za zapisivanje i analizu događaja. Sustav za bilježenje omogućuje prikupljanje informacija o legitimnom i zlonamjernom prometu, dok analitičke komponente pomažu u prepoznavanju obrazaca ponašanja i potencijalnih sigurnosnih incidenata. Takav pristup doprinosi ne samo zaštiti u stvarnom vremenu, već i dugoročnom unapređenju sigurnosnih politika.

Prilikom dizajna arhitekture WAF-a važno je voditi računa o performansama i pouzdanosti sustava. Budući da sav promet prolazi kroz WAF, njegova arhitektura mora biti dovoljno skalabilna kako bi podnijela opterećenje bez negativnog utjecaja na dostupnost i odziv web aplikacije. Zbog toga se arhitektura WAF-a često planira u skladu s postojećom infrastrukturom i očekivanim rastom sustava.

---

### Vrste WAF-ova

Web Application Firewalle moguće je klasificirati prema načinu implementacije i mjestu na kojem se nalaze u odnosu na web aplikaciju i infrastrukturu. Takva podjela omogućuje lakše razumijevanje razlika između pojedinih rješenja te olakšava odabir odgovarajućeg WAF-a ovisno o sigurnosnim zahtjevima, organizacijskoj strukturi i tehničkim ograničenjima.

#### Mrežni (network-based) WAF

Mrežni WAF implementira se kao zaseban sigurnosni uređaj ili virtualna instanca koja se postavlja ispred web poslužitelja. Ovakav tip WAF-a obično djeluje kao centralna točka kroz koju prolazi sav web promet prema jednoj ili više aplikacija. Prednost mrežnog WAF-a je visoka razina kontrole i performansi, budući da je optimiziran isključivo za sigurnosne zadatke.

Mrežni WAF-ovi često se koriste u većim organizacijama s vlastitom infrastrukturom, gdje postoji potreba za centraliziranom zaštitom više aplikacija. Međutim, njihova implementacija može zahtijevati dodatna ulaganja u hardver ili virtualne resurse te stručnost za održavanje i upravljanje.

#### WAF temeljen na poslužitelju (host-based WAF)

Host-based WAF instalira se izravno na web poslužitelj ili aplikacijski poslužitelj kao softverska komponenta ili modul. Takav WAF postaje dio postojećeg sustava i izravno se integrira s web poslužiteljem, što omogućuje detaljniji uvid u aplikacijski promet.

Prednost ovog pristupa je fleksibilnost i niži troškovi implementacije u odnosu na mrežne WAF-ove. Također, host-based WAF može biti prikladan za manje sustave ili razvojna okruženja. S druge strane, ovakav tip WAF-a dijeli resurse s aplikacijom, što može utjecati na performanse te povećava složenost administracije na razini pojedinog poslužitelja.

#### WAF u oblaku (cloud-based WAF)

Cloud-based WAF pruža se kao usluga i nalazi se izvan lokalne infrastrukture organizacije. Sav web promet prema aplikaciji usmjerava se kroz infrastrukturu pružatelja usluge, gdje se provodi sigurnosna analiza i filtriranje. Ovakav pristup omogućuje brzu implementaciju i visoku skalabilnost, budući da se upravljanje infrastrukturom prebacuje na vanjskog pružatelja.

Prednost cloud-based WAF-a je jednostavnost korištenja i mogućnost prilagodbe opterećenju, što ga čini posebno pogodnim za aplikacije s promjenjivim prometom. Međutim, ovisnost o vanjskom pružatelju usluge i prijenos prometa izvan vlastite infrastrukture mogu predstavljati ograničenja u okruženjima s posebnim sigurnosnim ili regulatornim zahtjevima.

---

### Glavne funkcionalnosti WAF-a

Web Application Firewall pruža niz funkcionalnosti koje su usmjerene na zaštitu web aplikacija, nadzor prometa i povećanje ukupne sigurnosti sustava. Te funkcionalnosti omogućuju kontrolu komunikacije između korisnika i aplikacije te provedbu sigurnosnih politika na aplikacijskoj razini.

Jedna od temeljnih funkcionalnosti WAF-a jest filtriranje web prometa. WAF analizira dolazne i odlazne HTTP/HTTPS zahtjeve te na temelju definiranih pravila odlučuje hoće li određeni zahtjev biti dopušten ili blokiran. Na taj način sprječava se pristup aplikaciji putem sumnjivih ili nepoželjnih zahtjeva prije nego što oni dođu do web poslužitelja.

Sljedeća važna funkcionalnost odnosi se na validaciju korisničkog unosa. WAF omogućuje provjeru parametara koji se šalju putem URL-a, obrazaca i zaglavlja, čime se smanjuje mogućnost zloupotrebe aplikacijskog sučelja. Validacijom se osigurava da aplikacija prima samo podatke koji su u skladu s očekivanim formatom i pravilima.

WAF također ima značajnu ulogu u upravljanju sesijama i kolačićima. Nadzor nad načinom korištenja sesija omogućuje dodatnu kontrolu nad autentifikacijom korisnika te smanjuje rizik od zloupotrebe sesijskih podataka. Ova funkcionalnost doprinosi stabilnosti i sigurnosti korisničkih interakcija s web aplikacijom.

Važna funkcionalnost WAF-a je i bilježenje i nadzor sigurnosnih događaja. WAF prikuplja podatke o web prometu, blokiranim zahtjevima i sumnjivim aktivnostima, čime omogućuje detaljnu analizu sigurnosnog stanja aplikacije. Takvi zapisi mogu poslužiti kao osnova za otkrivanje sigurnosnih incidenata, forenzičku analizu te poboljšanje sigurnosnih politika.

Osim toga, WAF omogućuje centralizirano upravljanje sigurnosnim pravilima. Pravila i politike mogu se definirati i primjenjivati na više aplikacija iz jednog sustava, što olakšava administraciju i osigurava dosljednu primjenu sigurnosnih mjera. Ova funkcionalnost posebno je korisna u složenim okruženjima s većim brojem web aplikacija.

WAF može imati i zaštitnu ulogu u osiguravanju dostupnosti aplikacije. Praćenjem prometa i primjenom ograničenja na zahtjeve, WAF pomaže u smanjenju opterećenja aplikacije te sprječava situacije koje bi mogle dovesti do prekida rada ili degradacije usluge.

---

### Mehanizmi detekcije napada

Mehanizmi detekcije napada predstavljaju temeljni dio rada Web Application Firewalla jer omogućuju prepoznavanje zlonamjernih zahtjeva i sumnjivog ponašanja u web prometu. Za razliku od općih sigurnosnih mehanizama, WAF koristi pristupe prilagođene aplikacijskoj razini komunikacije, analizirajući sadržaj i kontekst HTTP/HTTPS zahtjeva.

Jedan od najčešće korištenih mehanizama detekcije je detekcija temeljena na potpisima (signature-based detection). Ovaj pristup temelji se na usporedbi dolaznih zahtjeva s bazom poznatih obrazaca napada. Ako zahtjev odgovara nekom od definiranih potpisa, WAF ga identificira kao potencijalno zlonamjeran i poduzima odgovarajuću akciju. Prednost ovog mehanizma je visoka učinkovitost u prepoznavanju poznatih prijetnji, dok mu je ograničenje nemogućnost detekcije novih ili nepoznatih napada.

Drugi važan pristup je detekcija temeljena na anomalijama (anomaly-based detection). Ovaj mehanizam ne oslanja se na unaprijed definirane obrasce, već na analizu uobičajenog ponašanja web aplikacije i korisnika. WAF uspostavlja referentni model normalnog prometa te identificira odstupanja od tog modela kao potencijalne prijetnje. Takav pristup omogućuje prepoznavanje nepoznatih i ranije neuočenih napada, ali može rezultirati većim brojem lažno pozitivnih detekcija.

Osim navedenih pristupa, WAF često koristi pozitivni sigurnosni model, poznat i kao pristup temeljen na dopuštenim obrascima. U ovom slučaju, definiraju se isključivo legitimni obrasci ponašanja i strukture zahtjeva, dok se svi ostali zahtjevi automatski odbacuju. Ovakav pristup pruža visoku razinu sigurnosti, ali zahtijeva detaljno poznavanje aplikacije i pažljivu konfiguraciju pravila.

Suprotno tome, negativni sigurnosni model temelji se na definiranju zabranjenih obrazaca i ponašanja. WAF dopušta sav promet koji ne krši definirana pravila, dok blokira zahtjeve koji odgovaraju poznatim prijetnjama. Ovaj model jednostavniji je za implementaciju, ali može biti manje učinkovit protiv sofisticiranih ili novih napada.

U praksi se najčešće koristi kombinirani pristup, koji objedinjuje više mehanizama detekcije. Kombinacijom potpisne i anomalijske detekcije, uz primjenu pozitivnih i negativnih sigurnosnih modela, WAF postiže uravnotežen odnos između sigurnosti i funkcionalnosti aplikacije. Takav pristup smanjuje vjerojatnost uspješnog napada, ali i rizik od nepotrebnog blokiranja legitimnog prometa.

---

### Prednosti i ograničenja WAF-a

Web Application Firewall donosi brojne prednosti u zaštiti web aplikacija, ali istovremeno ima i određena ograničenja koja je potrebno uzeti u obzir prilikom njegove primjene. Razumijevanje obje strane ključno je za pravilno pozicioniranje WAF-a unutar sigurnosne arhitekture informacijskog sustava.

Jedna od najvažnijih prednosti WAF-a jest mogućnost brze i učinkovite zaštite web aplikacija bez izmjene izvornog koda. To je posebno značajno u produkcijskim okruženjima gdje su promjene aplikacije složene ili vremenski ograničene. WAF omogućuje smanjenje izloženosti aplikacije poznatim prijetnjama u kratkom vremenskom roku, čime se povećava ukupna razina sigurnosti sustava.

Dodatna prednost WAF-a je centralizirana kontrola sigurnosnih politika. Korištenjem jednog sigurnosnog mehanizma moguće je upravljati zaštitom više web aplikacija, što pojednostavljuje administraciju i osigurava dosljednu primjenu sigurnosnih pravila. Takav pristup olakšava održavanje i smanjuje mogućnost pogrešaka u konfiguraciji.

WAF također doprinosi povećanoj vidljivosti sigurnosnih događaja. Analizom i bilježenjem web prometa omogućuje se praćenje sumnjivih aktivnosti i prepoznavanje obrazaca ponašanja koji mogu upućivati na sigurnosne incidente. Ova vidljivost korisna je za analizu incidenata, izvještavanje i unapređenje sigurnosnih politika.

Osim navedenog, WAF može pomoći u ispunjavanju regulatornih i industrijskih zahtjeva vezanih uz sigurnost web aplikacija i zaštitu podataka. U mnogim sigurnosnim standardima WAF se navodi kao preporučena ili prihvatljiva zaštitna mjera, što organizacijama olakšava usklađivanje s propisima i smanjuje rizik od financijskih i reputacijskih posljedica sigurnosnih incidenata.

Unatoč brojnim prednostima, WAF ima i određena ograničenja. Jedno od ključnih ograničenja odnosi se na mogućnost lažno pozitivnih i lažno negativnih detekcija. Nepravilno konfiguriran WAF može blokirati legitimni promet ili, s druge strane, propustiti zlonamjerne zahtjeve. Zbog toga je potrebno kontinuirano prilagođavanje pravila i praćenje ponašanja sustava.

Također, WAF ima ograničen uvid u poslovnu logiku aplikacije. Iako može analizirati strukturu i sadržaj web prometa, ne razumije u potpunosti kontekst i specifične procese aplikacije. Zbog toga ne može spriječiti sve vrste zloupotreba koje proizlaze iz logičkih pogrešaka u dizajnu aplikacije.

Dodatno ograničenje može predstavljati utjecaj na performanse sustava. Budući da sav promet prolazi kroz WAF, nepravilno dimenzioniran ili konfiguriran sustav može dovesti do povećanog kašnjenja ili smanjenja dostupnosti aplikacije. To zahtijeva pažljivo planiranje i testiranje prije implementacije.

---

### Vrste napada koje WAF sprječava

Web Application Firewall osmišljen je za zaštitu web aplikacija od napada koji ciljaju ranjivosti na aplikacijskoj razini. Takvi napadi iskorištavaju način na koji aplikacija obrađuje korisnički unos, upravlja sesijama i komunicira s pozadinskim sustavima. WAF omogućuje prepoznavanje i blokiranje širokog spektra prijetnji koje se pojavljuju u stvarnim web okruženjima.

Jedan od najčešćih napada koje WAF sprječava je SQL injection. Ovaj napad temelji se na ubacivanju zlonamjernih SQL naredbi u korisnički unos s ciljem neovlaštenog pristupa bazi podataka, izmjene podataka ili njihovog brisanja. WAF može prepoznati sumnjive obrasce u zahtjevima i spriječiti njihovo prosljeđivanje aplikaciji.

Cross-Site Scripting (XSS) predstavlja još jednu značajnu prijetnju web aplikacijama. Kod ovog napada napadač pokušava umetnuti zlonamjerni skript koji se izvršava u pregledniku krajnjeg korisnika. Posljedice mogu uključivati krađu sesijskih podataka ili manipulaciju sadržajem stranice. WAF pomaže u smanjenju rizika od XSS napada filtriranjem i kontrolom nepoželjnog sadržaja u web prometu.

Cross-Site Request Forgery (CSRF) napadi koriste povjerenje web aplikacije u autentificiranog korisnika kako bi izvršili neovlaštene radnje bez znanja korisnika. WAF može prepoznati neuobičajene obrasce zahtjeva i dodatno zaštititi aplikaciju od ovakvih pokušaja zloupotrebe korisničkih sesija.

Napadi manipulacijom parametara usmjereni su na izmjenu vrijednosti koje aplikacija koristi za obradu zahtjeva, s ciljem zaobilaženja sigurnosnih ograničenja. WAF nadzire strukturu i očekivane vrijednosti parametara, čime smanjuje mogućnost njihove zloupotrebe.

Path traversal napadi nastoje omogućiti pristup datotekama i direktorijima izvan predviđenog raspona aplikacije. Iskorištavanjem nepravilne obrade putanja, napadač može doći do osjetljivih datoteka na poslužitelju. WAF može ograničiti i kontrolirati takve zahtjeve prije nego što dođu do aplikacije.

Osim toga, WAF može pomoći u zaštiti od automatiziranih napada, poput pokušaja masovnih prijava ili zloupotrebe aplikacijskih resursa. Praćenjem ponašanja i učestalosti zahtjeva, WAF doprinosi smanjenju utjecaja ovakvih napada na dostupnost aplikacije.

U određenoj mjeri, WAF može ublažiti i aplikacijske napade uskraćivanja usluge (Application-layer DoS), koji se temelje na velikom broju zahtjeva dizajniranih da iscrpe resurse aplikacije. Iako WAF nije zamjena za specijalizirana rješenja protiv distribuiranih napada, on može smanjiti njihov učinak filtriranjem sumnjivog prometa.
