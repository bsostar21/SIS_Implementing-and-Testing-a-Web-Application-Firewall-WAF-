## Tesitranje

### Testiranje bez uključenog WAF-a
Kako bi se usporedilo ponašanje sustava, prvo je testirano stanje bez aktivnog WAF-a. U konfiguraciji ModSecurityja postavljena je opcija:
```bash
SecRuleEngine Off
```
U tom stanju zlonamjerni HTTP zahtjevi nisu blokirani te se normalno prosljeđuju WebGoat aplikaciji, što potvrđuje ranjivost sustava bez WAF zaštite.
<img width="1161" height="821" alt="image" src="https://github.com/user-attachments/assets/e3fbfb2c-d596-49d0-bb44-27f6bab62ec3" />
<br>
Slika 7. Zahtjevi kada je WAF isključen
<br>
<br>
### XSS napad bez WAF zaštite
```bash
curl -iG --data-urlencode "q=<script>alert(1)</script>" http://195.168.2.7/
```
Ovim zahtjevom simuliran je pokušaj reflektiranog XSS napada, gdje se JavaScript kod umeće u HTTP parametar q. Budući da WAF nije aktivan, zahtjev nije blokiran na razini web poslužitelja.
Aplikacija vraća HTTP odgovor 302 Found, što označava preusmjeravanje na login stranicu WebGoat aplikacije. Iako se JavaScript kod u ovom slučaju ne izvršava zbog same logike aplikacije, zahtjev nije filtriran niti zaustavljen, što potvrđuje izloženost sustava potencijalnim XSS napadima.

### SQL Injection napad bez WAF zaštite
```bash
curl -I "http://195.168.2.7/?id=%27%20OR%201%3D1%20--%20"
```
U ovom primjeru parametar id sadrži klasičan SQL Injection uzorak. Bez aktivnog WAF-a, zahtjev nije blokiran niti prepoznat kao zlonamjeran na razini web poslužitelja.
Aplikacija ponovno odgovara preusmjeravanjem (302 Found), što ukazuje na to da ne postoji dodatni zaštitni mehanizam koji bi spriječio pokušaj SQL Injection napada prije nego što dosegne aplikaciju.

### Legitiman zahtjev bez WAF zaštite
```bash
curl -I "http://195.168.2.7/?q=hello"
```
Legitiman HTTP zahtjev prolazi bez ikakvih ograničenja te se normalno prosljeđuje prema WebGoat aplikaciji. 
Ovo ponašanje je očekivano, ali u kombinaciji s prethodnim testovima pokazuje da sustav bez WAF-a ne razlikuje zlonamjerne i legitimne zahtjeve.

## Testiranje s uključenim WAF-om
Nakon ponovne aktivacije WAF-a (SecRuleEngine On), isti zahtjevi poslani su ponovno.
<img width="1135" height="800" alt="image" src="https://github.com/user-attachments/assets/0c44dc61-a266-4620-8b03-39c4bca8025f" />
<br>
Slika 8. Zahtjevi kada je WAF uključen
<br>
<br>
U ovom slučaju WAF je uspješno detektirao i blokirao zlonamjerne zahtjeve, što je rezultiralo HTTP odgovorima tipa 403 Forbidden.
