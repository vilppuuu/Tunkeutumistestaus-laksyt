## h5 Injected Sequel
*Nyt olisi tarjolla OWASP 2017 numero uno, SQL injektio. Kannattaa aloittaa saman tien, niin ehdit ratkoa tehtäviä monena päivänä.*



### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**Karvinen 2016: PostgreSQL Install and One Table Database – SQL CRUD tutorial for Ubuntu (Virtuaaliympäristöä ei tarvita, voit aloittaa kohdasta "Three line install"**

- 

**OWASP 2017: A1:2017-Injection**

- 2017 injektiohyökkäykset olivat sijalla yksi tuolla OWASP:in top10-listalla, ja tässä taulussa esitellään ne pääpiirteissään.
- Ensimmäisenä mainitaan se tosiasia, että käytännössä aina kun voidaan syöttää dataa, siinä on myös mahdollisuus injektiolle. Näitä injektiohaavoittuvaisuuksia löytyy etenkin 'legacy-koodista', ja myös erilaisita tietokantahauista, sekä muista tiedonsyöttö/haku -metodeista.
- Seurauksten vakavuus injektiohyökkäyksissä vaihtelee, ja se on riippuvainen siitä kuinka kriittistä kohteena oleva data on.
- Yleisesti ottaen sovellus on haavoittuvainen injektiolle, jos siihen syötettyä dataa ei validoida, filtteröidä, tai 'siivota' mitenkään. Tai jos ohjelma salii erilaisten hakujen suorittamisen konteksista riippumatta.

**PortSwigger Academy: SQL injection**1
*Kaikki muut luvut paitsi ei "Blind SQL injection"
Tämä kappale kannattaa pitää näkyvissä injektioita tehdessä SQL injection cheat sheet*

**Vapaaehtoinen: Karvinen 2019: MitmProxy on Kali and Xubuntu – attack and testing (Nykyisin asennus 'sudo apt-get install mitmproxy')**

### a) CRUD. Tee uusi PostgreSQL-tietokanta ja demonstroi sillä create, read, update, delete (CRUD). Keksi taulujen ja kenttien nimet itse. Taulujen nimet monikossa, kenttien nimet yksikössä, molemmat englanniksi.

### b) SQLi me. Kuvaile yksinkertainen SQL-injektio, ja demonstroi se omaan tietokantaasi psql-komennolla. Selitä, mikä osa on käyttäjän syötettä ja mikä valmiina ohjelmassa. (Tässä harjoituksessa voit vain kertoa koodista, ei siis tarvitse välttämättä koodata sitä ohjelmaa, joka yhdistää käyttäjän syötteen SQL:n)

### PortSwigger Labs

#### c) (Alakohta c poistettu, tämänhän ratkoimme jo aiemmin: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data)

#### d) SQL injection vulnerability allowing login bypass

#### e) SQL injection attack, querying the database type and version on Oracle

#### f) SQL injection attack, querying the database type and version on MySQL and Microsoft

#### g) SQL injection attack, listing the database contents on non-Oracle databases

#### h) SQL injection UNION attack, determining the number of columns returned by the query

#### i) SQL injection UNION attack, retrieving data from other tables

#### j) SQL injection UNION attack, retrieving multiple values in a single column

### k) Vapaaaehtoinen: Mitmproxy. Demonstroi mitmproxy:n käyttöä terminaalista (TUI tai CLI).

### l) Vapaaehtoinen: Attack lab. Asenna vapaavalintainen kone Vulnhubista ja murtaudu siihen.
