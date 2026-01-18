## Analiza rezultata – usporedba sustava bez i s uključenim WAF-om

U završnoj fazi projekta provedena je analiza ponašanja sustava u dva scenarija:  
1) bez uključenog Web Application Firewalla (WAF)  
2) s uključenim WAF-om temeljenim na Apache + ModSecurity + OWASP CRS arhitekturi  

Cilj analize bio je procijeniti utjecaj WAF-a na sigurnost web aplikacije te usporediti sposobnost sustava da razlikuje zlonamjerni i legitimni promet.

---

### Analiza sustava bez uključenog WAF-a

U scenariju bez WAF-a ModSecurity je bio isključen (`SecRuleEngine Off`), a sav HTTP promet bio je izravno prosljeđivan WebGoat aplikaciji.

Rezultati testiranja pokazali su sljedeće:
- XSS i SQL Injection zahtjevi nisu blokirani na razini web poslužitelja
- ne postoji mehanizam za prepoznavanje zlonamjernih uzoraka u dolaznim zahtjevima
- sustav se u potpunosti oslanja na sigurnost same aplikacije
- potencijalni napadi mogu doći do aplikacijske logike bez ikakve prethodne filtracije

---

### Analiza sustava s uključenim WAF-om

U drugom scenariju ModSecurity je aktiviran u blocking načinu rada (`SecRuleEngine On`), a OWASP Core Rule Set korišten je za analizu i filtriranje HTTP prometa.

Rezultati testiranja s uključenim WAF-om pokazali su:
- XSS i SQL Injection napadi su pravovremeno detektirani i blokirani (HTTP 403 Forbidden)
- zlonamjerni zahtjevi nisu proslijeđeni WebGoat aplikaciji
- legitiman promet je dopušten i ispravno obrađen
- filtriranje se provodi selektivno, bez narušavanja funkcionalnosti aplikacije

---

### Usporedna analiza (bez WAF-a vs. s WAF-om)

| Karakteristika                | Bez WAF-a                         | S uključenim WAF-om               |
|------------------------------|-----------------------------------|-----------------------------------|
| Filtriranje HTTP prometa     | Ne                                | Da                                |
| Detekcija XSS napada         | Ne                                | Da                                |
| Detekcija SQL Injectiona     | Ne                                | Da                                |
| Blokiranje napada            | Ne                                | Da (403 Forbidden)                |
| Propusnost legitimnog prometa| Da                                | Da                                |
| Ovisnost o aplikaciji        | Visoka                            | Smanjena                          |
| Razina sigurnosti sustava    | Niska                             | Značajno povećana                 |

Usporedba jasno pokazuje da sustav s uključenim WAF-om pruža višestruko višu razinu sigurnosti u odnosu na sustav bez dodatnog zaštitnog sloja.

---

## Zaključak

U ovom projektu uspješno je implementiran i testiran Web Application Firewall u virtualnom okruženju korištenjem Apache web poslužitelja, ModSecurity modula i OWASP Core Rule Seta. Provedeno testiranje pokazalo je da WAF predstavlja učinkovit mehanizam za zaštitu web aplikacija od najčešćih web napada, poput Cross-Site Scripting (XSS) i SQL Injection napada.

Analiza rezultata jasno ukazuje da sustav bez WAF-a ostaje značajno ranjiv, dok primjena WAF-a omogućuje rano zaustavljanje zlonamjernog prometa prije nego što dođe do same aplikacije. Time se smanjuje napadna površina sustava i povećava ukupna razina sigurnosti web aplikacije.

Međutim, iako WAF značajno doprinosi sigurnosti, on nije univerzalno niti dugoročno rješenje za zaštitu web aplikacija. WAF djeluje kao dodatni zaštitni sloj koji može ublažiti ili zaustaviti poznate obrasce napada, ali ne uklanja temeljni uzrok problema – ranjivosti u samoj aplikaciji. Dugoročno gledano, najučinkovitiji pristup sigurnosti jest razvoj aplikacija koje su od samog početka dizajnirane kao sigurne, kroz primjenu sigurnog razvojnog procesa i uklanjanje ranjivosti u izvornom kodu.

U tom kontekstu, WAF treba promatrati kao važnu dopunu, a ne zamjenu za sigurnu aplikaciju. Kao dio višeslojnog sigurnosnog pristupa (defense in depth), pravilno konfiguriran i optimiziran WAF može značajno povećati otpornost sustava, dok istovremeno očuvanje sigurnosti aplikacije mora ostati primarna odgovornost razvojnog procesa.

