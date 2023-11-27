## h5 Injected Sequel
*Nyt olisi tarjolla OWASP 2017 numero uno, SQL injektio. Kannattaa aloittaa saman tien, niin ehdit ratkoa tehtäviä monena päivänä.*



### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**Karvinen 2016: PostgreSQL Install and One Table Database – SQL CRUD tutorial for Ubuntu (Virtuaaliympäristöä ei tarvita, voit aloittaa kohdasta "Three line install"**



**OWASP 2017: A1:2017-Injection**

- 2017 injektiohyökkäykset olivat sijalla yksi tuolla OWASP:in top10-listalla, ja tässä taulussa esitellään ne pääpiirteissään.
- Ensimmäisenä mainitaan se tosiasia, että käytännössä aina kun voidaan syöttää dataa, siinä on myös mahdollisuus injektiolle. Näitä injektiohaavoittuvaisuuksia löytyy etenkin 'legacy-koodista', ja erilaisita tietokantahauista, sekä muista tiedonsyöttö/haku -metodeista.
- Seurauksten vakavuus injektiohyökkäyksissä vaihtelee, ja se on riippuvainen siitä kuinka kriittistä kohteena oleva data on.
- Yleisesti ottaen sovellus on haavoittuvainen injektiolle, jos siihen syötettyä dataa ei validoida, filtteröidä, tai 'siivota' mitenkään. Tai jos ohjelma salii erilaisten hakujen suorittamisen konteksista riippumatta.

**PortSwigger Academy: SQL injection**
*Kaikki muut luvut paitsi ei "Blind SQL injection" Tämä kappale kannattaa pitää näkyvissä injektioita tehdessä SQL injection cheat sheet*

- SQL-injektiossa hyökkääjä käyttää sovelluksen tietokantahakuja 'epärehdillä' tavalla päästäkseen käsiksi tietoihin, jotka normaaleissa olosuhteissa olisivat saavuttamattomissa (esim. salasanat, käyttäjätiedot etc.). Joissain tapauksissa SQL-injektio voi johtaa palvelimen tai muun infran vaarantumiseen, ja se voi myös mahdollistaa palvelunestohyökkäyksiä.
- Useimmat SQL-injektiot suoritetaan *SELECT*-kyselyn *WHERE*-osassa, jonka muokkaamisesta artikkelissa on muutama esimerkkikikka.
- SQL-injektiolla voidaan saada selville tietoa, joka ei pitäisi olla saatavissa, tai sillä voidaan myös ohittaa ohjelman logiikka, tai suorittaa hakuja muihinkin tietokannan tauluihin.
- Vaikkakin useat perustoiminnot on toteutettu erilaisissa SQL-tietokannoissa samoin, jonka johdosta myös haavoittuvuudetkin voivat usein olla samoja, on myös huomioitava että niissä on myös eroja syntaksin ym. suhteen.
  
**Vapaaehtoinen: Karvinen 2019: MitmProxy on Kali and Xubuntu – attack and testing (Nykyisin asennus 'sudo apt-get install mitmproxy')**

- Seurasin Teron [ohjetta](https://terokarvinen.com/2019/05/22/mitmproxy-on-kali-and-xubuntu-attack-and-testing/?fromSearch=mitmproxy), ja asennuksessa ei ollut mitään sen ihmeellisempää. Sertin asentamiseen löytyi nopealla googlellla [ohje](https://docs.mitmproxy.org/stable/concepts-certificates/) mitm-proxyn dokumentaatiosta, eli ~/.mitmproxy -kansiosta löytyy tuo tarvittava ca.cert -tiedosto, joka voidaan viedä selaimeen.
  
  ![mitm](https://i.imgur.com/WOEfHPv.png)

### a) CRUD. Tee uusi PostgreSQL-tietokanta ja demonstroi sillä create, read, update, delete (CRUD). Keksi taulujen ja kenttien nimet itse. Taulujen nimet monikossa, kenttien nimet yksikössä, molemmat englanniksi.

- Alkuun ylempänä olevaa Teron ohjetta seuraten apista install ja sen jälkeen käynnistys systemctl startilla, jonka jälkeen nuo tietokannan ja käyttäjän luominen. Molemmat noista komennoista antoi valituksen *could not change directory to "/home/vilppu": Permission denied*, mutta katsotaan vaikuttaako se mihinkään, eli psql auki ja yritetään yhdistää tuohon luomaamme tietokantaan *\c vilppu vilppu*, ja näemmä se oli sinne ihan oikein luotu.

    ![psqlll](https://i.imgur.com/tiUa5Wa.png)

- Kun yrittää luoda taulua tuohon tietokantaan antaa *ERROR: permission denied for schema public*. Googlella löytyi ainakin tämä [artikkeli](https://www.cybertec-postgresql.com/en/error-permission-denied-schema-public/) ongelmaan liittyen, mutta en nyt ainkaan nopeasti tuosta keksinyt ratkaisua tähän, joten antaa nyt ajankäytön vuoksi olla. Varmaan helpointa olisi ladata vanhempi versio tuosta Postgresql:stä, koska artikkelissa mainitaan että versioon 14 asti kuka tahansa on voinut luoda taulun tuohon public schemaan.
- **Maantantai 27. 11 edit:** 

### b) SQLi me. Kuvaile yksinkertainen SQL-injektio, ja demonstroi se omaan tietokantaasi psql-komennolla. Selitä, mikä osa on käyttäjän syötettä ja mikä valmiina ohjelmassa. (Tässä harjoituksessa voit vain kertoa koodista, ei siis tarvitse välttämättä koodata sitä ohjelmaa, joka yhdistää käyttäjän syötteen SQL:n)

- ?

### PortSwigger Labs

#### ~~c) (Alakohta c poistettu, tämänhän ratkoimme jo aiemmin: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data)~~

#### d) SQL injection vulnerability allowing login bypass

- Huvin ja urheilun vuoksi päätin kokeilla käyttää tuota mitmproxyä tässä, ja jotta siitä tulisi jotain kaivoin siihen jonkinlaisen [cheatsheetin](https://quickref.me/mitmproxy.html), ja [ohjeen](https://docs.mitmproxy.org/stable/mitmproxytutorial-modifyrequests/) pyyntöjen muokkaamiseen. Tehtävänannon mukaan pitäisi päästä kirjautumaan sisään adminina, niin jokseenkin järkevä paikka aloittaa on varmaankin tehdä kirjautumisyritys selaimella, ja pysäyttää se proxyyn. Mitmproxyssä **i**:tä (intercept) painamalla ja lisäämällä siihen ehto (laitoin tässä login siihen) voidaan liikennettä kaapata, ja sen jälkeen **e**:llä (edit) muokata pyyntöä.

    ![asdasd](https://i.imgur.com/YnFPYKJ.png)

- Tuosta siis valitaan muokattava kohta, joka siis tässä tapauksessa vissinkin oli tuo URLEncoded form, koska ainakin siellä näkyy administrator, jonka perään tämä injektio tullee. Se siis avautuu tekstieditorissa ja sitä voidaan muokata. Tässä vaiheessa meni vähän arvalla tämä ratkaisu, eli koska tuossa aiemmassa [Portswiggerin SQL-injektio materiaalissa](https://portswigger.net/web-security/sql-injection) oli esimerkkinä käytännössä sama skenaario, niin kokeillaanpa sitä. Siinä käyttäjänimen perään lisättiin **'--**, mikä siis SQL:ssä tarkoittaa kommentin alkua, joten tuo kyselyn loppuosa eli salasanan kysyminen jäisi siitä kokonaan pois.
- Muokatun pyynnön lähettäminen Mitmproxyssä onnistuu ensin tallentamalla se **esciä** painamalla, ja sen jälkeen **q q** takaisin flow-ikkunaan, ja **a** flow-ikkunassa, joka lähettää pyynnön. 

    ![asdasdsadas](https://i.imgur.com/EmmEIR9.png)

    ![asdasasdasddsa](https://i.imgur.com/mClc9Tw.png)
  
#### e) SQL injection attack, querying the database type and version on Oracle

- Ideana tässä oli käyttää UNION-hyökkäystä, jolla siis voidaan suorittaa haku tietokannan muihinkin tauluihin. Tehtävänantona oli saada Oracle tietokannan tyyppi ja versio selville tälläistä hyökkäystä käyttämällä, ja haavoittuvuus löytyy 'product category filtteristä', joten aluksi proxyllä tarkastelemaan sitä.
- Ensimmäinen ideani tietty on koittaa syöttää tuon haun perään tuolla UNION SELECT:llä tuosta [SQL Injection Cheatsheetistä](https://portswigger.net/web-security/sql-injection/cheat-sheet) löytyvä Oracle tietokannan versiohaku.

    ![asdasd](https://i.imgur.com/p07jJ9y.png)

- Eipä toiminut, vaan tarjoaa *HTTP/1.1 500 Internal Server Error*, eli jotain tuossa haussa on vialla. Aikani tätä pohdittuani päädyin kaivelemaan tuota dokumentaatiota, josta löytyi [linkki](https://portswigger.net/web-security/sql-injection/union-attacks) noihin UNION-hyökkäyksiin tarkemmin, jossa mainitaan että toimiakseen haussa täytyy olla oikea määrä sarakkeita, ja oikeat datatyypit, ja Oraclen tietokannoissa täytyy myös aina määrittää joku taulu, jota varten siellä on valmiiksi luotuna *dual*.
- Samasta dokumentista löytyy myös ohje miten tuo tarvittujen sarakkeiden määrä voidaan tarkistaa, eli lisäämällä hakuun *' ORDER BY 1,2,3 etc.--*, kunnes vastauksena tulee jotain muuta kuin OK:ta. Tässä tapauksessa *'ORDER BY 3--* tuotti *Internal Server Errorin*, joten kaksi lienee oikea lukumäärä.
- No tästä viisastuneena kokeilin tuota aiempaa hakua, mutta lisäten siihen toisen kentän (laitoin vaan 'asd' eli random merkkijonon), ja sepä toimikin.

    ![asda1sd](https://i.imgur.com/okAZXXT.png)

#### f) SQL injection attack, querying the database type and version on MySQL and Microsoft

- Tässä sama idea kuin edellisessäkin tehtävässä, mutta vaan eri tietokanta, eli jos vilkaistaan tuolta cheat-sheetistä, niin syntaksi MySQL & Microsoftiin olisi *@@version*, eli kokeillaan vaihtaa se tuohon aiemmin käytettyyn hakuun tuon *SELECT banner FROM v$version* tilalle, eli: `'UNION SELECT @@version, 'asd'--`.
- Ei toiminut, joten takaisin dokumentaatiota tutkimaan, ja cheatsheetistä löytyikin mainita, että MySQL:ssä jos käytettään tuota *--* kommentointiin, niin tarvitsee sen perään lisätä välilyönti, eli kokeillaan uudestaan sillä, ja se toimikin.

    ![asdasdasdsadasda213](https://i.imgur.com/se13Pj8.png)

#### g) SQL injection attack, listing the database contents on non-Oracle databases

- Taas koska varmaankin käytetään UNION hyökkäystä tarvitsee selvittää tuo kenttien määrä tuolla *ORDER BY --* jutulla, ja sehän oli taas kaksi kpl. Sitten pitäisi varmaankin ensinäkin keksiä miten saada nuo taulujen nimet haettua, ja googlella löytyi jonkinlainen [ohje](https://www.sqltutorial.org/sql-list-all-tables/), eli kokeillaan varmaankin *SELECT table_name FROM * all_tables*, paitsi että siihen tarvinnee lisätä myös toinen kenttä. Eli haulla `'UNION SELECT table_name, 'asd' FROM all_tables--` saadaan vastaukseksi pitkä lista tauluja, joista pitäisi seuraavaksi keksiä miten kaivaa tuon administator + salasana esiin.

    ![as123414](https://i.imgur.com/vPmBZkV.png)

- Tässä taas piti eka selvittää, että miten saada nuo haettua noi kaikki sarakkeet valitusta taulusta, niin tuli mieleen kysyä ChatGPT:ltä tätä ja vastaus tulikin kuin apteekin hyllyltä, eli `SELECT COLUMN_NAME FROM ALL_TAB_COLUMNS WHERE TABLE_NAME = 'your_table_name'`, ja tuohon kun lisätään eteen UNION, ja tuo toinen kenttä, sekä taulun nimeksi tuolla aiemmalla haulla löytynyt *USERS_OXKMFF* saadaan hakutuloksena lupaavat salasana -ja käyttäjäsarakkeet.

  `'UNION SELECT column_name, 'asd' FROM all_tab_columns WHERE  table_name = 'USERS_OXKMFF'--`

  ![asd124124314341](https://i.imgur.com/f2SBwrb.png)

- Eli nyt meillä kaiken järjen mukaan on kaikki tarvittavat tiedot, että voidaan suorittaa ns. normaali haku näitä käyttäen. 

  `'UNION SELECT USERNAME_UBAOAH, PASSWORD_SEJHCI FROM USERS_OXKMFF--`

  ![asdasdsadassadasdadsdasdsadas](https://i.imgur.com/zZfe8TH.png)

- Kokeillaan kirjautua admin-tunnareilla ja salasanalla, et voilà c'est magnifique.

  ![asdasdsadasdas124123141245325r234](https://i.imgur.com/gNMlQgE.png)

#### h) SQL injection UNION attack, determining the number of columns returned by the query

- Siis onkohan tämän tehtävän ideana nyt vaan selvittää tuo sarakkeiden määrä, mikä hoituu tuolla ORDER BY:llä ja oli näemmä tässä kolme. Ei vaan ainakaan muutu ratkaistuksi tuolla keinolla?
- Jaa-a, katoin sitten tuota annettua ratkaisua, niin siinä oli että lisätään NULL:ia niin kauan, että saadaan tuo sama asia selville, ja tämän rimpsun jos tuonne lähetti, niin muuttui tuo labra myös solved muotoon, mutta en nyt tiedä onko tähän mitään järkevää selitystä, koska nähdäkseni tuo ORDER BY ajaa saman asian.

  ![1ajjj](https://i.imgur.com/WMtnBa8.png)
  
#### i) SQL injection UNION attack, retrieving data from other tables

- Tämähän taas oli helpompi versio tuosta g-tehtävästä, kun tässä jo annettiin suoraan tietokannan ja sarakkeiden nimet, niin eipä tarvitse muuta kuin lisätä ne UNION kyselyyn, niin ja tarkastaa ORDER BY:llä sarakkeiden määrä.

  `'UNION SELECT username, password FROM users--`

  ![agdsbv](https://i.imgur.com/kmKHigq.png)

#### j) SQL injection UNION attack, retrieving multiple values in a single column

- Tosiaan tuolla UNION-hyökkäysten dokumentaatiossa on kohta, jonka mukaan yhdistämällä merkkijonoja voidaan hakea useampia arvoja kerralla, eli se varmaankin tässä on ideana.
- Ihan ensimmäiseksi taas katsotaan sarakkeiden määrä ORDER BY:llä, mikä oli taas kaksi. Seuraavaksi testiin, että josko tämä olisi niinkin helppo, että ratkeaisi suoraan tuolla dokumentaatiossa Oraclen tietokannoille annetulla rimpsulla (*' UNION SELECT username || '~' || password FROM users--*). Ei sentään ratkennut tuolla, mutta tuosta voidaan päätellä, että kyseessä ei ole Oraclen tietokanta, joten seuraavaksi testiin joku toinen keino.
- Cheatsheetissä listalla seuraavana olisi Microsoft, jossa yhdistäminen tapahtuu näemmä vain *+* -merkkiä käyttämällä, eli `'UNION SELECT username + password FROM users--`, vaan eipä toiminut tuokaan. Eli jäljelle jäisi MySQL, ja `'UNION SELECT CONCAT(username, password) FROM users--`, mikä ei myöskään toimi xD.
- Eli joko teen kokonaan jotain väärin, tai sitten ehkä todennäköisemmin jossain näissä on joku syntaksivirhe, mutta nyt kyllä cba alkaa niitä etsimään.


### k) Vapaaaehtoinen: Mitmproxy. Demonstroi mitmproxy:n käyttöä terminaalista (TUI tai CLI).

- ks. kohta d)
  
### l) Vapaaehtoinen: Attack lab. Asenna vapaavalintainen kone Vulnhubista ja murtaudu siihen.

### Lähteet:

- https://www.cybertec-postgresql.com/en/error-permission-denied-schema-public/
- https://terokarvinen.com/2016/03/05/postgresql-install-and-one-table-database-sql-crud-tutorial-for-ubuntu/
- https://terokarvinen.com/2019/05/22/mitmproxy-on-kali-and-xubuntu-attack-and-testing/?fromSearch=mitmproxy
- https://portswigger.net/web-security/sql-injection/cheat-sheet
- https://portswigger.net/web-security/sql-injection
- https://portswigger.net/web-security/sql-injection/union-attacks
- https://owasp.org/www-project-top-ten/2017/A1_2017-Injection
- https://docs.mitmproxy.org/stable/concepts-certificates/
- https://docs.mitmproxy.org/stable/mitmproxytutorial-modifyrequests/
- https://quickref.me/mitmproxy.html
- https://openai.com/chatgpt


