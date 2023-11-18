### h4 Totally Legit Sertificate

Opit murtautumaan weppiin. Nyt päästään kokeilemaan niitä hyökkäyksiä, joita päivällä opittiin. Paljon uusia tehtäviä!

Tässä läksyssä on monta uudistettua tehtävää, joita en ehtinyt testata. Jos joku aiheuttaa pulmia, ratkaise se viimeisenä. Käytä työkaluja vain harjoitusmaaleihin, ei muiden ihmisten koneisiin.

#### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**OWASP 2021: OWASP Top 10:2021**

  **A01:2021 – Broken Access Control (IDOR ja path traversal ovat osa tätä)**
  
  - Tosiaan rikkinäinen pääsynhallinta päässyt näköjään OWASP:in listalla ykköseksi vuonna 2021. Yksinkertaisesti rikkinäinen pääsynhallinta tarkoittaa tilanteita, joissa käyttäjät/hyökkääjät saavat käyttöoikeuksia palveluihin, sovelluksiin, ja tietoihin poikkeuksellisilla tavoilla.
  - Yleisiä pääsynhallintaan liittyviä haavoittuvuuksia ovat mm. tarkastusten ohittaminen erinäisin keinoin, API:en huijaaminen eri keinoin, virheet käyttäjäoikeuksissa, elevation of privilege, erilaisten tunnistautumis-tokenien- ja keksien manipuloiminen jne.
  - Ennaltaehkäisykeinot tätä vastaan ovat käytännössä, että zero trust ja hoida tietoturva täydellisesti. Joitain teknisiä keinojakin tuossa oli lueteltu, kuten laittaa 'rate limitt' API pyynnöille minimoidaan automatisoidut hyökkäystyökalut, ja tilalliset kirjautumis-tokenit tulisi poistaa käytöstä uloskirjautumisen jälkeen, ja tilallisten tokenien tulisi olla lyhytikäisiä.

  **A10:2021 – Server-Side Request Forgery (SSRF)**

  - Tämä oli laitettu listalle, koska se oli noussut ykköseksi käyttäjäkyselyssä. SSRF tarkoittaa siis sitä, että käyttäjä pyytää palvelimelta/web-sovellukselta pääsyä johonkin resurssiin, ja palvelin/web-sovellus ei varmista pyyntöä oikein, jolloin päästään lähettämään pyyntöjä sellaisiin kohteisiin, joiden pitäisi olla saavuttamattomissa (esim. muurin, VPN:n, tai muun ACL:n takana).
  - Esimerkkejä tällaisista haavoittuvuuksistusista ovat mm. pääsy sensitiivisen datan luo, pääsy metadataan pilvitallennuspalveluissa, sisäiten palvelinten porttiskannaus, DoS, tai jopa RCE.
  - Ennaltaehkäisyyn tässä ehdotetaan aika perus toimenpiteitä eli mm. verkon segementointia, VPN:ää, ja'deny by default' palomuuri policyjä, mitkä nyt sinällään ovat itsestäänselvyyksiä, mutta niistä saattaa löytyä parannettavaa senkin takia, että arkkitehtuurit alkavat olla aika monimutkaisia useissa ympäristöissä.
  - Sovelluspuolella taas käyttäjältä tuleva data tulisi validoida ja 'puhdistaa', URL:ien muoto, portti ja lähde tulisi mätsätä ACL:ään, tarpeettomat HTTP-uudelleenohjaukset tulisi poistaa jne.

**PortSwigget Academy:**

  **Access control vulnerabilities and privilege escalation (IDOR on osa tätä)**
  
  **Server-side template injection**
  
  **Server-side request forgery (SSRF)**

  - Kuten haavoittuvuuden nimestäkin voi päätellä SSRF:n toimintaperiaate on sellainen, että palvelimelle lähetetään väärennetty pyyntö, jossa viitataan johonkin sisäiseen resurssiin, ja koska pyyntö tulee palvelimen kautta ohittaa se normaalit pääsynhallintatoimet.
  - Yleisiä tapoja käyttää tätä on käyttää sisäisiä ip-osoitteita ja localhostia väärennetyissä pyynnöissä.

  
  **Cross-site scripting**

**Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking**

  
#### a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimi localhost:lla ilman Foxyproxya)

- Tuo Zaproxy löytyi ainakin Kalille suoraan paketinhallinnasta (versio 2.14.0), joten sieltä laittelin vaan: *sudo apt install zaproxy* ja käynnistin sen, ja näemmä Javakin löytyi valmiina Kalista, koska homma lähti toimimaan.
- Tämän jälkeen voidaan proxy käydä lisäämässä selaimeen, ja Zaproxyn default osoite löytyy ohjelman alareunsta tai asetuksista *Local Servers/Proxies* alta. Firefoxiin proxyn saa lisättyä, kun avaa Settings-valikon, ja kirjoittaa hakukenttään proxy ja avaa sieltä settings-kohdan, johon voidaan täyttää käyttämämme proxy ja valita se myös https-proxyksi.
  
  ![zap](https://i.imgur.com/GC4sTK7.png)

![ffproxy](https://i.imgur.com/xPhHaT5.png)kali

![httppss](https://i.imgur.com/KxQB5Ko.png)

- Jotta https toimisi täytyy sitä varten luoda CA-sertifikaatti, ja lisätä se selaimessa luotettuihin serteihin. Sertin hakeminen/luominen Zapissa on simppeliä, eli mennään *Options -> Network -> Server Certificates* ja avautuvassa ikkunassa *Save* ja valitsemaamme paikkaan.
- Tämän jälkeen luotu serti voidaan tuoda Firefoxiin, joka tapahtuu Firefoxin asetuksissa: *Certificates -> View Certificates -> Import* ja etsitään juuri generoimamme serti ja lisätään se, ja raksit ruutuun, että luotamme siihen.
  
  ![htttpssss](https://i.imgur.com/GRjfVfl.png)

#### b) Kettumaista. Asenna FoxyProxy Standard Firefox Addon, ja lisää ZAP proxyksi siihen.

- Perus laajennuksen asentaminen selaimeen, eli oikeasta ylänurkasta palapelikuvaketta -> Hakuun Foxyproxy -> Klikataan add, jonka jälkeen se asentuu ja se voidaan avata samaaa palapelikuvaketta klikkaamalla.
- FoxyProxyn asetuksiin konffataan tuo Zap sinne ja enabloidaan nuo profiilit, jonka jälkeen voidaan testata toimiiko proxy myös localhostilla.
  
  ![fxyprx](https://i.imgur.com/yvyJTZn.png)

  #### c) PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. (Voi käyttää ZAPia, vaikka malliratkaisut käyttävät harjoitusten tekijän maksullista ohjelmaa)

  #### d) Insecure direct object references

 - Tuossa labran kuvauksessa mainitaan, että palvelun chat-lokit tallentuvat suoraan palvelimelle, ja niihin ne käyttävät staattisia osoitteita, joten looginen aloituspaikka oli lähteä tutkailemaan miltä se näyttää. Avataan siis tuo chat, ja lähetetään sinne joku viesti, jonka jälkeen 'trankskriptio' siitä voidaaan hakea painamalla View transcript, ja kun samaan aikaan katsomme selaimen developer-työkaluilla (f12) network-välilehteä näemme, että sieltä lähetetään http GET-pyyntö osoitteeseen, josta lokit haetaan.
 - Osoitteen lopusta '3.txt' voidaan myös tehdä sellainen oletus, että lokissa ei ihan hirveästi tavaraa ole, kun itse lähetimme sinne kaksi viestiä, joten meidän kannattanee kokeilla muuttaa osoitekenttään tuohon loppuun '1.txt' ja katsoa tuottaako se mitään.
 - No sieltähän löytyi keskustelu, jossa joku oli kadottanut salasanansa, ja ystävällinen chat-henkilö sen hänelle sielä antoi takaisin, eli olisiko kyseessä Carlos ja hänen salasanansa. Testataan kirjautua tunnuksella Carlos ja chatistä löytyneellä salasanalla, ja bingobangobongo.
    
    ![asdasd](https://i.imgur.com/hEvimif.png)

    ![bINgO](https://i.imgur.com/X6U76pu.png)

  #### e) File path traversal, simple case

  - Tähän vastaus löytyi myös samaan tapaan kuin aiempaankin tehtävän, eli vihjeen mukaan tuotekuvien näyttäminen mahdollistaa 'path traversalin', joten taas looginen tapa aloittaa on avata selaimessa dev-tools ja network-välilehti ja mennä katsomaan miltä tuo tuotekuvan pyytäminen näyttää. 

       `GET https://0a31009704e5882880868a5b0091008c.web-security-academy.net/image?filename=21.jpg`

  - Tuo pyynnön filename-kohta näyttää siltä, että se varmaankin on haavoittuvainen tälle hyökkäykselle, joten voimme kokeilla muuttaa siihen vaikkapa *../../../etc/passwd*, joka siis menee unix-systeemissä ensiksi root-hakemistoon ja sieltä etc/passwd-tiedostoon, jos tätä vastaan ei ole suojaduttu mitenkään, ja kappas kummaa eipä ollutkaan.

       ![124345](https://i.imgur.com/uwpXxL5.png)

  #### f) File path traversal, traversal sequences blocked with absolute path bypass

- En tiedä miksi tämä ei näy tuolla Zapin pääikkunassa, vaan ainoastaan tässä jäte hudissa, jota en osaa käyttää xD. Tuohon filenamen kohdalle varmaan pitäisi muokkailla jotenkin sitä etc/passwd, jotta saisi ratkaistua...

  ![asdas](https://i.imgur.com/WHxqLYE.png)
     
  #### g) File path traversal, traversal sequences stripped non-recursively

  #### h) Basic SSRF against the local server
  
- Ideana tässä on hyväksikäyttää API-pyyntöä, joka tarkastaa tuotteen saatavuuden hakemalla dataa sisäisestä järjestelmästä. Aloitetaan siis sillä, että mennään jollekkin tuotesivulle ja klikataan tuota 'Check stock' -painiketta, ja sen jälkeen etsitään kyseinen pyyntö Zapista ja tarkastellaan sitä.

  ![api](https://i.imgur.com/PWnF9dv.png)

- Tässä meitä kiinnostava kohta on tuoa stockApi -pyyntö, jota muokkaamalla voisimme saada pääsyn sisäisiin resursseihin. Eli m2 tähän pyyntöön, ja avautuvasta listasta valitaan *Open/Resend with Request Editor*, jolloin kyseistä kohtaa päästään muokkaamaan ja lähettämään väärennetty pyyntö.

  ![asdasdad](https://i.imgur.com/qFIKSzq.png)

- Kun katsotaan vastausta tähän meidän väärentämäämme pyyntöön voidaan sen htlm:stä löytää kohta, mistä poistetaan käyttäjä carlos. Seuraavaksi siis lisätään tuo tuohon meidän aiemmin lähettämämme väärennetyn pyynnön perään, niin käyttäjän carlos pitäisi poistua keskuudestamme, ja sehän toimi ja labra on ratkaistu.

  `
  <a href="/admin/delete?username=carlos">Delete</a>
  `

![ripcarlos](https://i.imgur.com/8GpaqJi.png)

#### i) Reflected XSS into HTML context with nothing encoded

- Tarkoituksena on päästä suorittamaan omaa skriptiä web-sovelluksen hakutoimintoa hyväksikäyttäen. Alkuun taas Zap auki ja etsitään tuo suorittamamme pyyntö.

  `GET https://0ad400c303de825c803aa891004200d9.web-security-academy.net/?search=123 HTTP/1.1`
  
- Jos siis tästä tuota *search=* ei ole suojattu mitenkään voidaan siihen sijoittaa omaa skriptiä, ja lähettää pyyntö ja saadaan url, josta tuo luomamme skripti ajetaan käyttäjällä. Tässä tapauksessa tuohon laitettiin, vain *<script>alert()</script>*.

  ![asd12341](https://i.imgur.com/JY0poOV.png)

 #### j) Stored XSS into HTML context with nothing encoded

 - Tässä sama idea kuin aiemmassa tehtävässä paitsi, että haitallinen skripti sijoitetaan kommenttikenttään ja se näkyy kaikille sivun avaaville. Eli taas kun etsimme tuon jättämämme kommentin http-requestin Zapista ja löydämme sieltä *comment=* -kohdan, johon voimme taas syöttää script-tagien sisään settiä.

   ![asda](https://i.imgur.com/AgVs2Xt.png)
   
   ![1234124](https://i.imgur.com/sKFMEfM.png)

#### k) Asenna Webgoat 2023.4

- Webgoat on siis web-sovellus harjoitusmaali, jossa on Java-pohjaisia haavoittuvuuksia joka lähtöön.
- Päätin asentaa tuon docker-kontin, kun se nyt varmaankin on helpoin tapa. Eli kopioin seurasin WebGoatin sivuilta (https://owasp.org/www-project-webgoat/) downloads-kohdan alta löytyvää linkkiä, josta löytyi komennot docker-imagen lataamiseen ja käynnistämiseen. Asennuksessa ja käynnistämisessä ei sinällään mitään ongelmaa, paitsi että asennnuksen jälkeen WebGoat ei suostunut aukeamaan Zaproxyllä.
  
   ![tllls](https://i.imgur.com/TVzDkvB.png)

- Kokeilin virheilmoituksen urlissa mainittua Java policyjen lataamista ja päivittämistä (curl -L -b "oraclelicense=a" http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip), mutta ei tuottanut tulosta ainakaan heti. Tämän jälkeen tilteissä vähän käynnistelin uudestaan Zapia ja Firefoxia, ja kokeilin pelkällä http tuota WebGoatin osoitetta, mikä toimi ja tämän jälkeen osoitekenttäänkin päivittyi https(??), ei järkeä. Tai miksi toi tls-handshake yhtäkkiä alkaisi toimimaan, ei kai siinä järjestyksellä voi olla väliä.

   ![asdasda](https://i.imgur.com/8Dxq1Xd.png)

#### m) (A1) Broken Access Control (WebGoat 2023.4)
**Hijack a session (1)**

-Ei kyllä nyt soita kelloja, et mitä tästä pitäis saadad irti.

![asda](https://i.imgur.com/m1ZG5NA.png)

**Insecure Direct Object References (4)**
1. Authenticate First, Abuse Authorization Later
   - En nyt taas ihan ymmärrä mitä tässä haetaan? Siis varmaan tuohon pyyntöön noiden usernamen & passwordin jälkeen koittaisin lisätä vaikka role=admin (ei tapahdu mitään?).

     ![asdasd](https://i.imgur.com/1PzOAPa.png)

**Missing Function Level Access Control (3)**

**Spoofing an Authentication Cookie (1)**

#### n) (A7) Identity & Auth Failure (WebGoat 2023.4)
Authentication Bypasses (1)
Insecure Login (1)

#### o) (A10) Server-side Request Forgery (WebGoat 2023.4)
Server-Side Request Forgery (2)

![tom](https://i.imgur.com/UySNU6R.png)

#### p) Client side (WebGoat 2023.4)
Bypass front-end restrictions (2)

   ![asdas](https://i.imgur.com/a05wuDX.png)
