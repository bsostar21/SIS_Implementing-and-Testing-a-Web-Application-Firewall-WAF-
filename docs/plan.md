U praktičnom dijelu projekta implementirat će se i testirati Web Application Firewall (WAF) u kontroliranom virtualnom okruženju. Cilj ovog dijela projekta je analizirati učinkovitost WAF-a u detekciji i blokiranju web napada te procijeniti njegov utjecaj na sigurnost web aplikacije.

Virtualno okruženje sastojat će se od dvije virtualne mašine. Na prvoj virtualnoj mašini bit će instalirana ranjiva web aplikacija OWASP WebGoat, koja će služiti kao simulacija stvarne web aplikacije izložene napadima. Druga virtualna mašina bit će konfigurirana kao zaštitni sloj te će na njoj biti instaliran Apache web poslužitelj s ModSecurity WAF-om, postavljenim u ulozi reverse proxyja ispred aplikacije.

Na WAF virtualnoj mašini instalirat će se OWASP Core Rule Set (CRS), koji sadrži unaprijed definirana sigurnosna pravila za prepoznavanje i blokiranje najčešćih vrsta web napada, poput SQL Injectiona, Cross-Site Scriptinga (XSS) i drugih poznatih prijetnji.

Napadi na aplikaciju izvodit će se pomoću alata OWASP ZAP, koji omogućuje simulaciju stvarnih napada kroz automatizirane i ručne testove. Tijekom testiranja pratit će se ponašanje WAF-a, odnosno način na koji sustav reagira na zlonamjerne zahtjeve – koje zahtjeve blokira, a koje propušta prema aplikaciji.

Nakon provedenog testiranja izvršit će se analiza prikupljenih podataka, s posebnim naglaskom na identifikaciju false positive i false negative detekcija. Na temelju dobivenih rezultata provest će se prilagodba i optimizacija sigurnosnih pravila s ciljem povećanja preciznosti i učinkovitosti WAF-a.

Na kraju projekta rezultati analize bit će sustavno prikazani kroz tablične i grafičke prikaze, uz popratnu dokumentaciju i zaključke o učinkovitosti implementiranog WAF rješenja.
