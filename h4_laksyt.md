### h4 Totally Legit Sertificate

Opit murtautumaan weppiin. Nyt päästään kokeilemaan niitä hyökkäyksiä, joita päivällä opittiin. Paljon uusia tehtäviä!

Tässä läksyssä on monta uudistettua tehtävää, joita en ehtinyt testata. Jos joku aiheuttaa pulmia, ratkaise se viimeisenä. Käytä työkaluja vain harjoitusmaaleihin, ei muiden ihmisten koneisiin.

#### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**OWASP 2021: OWASP Top 10:2021**

  **A01:2021 – Broken Access Control (IDOR ja path traversal ovat osa tätä)**

  **A10:2021 – Server-Side Request Forgery (SSRF)**

**PortSwigget Academy:**

  **Access control vulnerabilities and privilege escalation (IDOR on osa tätä)**
  
  **Server-side template injection**
  
  **Server-side request forgery (SSRF)**
  
  **Cross-site scripting**

**Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking**

  
#### a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimi localhost:lla ilman Foxyproxya)

- Tuo Zaproxy löytyi ainakin Kalille suoraan paketinhallinnasta (versio 2.14.0), joten sieltä laittelin vaan: *sudo apt install zaproxy* ja käynnistin sen, ja näemmä Javakin löytyi valmiina Kalista, koska homma lähti toimimaan.
- Tämän jälkeen voidaan proxy käydä lisäämässä selaimeen, ja Zaproxyn default osoite löytyy ohjelman alareunsta tai asetuksista *Local Servers/Proxies* alta. Firefoxiin proxyn saa lisättyä, kun avaa Settings-valikon, ja kirjoittaa hakukenttään proxy ja avaa sieltä settings-kohdan, johon voidaan täyttää käyttämämme proxy ja valita se myös https-proxyksi.
  
  ![zap](https://i.imgur.com/GC4sTK7.png)

![ffproxy](https://i.imgur.com/xPhHaT5.png)

![httppss](https://i.imgur.com/KxQB5Ko.png)

- Jotta https toimisi täytyy sitä varten luoda CA-sertifikaatti, ja lisätä se selaimessa luotettuihin serteihin. Uuden sertin luominen Zapissa on simppeliä, eli mennään *Options -> Network -> Server Certificates* ja avautuvassa ikkunassa *Save* ja valitsemaamme paikkaan.
- Tämän jälkeen luotu serti voidaan tuoda Firefoxiin, joka tapahtuu Firefoxin asetuksissa: *Certificates -> View Certificates -> Import* ja etsitään juuri generoimamme serti ja lisätään se, ja raksit ruutuun, että luotamme siihen.
- 
