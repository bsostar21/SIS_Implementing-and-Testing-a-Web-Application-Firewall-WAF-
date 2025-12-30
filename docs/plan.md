1. Arhitektura sustava
Praktični dio projekta temelji se na implementaciji Web Application Firewalla u izoliranom virtualnom laboratorijskom okruženju. Sustav je dizajniran tako da oponaša realan scenarij zaštite web aplikacije u produkcijskom okruženju, pri čemu sav promet prema aplikaciji prolazi kroz WAF sloj.

Arhitektura sustava sastoji se od tri virtualne mašine, svaka s jasno definiranom ulogom u procesu testiranja sigurnosti web aplikacije. Ovakva podjela omogućuje jasnu separaciju napadača, sigurnosnog sloja i ciljne aplikacije, čime se postiže preglednost i kontrola tijekom testiranja.

1.1. Komponente sustava
Sustav uključuje sljedeće virtualne mašine:

Attack VM – virtualna mašina s koje se izvode napadi. Na njoj se koriste alati poput curl i OWASP ZAP za simulaciju ručnih i automatiziranih napada.

WAF VM – virtualna mašina na kojoj je implementiran Web Application Firewall. Koristi se Apache web poslužitelj u kombinaciji s ModSecurity modulom i OWASP Core Rule Set pravilima.

WebGoat VM – virtualna mašina na kojoj se nalazi ranjiva web aplikacija OWASP WebGoat, koja služi kao meta napada.

1.2. Topologija sustava
Attack VM → WAF VM → WebGoat VM

Svi HTTP zahtjevi koji dolaze s Attack VM prvo prolaze kroz WAF VM, gdje se analiziraju i filtriraju, a tek nakon toga, ako su dopušteni, prosljeđuju se WebGoat aplikaciji. Na taj način osigurano je da WebGoat nikada nije izravno izložen napadaču, već je zaštićen WAF slojem.

Ovakva arhitektura omogućuje jasnu demonstraciju uloge Web Application Firewalla i preciznu analizu njegovog ponašanja u stvarnim napadnim scenarijima.

---

2. VirtualBox mrežna konfiguracija
Kako bi se osigurala pravilna komunikacija između virtualnih mašina, sve tri VM povezane su u zajedničku internu mrežu unutar VirtualBox okruženja. Cilj ovakve konfiguracije je omogućiti međusobnu komunikaciju VM-ova uz istovremeno zadržavanje izolacije od vanjske mreže.

Za svaku virtualnu mašinu konfigurirana su dva mrežna adaptera.

2.1 Interna mreža

Prvi mrežni adapter postavljen je na Internal Network s nazivom mreže waf_lab. Ova mreža omogućuje komunikaciju između Attack VM, WAF VM i WebGoat VM unutar istog subnet-a, bez izravnog pristupa internetu.

Interna mreža koristi se za sav promet vezan uz testiranje WAF-a, uključujući napade i prosljeđivanje zahtjeva prema WebGoat aplikaciji.

2.2 NAT mreža

Drugi mrežni adapter postavljen je na NAT i koristi se isključivo za pristup internetu, primjerice za instalaciju paketa, preuzimanje alata i ažuriranje sustava. Ovaj adapter nije uključen u komunikaciju između virtualnih mašina tijekom testiranja.

2.3 Provjera mrežne konfiguracije

Na svakoj virtualnoj mašini provjerena je IP adresa pomoću naredbe:
ip a

Ovom naredbom ispisuju se mrežna sučelja i dodijeljene IP adrese. Provjerom je potvrđeno da sve tri virtualne mašine imaju IP adrese unutar istog subnet-a interne mreže, čime je osigurana njihova međusobna povezanost.

---

3. WebGoat VM – instalacija i pokretanje
U ovoj točki provedena je instalacija i pokretanje ranjive web aplikacije OWASP WebGoat, koja u praktičnom dijelu projekta služi kao ciljna aplikacija za testiranje Web Application Firewalla. WebGoat je edukativna aplikacija namjerno dizajnirana s brojnim sigurnosnim ranjivostima kako bi se omogućilo sigurno učenje i demonstracija napada na web aplikacije.

WebGoat je instaliran na zasebnoj virtualnoj mašini temeljnoj na operacijskom sustavu Ubuntu 22.04 LTS, čime se osigurava jasna izolacija između aplikacije, WAF-a i napadačkog okruženja.

3.1 Instalacija potrebnih paketa
sudo apt update
sudo apt install -y openjdk-17-jdk wget

Prvo je izvršena naredba sudo apt update, kojom se osvježava popis dostupnih softverskih paketa iz službenih repozitorija operacijskog sustava. Ovaj korak osigurava da se tijekom instalacije koriste najnovije dostupne i stabilne verzije paketa.

Nakon toga instalirani su potrebni paketi naredbom sudo apt install -y openjdk-17-jdk wget. Paket openjdk-17-jdk omogućuje pokretanje Java aplikacija, što je nužno jer je WebGoat razvijen u programskom jeziku Java. Paket wget koristi se za preuzimanje instalacijskih datoteka s interneta. Opcija -y omogućuje automatsko potvrđivanje instalacije, čime se proces odvija bez dodatne korisničke interakcije.

3.2 Preuzimanje WebGoat aplikacije
wget https://github.com/WebGoat/WebGoat/releases/download/v8.2.2/webgoat-server-8.2.2.jar

Ovom naredbom preuzima se izvršna JAR datoteka WebGoat aplikacije s GitHub repozitorija projekta OWASP WebGoat. Korištena je verzija 8.2.2 jer predstavlja stabilno izdanje koje uključuje velik broj implementiranih sigurnosnih ranjivosti prikladnih za testiranje i edukaciju. Preuzeta datoteka sadrži kompletan WebGoat poslužitelj, čime se izbjegava potreba za dodatnom konfiguracijom aplikacijskog poslužitelja.

3.3 Pokretanje WebGoat aplikacije
java -jar webgoat-server-8.2.2.jar

Naredbom java -jar webgoat-server-8.2.2.jar pokreće se WebGoat aplikacija kao lokalni web poslužitelj unutar virtualne mašine. Tijekom pokretanja aplikacija inicijalizira potrebne servise i otvara mrežni port na kojem postaje dostupna korisnicima.

Nakon uspješnog pokretanja, WebGoat aplikaciji moguće je pristupiti putem web preglednika koristeći IP adresu WebGoat virtualne mašine i port 8080, na sljedećoj adresi:

http://IP_WEBGOAT:8080

Ovim putem korisnik dobiva pristup WebGoat sučelju i ranjivim modulima aplikacije.

3.4 Uloga WebGoat aplikacije u sustavu

U arhitekturi sustava WebGoat aplikacija predstavlja namjerno ranjivu ciljnu aplikaciju koja se nalazi iza Web Application Firewalla. Njezina svrha nije pružanje sigurnog okruženja, već omogućavanje simulacije stvarnih napada na aplikacijskoj razini. Time se omogućuje promatranje i analiza ponašanja WAF-a u realističnim uvjetima te provjera njegove sposobnosti detekcije i blokiranja zlonamjernih zahtjeva.

Korištenjem WebGoata u praktičnom dijelu rada ostvarena je jasna veza između teorijskih koncepata sigurnosti web aplikacija i njihove praktične primjene u kontroliranom laboratorijskom okruženju.

---

4. WAF VM – instalacija i konfiguracija
U ovoj točki provedena je instalacija i osnovna konfiguracija Web Application Firewalla na zasebnoj virtualnoj mašini. Cilj ove faze bio je postaviti sigurnosni sloj koji će analizirati sav HTTP promet prije nego što on dođe do ranjive WebGoat aplikacije. Kao WAF rješenje korišten je ModSecurity, koji je integriran s Apache web poslužiteljem, uz primjenu OWASP Core Rule Set (CRS) pravila.

WAF virtualna mašina ima ulogu središnje sigurnosne komponente sustava i postavljena je između Attack VM i WebGoat VM.

4.1 Instalacija Apache web poslužitelja i ModSecurity modula
sudo apt update
sudo apt install -y apache2 libapache2-mod-security2 modsecurity-crs

Prvo je izvršena naredba sudo apt update, kojom se osvježava popis dostupnih paketa kako bi sustav koristio najnovije informacije iz repozitorija. Ovaj korak osigurava stabilnu i kompatibilnu instalaciju softvera.

Naredbom sudo apt install -y apache2 libapache2-mod-security2 modsecurity-crs instaliraju se ključne komponente WAF sustava. Apache web poslužitelj služi kao osnovna platforma na kojoj će WAF raditi. Modul libapache2-mod-security2 omogućuje integraciju ModSecurityja s Apacheom, čime Apache dobiva funkcionalnost analize i filtriranja HTTP zahtjeva. Paket modsecurity-crs instalira OWASP Core Rule Set, koji sadrži unaprijed definirana pravila za detekciju najčešćih napada na web aplikacije. Opcija -y omogućuje automatsku potvrdu instalacije bez dodatne interakcije korisnika.

4.2 Provjera učitavanja ModSecurity modula
sudo apache2ctl -M | grep security

Ovom naredbom provjerava se je li ModSecurity modul uspješno učitan u Apache web poslužitelj. Alat apache2ctl -M ispisuje sve aktivne Apache module, dok se pomoću grep security filtrira izlaz kako bi se pronašao sigurnosni modul. Pojava ModSecurity modula u izlazu potvrđuje da je WAF komponenta aktivna i spremna za konfiguraciju.

4.3 Omogućavanje potrebnih Apache modula
sudo a2enmod proxy proxy_http headers rewrite
sudo systemctl restart apache2

Naredbom sudo a2enmod proxy proxy_http headers rewrite omogućuju se dodatni Apache moduli potrebni za ispravan rad WAF-a u ulozi reverse proxyja. Modul proxy omogućuje prosljeđivanje zahtjeva prema drugom poslužitelju, dok proxy_http omogućuje prosljeđivanje HTTP prometa. Modul headers koristi se za obradu HTTP zaglavlja, a modul rewrite omogućuje prilagodbu i preusmjeravanje zahtjeva.

Nakon omogućavanja modula, Apache web poslužitelj ponovno se pokreće naredbom sudo systemctl restart apache2 kako bi se promjene učitale i postale aktivne.

4.4 Aktivacija ModSecurityja u blokirajućem načinu rada
sudo cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf

Ovom naredbom kopira se preporučena konfiguracijska datoteka ModSecurityja u aktivnu konfiguracijsku datoteku. Time se postavlja osnovna konfiguracija koja se može dodatno prilagođavati potrebama sustava.

U aktivnoj konfiguracijskoj datoteci postavlja se sljedeća direktiva:

SecRuleEngine On

Postavljanjem ove opcije ModSecurity se prebacuje iz nadzornog (DetectionOnly) u blokirajući način rada, što znači da WAF ne samo da detektira sumnjive zahtjeve, već ih aktivno blokira prije nego što dođu do web aplikacije.

Nakon izmjene konfiguracije, Apache se ponovno pokreće kako bi se promjene primijenile.

4.5 Uključivanje OWASP Core Rule Set pravila
IncludeOptional /usr/share/modsecurity-crs/owasp-crs.load

Ova direktiva omogućuje učitavanje OWASP Core Rule Set pravila u ModSecurity. Pravila definiraju obrasce poznatih napada na web aplikacije i primjenjuju se na sav HTTP promet koji prolazi kroz WAF. Uključivanjem CRS-a značajno se povećava sposobnost WAF-a da prepozna i blokira zlonamjerne zahtjeve.

Nakon uključivanja pravila, Apache web poslužitelj ponovno se pokreće kako bi nova konfiguracija postala aktivna.

4.6 Uloga WAF virtualne mašine u sustavu

WAF virtualna mašina u ovom projektu ima ključnu ulogu u zaštiti web aplikacije. Smještena je između napadačkog okruženja i WebGoat aplikacije te analizira sav promet u stvarnom vremenu. Ovakva konfiguracija omogućuje jasnu demonstraciju rada Web Application Firewalla i njegovu učinkovitost u detekciji i blokiranju napada na aplikacijskoj razini.

---

5. Reverse proxy konfiguracija

U ovoj fazi konfiguracije WAF virtualna mašina postavljena je da radi kao reverse proxy između napadačke virtualne mašine i WebGoat aplikacije. Reverse proxy omogućuje da sav promet namijenjen web aplikaciji prolazi kroz WAF, gdje se analizira i filtrira prije nego što se proslijedi WebGoatu. Na taj način WebGoat aplikacija nije izravno izložena klijentu ili napadaču, već je zaštićena dodatnim sigurnosnim slojem.

5.1 Konfiguracija prosljeđivanja zahtjeva

U Apache konfiguraciji definirano je prosljeđivanje svih dolaznih zahtjeva prema WebGoat aplikaciji koja radi na portu 8080. U konfiguracijsku datoteku virtualnog hosta dodane su sljedeće direktive:

ProxyPass / http://IP_WEBGOAT:8080/
ProxyPassReverse / http://IP_WEBGOAT:8080/

Direktiva ProxyPass određuje da se svi zahtjevi koji dolaze na korijensku putanju WAF poslužitelja (/) prosljeđuju prema WebGoat aplikaciji na adresi IP_WEBGOAT i portu 8080. Time WAF postaje ulazna točka za sav web promet.

Direktiva ProxyPassReverse služi za ispravnu obradu odgovora koje WebGoat vraća klijentu. Ona prilagođava HTTP zaglavlja u odgovorima tako da klijent vidi WAF kao krajnju adresu, a ne internu adresu WebGoat poslužitelja. Time se osigurava pravilno funkcioniranje aplikacije kroz proxy.

5.2 Primjena konfiguracije

Nakon dodavanja navedenih direktiva, Apache web poslužitelj ponovno se pokreće:

sudo systemctl restart apache2

Ovim korakom nova reverse proxy konfiguracija postaje aktivna. Od tog trenutka sav promet koji dolazi na WAF poslužitelj prolazi kroz ModSecurity i OWASP CRS pravila prije nego što se proslijedi WebGoat aplikaciji.

5.3 Provjera ispravnosti reverse proxyja

Ispravnost konfiguracije provjerava se slanjem HTTP zahtjeva prema IP adresi WAF virtualne mašine:

curl http://IP_WAF

Ako je konfiguracija ispravna, WAF će proslijediti zahtjev prema WebGoatu te će se u odgovoru prikazati sadržaj WebGoat aplikacije. Ova provjera potvrđuje da je WAF ispravno postavljen kao posrednik između klijenta i web aplikacije.

---

6. Attack VM – priprema okruženja

Nakon postavljanja WAF-a i WebGoat aplikacije, pripremljena je virtualna mašina namijenjena izvođenju napada. Attack VM služi za simulaciju ponašanja potencijalnog napadača i omogućuje testiranje reakcije WAF-a na zlonamjerne zahtjeve.

6.1 Instalacija alata za testiranje
sudo apt update
sudo apt install -y curl net-tools zaproxy

Naredbom sudo apt update osvježava se popis dostupnih paketa kako bi se osigurala instalacija najnovijih verzija alata.

Naredbom sudo apt install -y curl net-tools zaproxy instaliraju se alati potrebni za testiranje sigurnosti. Alat curl koristi se za ručno slanje HTTP zahtjeva i simulaciju pojedinačnih napada. Paket net-tools omogućuje osnovnu mrežnu dijagnostiku, dok je zaproxy OWASP ZAP alat namijenjen automatiziranom testiranju sigurnosti web aplikacija.

6.2 Provjera dostupnosti alata
which curl

Ovom naredbom provjerava se je li alat curl ispravno instaliran i dostupan u sustavu. Ispravan izlaz potvrđuje da je Attack VM spremna za izvođenje testnih napada.

---

7. Simulacija napada

U ovoj fazi praktičnog dijela provedena je simulacija različitih napada na web aplikaciju kako bi se provjerila učinkovitost Web Application Firewalla u detekciji i blokiranju zlonamjernih zahtjeva. Napadi su izvođeni s Attack VM, dok je sav promet bio usmjeren prema WAF VM, koji je zatim, ovisno o pravilima, zahtjeve prosljeđivao ili blokirao prije dolaska do WebGoat aplikacije.

Za simulaciju napada korišten je alat curl, koji omogućuje ručno slanje HTTP zahtjeva i preciznu kontrolu parametara. Time je omogućena jasna i transparentna provjera reakcije WAF-a na pojedine vrste napada.

7.1 Simulacija Cross-Site Scripting (XSS) napada
curl -I "http://IP_WAF/?test=<script>alert(1)</script>"

Ovom naredbom simulira se pokušaj Cross-Site Scripting napada. U parametar test umeće se JavaScript kod koji bi, u slučaju da nije filtriran, mogao biti izvršen u pregledniku korisnika. Cilj ovog testa je provjeriti prepoznaje li WAF pokušaj umetanja skriptnog koda unutar HTTP zahtjeva.

Opcija -I koristi se kako bi se dohvatila samo HTTP zaglavlja odgovora, što je dovoljno za utvrđivanje je li zahtjev blokiran ili dopušten. Ako WAF pravilno funkcionira, zahtjev se neće proslijediti WebGoat aplikaciji.

7.2 Simulacija SQL Injection napada
curl -I "http://IP_WAF/?id=' OR 1=1 --"

Ovom naredbom simulira se SQL injection napad, pri kojem se zlonamjerni SQL izraz umeće u parametar id. Takav unos može omogućiti manipulaciju SQL upitima u pozadini aplikacije, što može dovesti do neovlaštenog pristupa podacima ili zaobilaženja sigurnosnih mehanizama.

WAF analizira sadržaj zahtjeva i, koristeći OWASP Core Rule Set pravila, prepoznaje karakteristične obrasce SQL injection napada te blokira zahtjev prije nego što dođe do aplikacije.

7.3 Simulacija Command Injection napada
curl -I "http://IP_WAF/?cmd=;ls"

Ovom naredbom simulira se command injection napad. Parametru cmd dodaje se znak ;, kojim se pokušava prekinuti legitimna naredba i dodati nova naredba operacijskog sustava. Ako aplikacija ne provjerava pravilno korisnički unos, ovakav napad može rezultirati izvršavanjem proizvoljnih naredbi na poslužitelju.

WAF analizira zahtjev i prepoznaje sumnjivu strukturu parametra, čime sprječava njegovo prosljeđivanje WebGoat aplikaciji.

7.4 Očekivani rezultat simulacije napada

Za sve navedene napade očekivani odgovor WAF-a je HTTP statusni kod:

HTTP/1.1 403 Forbidden

Ovaj statusni kod označava da je zahtjev zabranjen i blokiran od strane Web Application Firewalla. Dobiveni rezultat potvrđuje da je WAF ispravno konfiguriran te da uspješno prepoznaje i blokira napade na aplikacijskoj razini.

---

8. Analiza logova

Nakon izvođenja napada provedena je analiza log zapisa koje generira ModSecurity. Analiza logova omogućuje uvid u način na koji WAF reagira na napade te pruža detaljne informacije o aktiviranim pravilima i ozbiljnosti detektiranih prijetnji.

8.1 Lokacija log datoteka

Log zapisi ModSecurityja nalaze se u sljedećoj datoteci:

/var/log/apache2/modsec_audit.log

Ova datoteka sadrži detaljne zapise o svim zahtjevima koje je WAF analizirao, uključujući i one koji su blokirani.

8.2 Analiza zapisa u logovima

U logovima se analiziraju sljedeći elementi:

Rule ID – identifikator sigurnosnog pravila koje je aktivirano

Message – opis detektirane prijetnje

Severity – razina ozbiljnosti napada

Action – akcija koju je WAF poduzeo (npr. block)

Analizom ovih podataka moguće je točno utvrditi koje je pravilo OWASP CRS-a aktivirano za pojedini napad te potvrditi ispravnost rada WAF-a.

---

9. Automatizirani napadi pomoću OWASP ZAP-a

Osim ručnih testova, provedeni su i automatizirani napadi korištenjem alata OWASP ZAP. Automatizirano testiranje omogućuje simulaciju većeg broja napada u kraćem vremenskom razdoblju te testiranje otpornosti WAF-a pod većim opterećenjem.

U OWASP ZAP alatu kao ciljana adresa postavljena je IP adresa WAF virtualne mašine:

http://IP_WAF

Nakon toga pokrenuti su moduli Spider i Active Scan, koji automatski generiraju i šalju velik broj testnih zahtjeva prema aplikaciji. Tijekom testiranja analizirani su zahtjevi koje je WAF blokirao te zapisi generirani u ModSecurity logovima.

