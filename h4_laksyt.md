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

- Tuo zaproxy löytyi ainakin Kalille suoraan paketinhallinnasta (versio 2.14.0), joten sieltä laittelin vaan: *sudo apt install zaproxy* ja käynnistin sen, ja näemmä Javakin löytyi valmiina Kalista, koska homma lähti toimimaan.
  
  ![zap](https://i.imgur.com/GC4sTK7.png)

- Tämän sai toimimaan myös lokaalisti vaihtamalla tuon Zapin Main Proxyn ip-osoitteen käyttämään koneen sisäverkon ip:tä (asetus löytyy valikosta *Tools -> Options -> Network*), ja asettamalla sen myös selaimeen.

![proxy](https://i.imgur.com/4ZqwgDk.png)

![proxy1](https://i.imgur.com/rVxFzSb.png)

![httppss](https://i.imgur.com/KxQB5Ko.png)

- Vielä että https toimisi oikein täytyy sitä varten luoda CA-sertifikaatti, ja lisätä se selaimessa luotettuihin serteihin. Uuden sertin luominen Zapissa on simppeliä, eli mennään *Options -> Network -> Server Certificates* ja avautuvassa ikkunassa *Generate* ja sitten tallennetaan haluamaamme paikkaan.
- Tämän jälkeen luotu serti voidaan tuoda Firefoxiin, joka tapahtuu Firefoxin asetuksissa: *Certificates -> View Certificates -> Import etsitään juuri generoimamme serti ja lisätään se*.
- 
