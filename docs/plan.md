## Praktični dio – implementacija i testiranje WAF-a

U praktičnom dijelu projekta implementira se i testira Web Application Firewall (WAF) u kontroliranom virtualnom okruženju. Cilj ovog dijela projekta je analizirati učinkovitost WAF-a u detekciji i blokiranju web napada te identificirati njegove prednosti i ograničenja u stvarnim uvjetima.

### Postavljanje virtualnog okruženja

Testno okruženje sastoji se od dvije virtualne mašine postavljene pomoću alata za virtualizaciju kao što su **VirtualBox** ili **VMware**:

- **Virtualna mašina s web aplikacijom**  
  Na ovoj virtualnoj mašini instalirana je ranjiva web aplikacija **OWASP WebGoat**, koja služi kao simulacija stvarne web aplikacije s poznatim sigurnosnim ranjivostima.

- **Virtualna mašina s WAF-om**  
  Druga virtualna mašina konfigurirana je kao zaštitni sloj ispred aplikacije. Na njoj je instaliran **Apache HTTP Server s ModSecurity WAF-om**, koji je postavljen u ulozi **reverse proxyja** i analizira sav dolazni HTTP promet.

### Konfiguracija WAF-a

Na virtualnoj mašini s WAF-om instaliran je i aktiviran **OWASP Core Rule Set (CRS)**. CRS sadrži unaprijed definirana sigurnosna pravila namijenjena prepoznavanju i blokiranju najčešćih vrsta web napada, uključujući:

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Command Injection
- Path Traversal i druge web napade

ModSecurity je konfiguriran u **aktivnom (blocking) načinu rada**, što znači da se zlonamjerni zahtjevi automatski blokiraju, a ne samo bilježe u zapisnike.

### Simulacija napada

Za testiranje učinkovitosti WAF-a koriste se napadi simulirani pomoću alata **OWASP ZAP (Zed Attack Proxy)**. Izvode se automatizirani sigurnosni skenovi, kao i ručni pokušaji napada, kako bi se što vjernije simulirali stvarni napadi na web aplikaciju.

### Praćenje i analiza

Tijekom testiranja prikupljaju se zapisi (logovi) WAF-a i analiziraju se reakcije sustava na dolazne zahtjeve. Analiza uključuje:

- identifikaciju zahtjeva koje je WAF uspješno blokirao
- identifikaciju zahtjeva koje je WAF propustio prema aplikaciji
- prepoznavanje **false positive** slučajeva, gdje su legitimni zahtjevi pogrešno blokirani
- prepoznavanje **false negative** slučajeva, gdje zlonamjerni zahtjevi nisu prepoznati

Na temelju dobivenih rezultata provodi se prilagodba i optimizacija sigurnosnih pravila s ciljem povećanja preciznosti WAF-a, uz zadržavanje funkcionalnosti aplikacije.

### Rezultati i dokumentacija

Rezultati testiranja prikazuju se pomoću **tablica i grafičkih prikaza**, koji omogućuju jasan uvid u ponašanje i učinkovitost WAF-a prije i nakon optimizacije pravila. Sva konfiguracija, testni scenariji, zapisi i rezultati dokumentirani su unutar **GitHub repozitorija**, čime se osigurava transparentnost, ponovljivost i kvaliteta projekta.
