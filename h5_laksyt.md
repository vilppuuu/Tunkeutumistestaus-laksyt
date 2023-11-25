## h5 Injected Sequel
*Nyt olisi tarjolla OWASP 2017 numero uno, SQL injektio. Kannattaa aloittaa saman tien, niin ehdit ratkoa tehtäviä monena päivänä.*



### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**Karvinen 2016: PostgreSQL Install and One Table Database – SQL CRUD tutorial for Ubuntu (Virtuaaliympäristöä ei tarvita, voit aloittaa kohdasta "Three line install"**

- 

**OWASP 2017: A1:2017-Injection**

- 2017 injektiohyökkäykset olivat sijalla yksi tuolla OWASP:in top10-listalla, ja tässä taulussa esitellään ne pääpiirteissään.
- Ensimmäisenä mainitaan se tosiasia, että käytännössä aina kun voidaan syöttää dataa, siinä on myös mahdollisuus injektiolle. Näitä injektiohaavoittuvaisuuksia löytyy etenkin 'legacy-koodista', ja erilaisita tietokantahauista, sekä muista tiedonsyöttö/haku -metodeista.
- Seurauksten vakavuus injektiohyökkäyksissä vaihtelee, ja se on riippuvainen siitä kuinka kriittistä kohteena oleva data on.
- Yleisesti ottaen sovellus on haavoittuvainen injektiolle, jos siihen syötettyä dataa ei validoida, filtteröidä, tai 'siivota' mitenkään. Tai jos ohjelma salii erilaisten hakujen suorittamisen konteksista riippumatta.

**PortSwigger Academy: SQL injection**1
*Kaikki muut luvut paitsi ei "Blind SQL injection"
Tämä kappale kannattaa pitää näkyvissä injektioita tehdessä SQL injection cheat sheet*

- SQL-injektiossa hyökkääjä käyttää sovelluksen tietokantahakuja 'epärehdillä' tavalla päästäkseen käsiksi tietoihin, jotka normaaleissa olosuhteissa olisivat saavuttamattomissa (esim. salasanat, käyttäjätiedot etc.). Joissain tapauksissa SQL-injektio voi johtaa palvelimen tai muun infran vaarantumiseen, ja se voi myös mahdollistaa palvelunestohyökkäyksiä.
- Useimmat SQL-injektiot suoritetaan *SELECT*-kyselyn *WHERE*-osassa, jonka muokkaamisesta artikkelissa on muutama esimerkkikikka.
- 

**Vapaaehtoinen: Karvinen 2019: MitmProxy on Kali and Xubuntu – attack and testing (Nykyisin asennus 'sudo apt-get install mitmproxy')**

- Seurasin Teron [ohjetta](https://terokarvinen.com/2019/05/22/mitmproxy-on-kali-and-xubuntu-attack-and-testing/?fromSearch=mitmproxy), ja asennuksessa ei ollut mitään sen ihmeellisempää. Täytyy kattoa, jos kerkeää ton kanssa leikkimään enemmän vielä tässä myöhemmin. Sertin asentamiseen löytyi nopealla googlellla [ohje](https://docs.mitmproxy.org/stable/concepts-certificates/) mitm-proxyn dokumentaatiosta, eli ~/.mitmproxy -kansiosta löytyy tuo tarvittava ca.cert -tiedosto, joka voidaan viedä selaimeen.
- 

  ![mitm](https://i.imgur.com/WOEfHPv.png)

### a) CRUD. Tee uusi PostgreSQL-tietokanta ja demonstroi sillä create, read, update, delete (CRUD). Keksi taulujen ja kenttien nimet itse. Taulujen nimet monikossa, kenttien nimet yksikössä, molemmat englanniksi.

### b) SQLi me. Kuvaile yksinkertainen SQL-injektio, ja demonstroi se omaan tietokantaasi psql-komennolla. Selitä, mikä osa on käyttäjän syötettä ja mikä valmiina ohjelmassa. (Tässä harjoituksessa voit vain kertoa koodista, ei siis tarvitse välttämättä koodata sitä ohjelmaa, joka yhdistää käyttäjän syötteen SQL:n)

### PortSwigger Labs

#### c) (Alakohta c poistettu, tämänhän ratkoimme jo aiemmin: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data)

#### d) SQL injection vulnerability allowing login bypass

- Huvin ja urheilun vuoksi päätin kokeilla käyttää tuota mitmproxyä tässä, ja jotta siitä tulisi jotain kaivoin siihen jonkinlaisen [cheatsheetin](https://quickref.me/mitmproxy.html), ja [ohjeen](https://docs.mitmproxy.org/stable/mitmproxytutorial-modifyrequests/) pyyntöjen muokkaamiseen. Tehtävänannon mukaan pitäisi päästä kirjautumaan sisään adminina, niin jokseenkin järkevä paikka aloittaa on varmaankin tehdä kirjautumisyritys selaimella, ja pysäyttää se proxyyn. Mitmproxyssä i:tä (intercept) painamalla ja lisäämällä siihen ehto (laitoin tässä login siihen) voidaan liikennettä kaapata, ja sen jälkeen e:llä (edit) muokata pyyntöä.

    ![asdasd](https://i.imgur.com/YnFPYKJ.png)

- Tuosta siis valitaan muokattava kohta, joka siis tässä tapauksessa oli tuo URLEncoded form, jonka jälkeen se avautuu tekstieditorissa ja sitä voidaan muokata. Tässä vaiheessa meni vähän arvalla tämä ratkaisu, eli koska tuossa aiemmassa Portswiggerin SQL-injektio materiaalissa oli esimerkkinä käytännössä sama skenaario. Siinä käyttäjänimen perään lisättiin **'--**, mikä siis SQL:ssä tarkoittaa kommentin alkua, joten tuo kyselyn loppuosa eli salasanan kysyminen jäisi siitä kokonaan pois.
- Muokatun pyynnön lähettäminen Mitmproxyssä onnistuu ensin tallentamalla se **esciä** painamalla, ja sen jälkeen **q q** takaisin flow-ikkunaan, ja **a** flow-ikkunassa, joka lähettää pyynnön. 

    ![asdasdsadas](https://i.imgur.com/EmmEIR9.png)

    ![asdasasdasddsa](https://i.imgur.com/mClc9Tw.png)
  
#### e) SQL injection attack, querying the database type and version on Oracle

- Ideana tässä oli käyttää UNION-hyökkäystä, jolla siis voidaan suorittaa haku tietokannan muihinkin tauluihin. Tehtävänantona oli saada Oracle tietokannan tyyppi ja versio selville tälläistä hyökkäystä käyttämällä, ja haavoittuvuus löytyy 'product category filtteristä', joten aluksi proxyllä tarkastelemaan sitä.
- Ensimmäinen ideani tietty on koittaa syöttää tuon haun perään tuolla UNION SELECT:llä tuosta [SQL Injection Cheatsheetistä](https://portswigger.net/web-security/sql-injection/cheat-sheet) löytyvä Oracle tietokannan versiohaku.

    ![asdasd](https://i.imgur.com/p07jJ9y.png)

- Eipä toiminut, vaan tarjoaa *HTTP/1.1 500 Internal Server Error*, eli jotain tuossa haussa on vialla. Aikani tätä pohdittuani päädyin kaivelemaan tuota dokumentaatiota, josta löytyi [linkki](https://portswigger.net/web-security/sql-injection/union-attacks) noihin UNION-hyökkäyksiin tarkemmin, jossa mainitaan että toimiakseen haussa täytyy olla oikea määrä sarakkeita, ja oikeat datatyypit, ja Oraclen tietokannoissa täytyy myös aina määrittää joku taulu, jota varten siellä on valmiiksi luotuna *dual*.
- Samasta dokumentista löytyy myös ohje miten tuo tarvittujen sarakkeiden määrä voidaan tarkistaa, eli lisäämällä hakuun *' ORDER BY 1,2,3 etc.--*, kunnes vastauksena tulee jotain muuta kuin OK:ta. Tässä tapauksessa *'ORDER BY 3--* tuotti *Internal Server Errorin*, joten kaksi lienee oikea lukumäärä.
- No tästä viisastuneena kokeilin tuota aiempaa hakua, mutta lisäten siihen toisen kentän (laitoin vaan 'asd' eli random stringin), ja sepä toimikin.

    ![asdasd](https://i.imgur.com/okAZXXT.png)

#### f) SQL injection attack, querying the database type and version on MySQL and Microsoft

#### g) SQL injection attack, listing the database contents on non-Oracle databases

#### h) SQL injection UNION attack, determining the number of columns returned by the query

#### i) SQL injection UNION attack, retrieving data from other tables

#### j) SQL injection UNION attack, retrieving multiple values in a single column

### k) Vapaaaehtoinen: Mitmproxy. Demonstroi mitmproxy:n käyttöä terminaalista (TUI tai CLI).

### l) Vapaaehtoinen: Attack lab. Asenna vapaavalintainen kone Vulnhubista ja murtaudu siihen.
