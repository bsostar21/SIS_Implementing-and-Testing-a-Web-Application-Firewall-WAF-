## Postavljanje okruženja

Za izradu i testiranje laboratorijskog okruženja korišten je alat za virtualizaciju **Oracle VM VirtualBox**. VirtualBox omogućuje pokretanje više virtualnih mašina na jednom fizičkom računalu te je pogodan za izgradnju izoliranih testnih okruženja.

### Preuzimanje VirtualBoxa

VirtualBox je preuzet sa službene web stranice proizvođača:

https://www.virtualbox.org/wiki/Downloads

Na stranici za preuzimanje odabrana je verzija VirtualBoxa koja odgovara operacijskom sustavu domaćina (host OS), u ovom slučaju sustav Windows. Korištenje službene stranice osigurava preuzimanje najnovije stabilne verzije alata te smanjuje rizik od sigurnosnih problema.

### Instalacija VirtualBoxa

Nakon preuzimanja instalacijske datoteke, pokrenut je instalacijski postupak te su korištene zadane (default) postavke instalacije. Tijekom instalacije VirtualBox automatski instalira potrebne virtualne mrežne adaptere koji su kasnije korišteni za konfiguraciju izolirane mreže između virtualnih mašina.

Po završetku instalacije provjereno je uspješno pokretanje aplikacije VirtualBox Manager, čime je potvrđeno da je sustav spreman za daljnje korake u izradi virtualnog laboratorija.

VirtualBox je zatim korišten za kreiranje i upravljanje virtualnim mašinama koje čine arhitekturu sustava, uključujući konfiguraciju mreže, dodjelu resursa te pokretanje testnog okruženja.

## Mrežna konfiguracija

Kako bi se omogućila komunikacija između virtualnih mašina, a istovremeno zadržala izolacija od vanjske mreže, u VirtualBoxu je konfigurirana **NAT Network** mreža. Ovakav tip mreže omogućuje svim virtualnim mašinama međusobnu komunikaciju unutar istog subnet-a, kao i pristup internetu prema potrebi.

### Kreiranje NAT Networka

U VirtualBox Manageru otvoren je izbornik **Tools → Network → NAT Networks**, gdje je kreirana nova NAT mreža sa sljedećim postavkama:

- **Name:** `waf_lab`
- **IPv4 Prefix:** `195.168.2.0/24`
- **DHCP Server:** Enabled

<img width="913" height="679" alt="image" src="https://github.com/user-attachments/assets/19b3a637-47a5-4dd4-90d1-d7cb8dcd3ea5" />
<br>
Slika 1. Postavljanje NAT Network-a
<br>
<br>
Postavljanjem vlastitog IPv4 raspona osigurano je da sve virtualne mašine budu unutar istog subnet-a (`195.168.2.X`), što je nužno za ispravnu komunikaciju između Attack VM-a, WAF VM-a i WebGoat VM-a. Omogućavanjem DHCP poslužitelja virtualnim mašinama se automatski dodjeljuju IP adrese, čime se pojednostavljuje mrežna konfiguracija.

### Mrežne postavke virtualnih mašina

Nakon kreiranja NAT Networka, na svakoj virtualnoj mašini postavljene su iste mrežne postavke. Za sve virtualne mašine (Attack VM, WAF VM i WebGoat VM) korišten je isti mrežni adapter:

- **Adapter:** Adapter 3  
- **Attached to:** NAT Network  
- **Name:** `waf_lab`

<img width="1002" height="659" alt="image" src="https://github.com/user-attachments/assets/48d9cfc1-ab76-49ce-84cb-3bf1128cb509" />
<br>
Slika 2. Podešavanje mrežnih postavki
<br>
<br>
Korištenjem iste NAT mreže za sve virtualne mašine osigurano je da se sve nalaze u istom mrežnom segmentu te da mogu međusobno komunicirati bez dodatnih pravila rutiranja ili ručne konfiguracije IP adresa.

### Mrežna arhitektura

Ovakva mrežna konfiguracija omogućuje sljedeće:
- Attack VM može slati zahtjeve prema WAF VM-u
- WAF VM može prosljeđivati promet prema WebGoat aplikaciji
- Sav promet prolazi kroz WAF, što omogućuje analizu i filtriranje zahtjeva
- Cijelo okruženje ostaje izolirano od vanjske mreže

Time je uspostavljena stabilna i kontrolirana mrežna infrastruktura koja predstavlja temelj za daljnju implementaciju, testiranje i analizu ponašanja Web Application Firewalla.

## WebGoat – instalacija i pokretanje ranjive web aplikacije

Za potrebe testiranja Web Application Firewalla korištena je namjerno ranjiva web aplikacija **OWASP WebGoat**. WebGoat je edukacijska aplikacija razvijena od strane OWASP zajednice i sadrži velik broj poznatih sigurnosnih ranjivosti, što je čini idealnom za testiranje i demonstraciju zaštitnih mehanizama poput WAF-a.

WebGoat je instaliran na zasebnoj virtualnoj mašini (WebGoat VM), koja u arhitekturi sustava predstavlja cilj napada.

### Instalacija potrebnih paketa

Prije instalacije WebGoat aplikacije izvršeno je ažuriranje paketa te instalacija potrebnih ovisnosti. Budući da je WebGoat Java aplikacija, potrebno je instalirati Java Development Kit (JDK).

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk wget
```

Instalacijom OpenJDK-a omogućeno je pokretanje WebGoat aplikacije, dok je alat wget korišten za preuzimanje aplikacije s GitHub repozitorija.

### Preuzimanje WebGoat aplikacije

WebGoat aplikacija preuzeta je sa službenog GitHub repozitorija OWASP projekta u obliku .jar datoteke:
```bash
wget https://github.com/WebGoat/WebGoat/releases/download/v8.2.2/webgoat-server-8.2.2.jar
```

Korištena je stabilna verzija WebGoat aplikacije kako bi se osigurala kompatibilnost i predvidivo ponašanje tijekom testiranja.

### Pokretanje WebGoat aplikacije
Nakon preuzimanja, WebGoat je pokrenut pomoću Java naredbe uz definirane mrežne postavke:
```bash
java -jar webgoat-server-8.2.2.jar --server.address=0.0.0.0 --server.port=8080
```
Postavka --server.address=0.0.0.0 omogućuje pristup aplikaciji s drugih virtualnih mašina unutar mreže, dok je aplikacija pokrenuta na portu 8080.

### Provjera dostupnosti
Nakon pokretanja, provjereno je ispravno izvođenje aplikacije pristupom WebGoat sučelju putem web preglednika:
```bash
http://195.168.2.6:8080
```
<img width="853" height="524" alt="image" src="https://github.com/user-attachments/assets/e5fe8add-ea9b-4cdd-befd-e25d4fdedd13" />
<br>
Slika 3. WebGoat u pregledniku
<br>
<br>
Uspješnim učitavanjem WebGoat početne stranice potvrđeno je da je aplikacija ispravno instalirana i dostupna unutar virtualne mreže. Time je WebGoat VM spreman za daljnje korake testiranja i integraciju s WAF virtualnom mašinom.

## WAF – instalacija, konfiguracija i testiranje

Web Application Firewall (WAF) implementiran je na zasebnoj virtualnoj mašini (WAF VM) i postavljen kao zaštitni sloj ispred ranjive WebGoat aplikacije. WAF je realiziran korištenjem **Apache HTTP Servera** u kombinaciji s modulom **ModSecurity** i pravilima iz **OWASP Core Rule Seta (CRS)**.

### Instalacija Apache web poslužitelja i ModSecurityja

Na WAF virtualnoj mašini prvo je izvršeno ažuriranje paketa te instalacija potrebnih komponenti:

```bash
sudo apt update
sudo apt install -y apache2 libapache2-mod-security2 modsecurity-crs
```
Ovim korakom instaliran je:
- Apache HTTP Server
- ModSecurity modul za Apache
- OWASP Core Rule Set (CRS)

Nakon instalacije provjereno je da je ModSecurity modul ispravno učitan:
```bash
sudo apache2ctl -M | grep security
```

### Konfiguracija ModSecurity-a
Po defaultu, ModSecurity dolazi s preporučenom konfiguracijskom datotekom. Kako bi se omogućio aktivni način rada, konfiguracija je kopirana i prilagođena.

```bash
sudo cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
sudo nano /etc/modsecurity/modsecurity.conf
```
Unutar konfiguracijske datoteke postavljena je sljedeća direktiva:
```bash
SecRuleEngine On
```
<img width="521" height="445" alt="image" src="https://github.com/user-attachments/assets/383c2e51-a48a-465e-81d0-f704e2dc0f76" />
<br>
Slika 4. Konfiguracija ModSecurity datoteke
<br>
<br>
Postavljanjem opcije SecRuleEngine On omogućuje se aktivno blokiranje zlonamjernih zahtjeva, umjesto samo njihovog bilježenja.
Nakon izmjena, Apache servis je ponovno pokrenut:
```bash
sudo systemctl restart apache2
```

### Aktivacija OWASP Core Rule Seta (CRS)
OWASP CRS sadrži skup unaprijed definiranih sigurnosnih pravila za detekciju najčešćih web napada. Kako bi se pravila učitala u Apache, potrebno ih je uključiti u ModSecurity konfiguraciju.

U datoteci /etc/apache2/mods-enabled/security2.conf dodana je sljedeća linija:
```bash
sudo nano /etc/apache2/mods-enabled/security2.conf
```
```bash
IncludeOptional /usr/share/modsecurity-crs/owasp-crs.load
```
<img width="519" height="444" alt="image" src="https://github.com/user-attachments/assets/b3d756eb-757d-436a-8e67-3a79cf13f87f" />
<br>
Slika 5. Aktivacija CRS-a
<br>
<br>
Nakon dodavanja CRS pravila, Apache je ponovno pokrenut:
```bash
sudo systemctl restart apache2
```
Time je osigurano da ModSecurity koristi OWASP CRS pravila za analizu dolaznog prometa.

### Konfiguracija reverse proxyja
Kako bi sav promet prema WebGoat aplikaciji prolazio kroz WAF, Apache je konfiguriran kao reverse proxy. Time WAF postaje jedina ulazna točka prema aplikaciji.

U datoteci /etc/apache2/sites-enabled/000-default.conf dodana je proxy konfiguracija:
```bash
sudo nano /etc/apache2/sites-enabled/000-default.conf
```
```bash
ProxyPreserveHost On
ProxyPass / http://IP_WEBGOAT:8080/WebGoat
ProxyPassReverse / http://IP_WEBGOAT:8080/WebGoat
```
<img width="540" height="464" alt="image" src="https://github.com/user-attachments/assets/50756f19-c05b-4d73-96c0-68d30fb6dddf" />
<br>

Slika 6. Proxy konfiguracija
<br>
<br>
Ovom konfiguracijom svi HTTP zahtjevi koji dolaze na WAF prosljeđuju se WebGoat aplikaciji, dok se istovremeno analiziraju i filtriraju kroz ModSecurity i OWASP CRS pravila.

Nakon izmjena, Apache je ponovno pokrenut:
```bash
sudo systemctl restart apache2
```
