## h6 Attaaack

*Tämä läksy sisältää PhishSticks-tiimin (Halonen, Rajala ja Ollikainen) ehdotusten pohjalta tehtyjä läksyjä.*

- Tehtävät suoritettu käyttäen seuraavaa kokoonpanoa:
    - Hardware: AMD Ryzen 5 5600X, Nvidia GeForce RTX 3070, 32 GB RAM
    - Host OS: Windows 11 Home 22H2
    - Virtualization Software: Oracle VM VirtualBox Version 7.0.12 r159484 (Qt5.15.2)
    - VMs Used: Debian 12, Kali Linux 2023.3

### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**€ Yehoshua and Kosayev 2021: Antivirus Bypass Techniques, luku:
Chapter 1: Introduction to the Security Landscape**

- Alussa perus liibalaabaa siitä minkä takia kyberrikollisuutta ylipäätään on, ja syy tähän tietysti on raha, eli koska paljon arvokasta tietoa käsitellään digitaalisesti luo se rikollisille taloudelliset motiivit koittaa päästä hyödyntämään tätä esim. varastamalla maksutietoja, käyttäjätietoja, dataa jne.
- Malware määritellään yleistermiksi (portmanteau=matkalaukku, fancy sana ranskaa :D) haitallisille ohjelmille, joiden tehtävänä on mm. saada pääsy kohteeseen, varastaa tietoa, salata tiedostoja, käyttää kohdetta osana bottiverkkoa etc.
- Antivirus-ohjelma on yleisin/simppelein tapa suojautua uhilta, mutta nykyään AV:n lisäksi ja tueksi on käytössä muitakin eri tuotteita (IDS/IPS, NAC, Tulimuurit, DLP=DataLossPrevention, EDR=EndpointDetection&Response), jotka toimivat hieman eri tavoilla ja eri osissa järjestelmää.
- Antivirus-ohjelmia on erilaisia tapoja havaita haitallisia tiedostoja, ja tässä ne on jaoteltu neljään yleisimpään kategoriaan toimintatavan perusteella (staattinen, dynaaminen, 'heuristic', 'unpacking').
- Yksinkertaistettuna staattinen skannaus vertaa tiedoston 'signaturea' tietokantaan, jossa on haittaohjelmia kirjattuna (suhteellisen hyödytön tapa, kun muutos tiedostoon muuttaa signaturea, ja saattaa ohittaa tämän tarkastuksen kokonaan).
- Dynaaminen skannaus taas käyttää API-kutsujen tarkastelua ja sandboxausta tunnistaakseen ajaako tiedosto haitallista koodia.
- Heuristinen skannaus on edistyneempi tapa, ja se käyttää aiemmin mainittuja metodeja ja tilastollista analyysiä havaitakseen haitallisia ohjelmia.
- Yleinen tapa piiloutua AV:lta on pakata haittaohjelma jollain tapaa, jolloin AV:n täytyy ensiksi osata purkaa (unpack) se. Tähän sisältyy sellainen ongelma, että saadakseen automatisoitua purkamisen AV:n täytyy tietää pakkaustapa, mikä tarkoittaa sitä että kaikki pakkaukset on ensiksi purettava käsin.
- Lopussa todetaan, että AV ei ole mikäään täydellinen suoja, vaan sen ohittaminen on hyvinkin mahdollista, mutta modernissa ympäristössä se ei välttämättä myöskään takaa hyökkääjälle automaattista voittoa.

**Halonen, Rajala ja Ollikainen 2023: PhishSticks Youtube Channel, kahdeksan videota, yhteensä noin 15 min**

**Halonen, Rajala ja Ollikainen 2023: PhishSticks Git Repository, sivut:**

**README.md (Aikajärjestyksessä, jos aloitat pohjalta. Kuvailee, miten näitä tekniikoita opeteltiin ja kehitettiin)**

- Kuvailee PhistSticks projektin tavoitteita ja edistymistä syksyn aloituspäivästä tähän päivään asti.
- Alussa määritellään tavoiteeksi USB-hyökkäystikun luominen, joka sisältäisi keyloggerin, ransomwaren, sekä reverse shellin.
- Kehitystyön edistymisen kuvausta vaihe vaiheelta, alkaen tikkuun tarvittavan hardwaren hankinnasta ja jatkuen siitä eri payloadien kehittämiseen ja testaamiseen, ja loppuen toimivan prototyypin valmistumiseen.
  
**Revshell**

- Kun tikulla sijaitsevaa 'raport' -fileä (ei ole oikeasti txt-file, Windowsia vaan pystyy jekuttamaan näyttämään väärän kuvakkkeen) klikataan avaa se oikeasti vsb-skriptin, joka lataa Netcutin ja laittaa sen toimimaan piilotetussa PowerShell -ikkunassa, sekä ottamaan yhteyden määriteltyyn ip-osoitteeseen ja porttiin.

**Mitigations**

- Sisältää valikoiman keinoja, joilla kyseinen hyökkäys voitaisiin estää. Näihin kuuluu mm. powershellin kieletäminen käyttäjältä kokonaan, powershellin blokkaaminen muurilta, win runin kieltäminen käyttäjältä, ja 'Controlled folder accessin' päälle laittaminen Windows Securitystä (estää tiedostojen salakirjoituksen).
- Myös jos palomuurisäännöt olisivat tiukemmat, eli oletuksena kaikki paitsi etukäteen määritellyt portit kielletty niin sisään kuin ulospäin ei reverse shell saisi yhteyttä. Mutta joo oletuksena, että kaikki liikenne ulospäin on sallittu on yleinen asetus, ja etenkin tilanteessa mitä simuloidaan, eli yrityksessä missä nollat taulussa tökitään random USB-tikkuja kiinni koneeseen tuskin on kovinkaan tarkasti mietitty palomuurisääntöjä. Muistaakseni myös keyloggerin olisi ollut mahdollista lähettää tietoja sähköpostilla, mikä tälläisessa tapauksessa olisi kuitenkin toiminut.

**Installing Windows 10 on a virtual machine**

- Muuten perus Windowsin asennusohje paitsi, että sisältää linkin tuohon tarvittavaan .iso-filuun, josta voidaan asentaa VM:lle, ja näemmä painamalla tuota Domain joinia ei tarvitse kirjautua Microsoft-tilillä.
- Kone myös liitetään VirtualBoxissa luotuuun host-verkkoon, jolloin se on irti interwebsistä ja sitä voidaan käyttää testaukseen yhdessä esim. aiemmin asennetun Kalin kanssa.

**MITRE Att&ck Frequently Asked Questions: Part 1. General.**
*Erityisesti kehikon omat määritelmät termeille tactics, techniques and procedures*

- Mitre ATT&CK on kyberhyökkääjien toiminnan, käytäntöjen, ja käytöksen seurantaan ja analysointiin tarkoitettu viitekehys.
- Tässä viitekehyksessä hyökkääjien tavoitteet ja toiminta on jaoteltu kolmeen eri vaiheeseen: taktiikoihin, tekniikoihin ja proseduureihin.
- Taktiikka tässä yhteydessä hieman paradoksaalisesti tarkoittaa hyökkääjän tavoitetta laajassa mittakaavassa, eli minkä takia hyökkäys tehdään (esim. halutaan saada pääsy tiettyyn dataan).
- Tekniikka taas tässä yhteydessä tarkoittaa sitä miten tähän tavoitteeseen voidaan päästä (jos termit olisivat loogisia tämä vaihe olisi taktiikka xD). Mutta siis tekniikat ovat käytännössä hyökkäystapoja (esim. bruteforce, phishing etc.) ja valitusta tekniikasta voidaan eritellä vielä tarkempi tekniikka (sub-technique, esim. Spearphishing Attachment olisi alitekniikka phishingiin).
- Proseduurit taas ovat tekniikoiden ja alitekniikoiden käytännöllisiä/teknisiä toteutustapoja, joita tiettyjen hyökkääjien on dokumentoitu käyttäneen. Lisäksi yksi tekniikka tai alitekniikka voi käyttää useampaa proseduuria kerralla.

**MITRE Att&ck Enterprise Matrix**
*Silmäile, poimi muutama esimerkki. Koko kehikko on laaja, eikä sitä tarvitse lukea tässä kokonaan.*

- Tuossa matriisin ylätasolla esitellyt taktiikat etenevät pitkälti samaan tapaan kuin kybertappoketju, eli aloitetaan tiedustelusta ja päätepisteenä on toimenpiteet kohteessa. Väliin vaan on mahdutetu huomattavasti yksityiskohtaisemmin tavaraa, liittyen etenkin kohteessa suoritettaviin toimenpiteisiin initial accessin jälkeen.
- Mielestäni juuri näiden taktiikoiden ja tekniikoiden suuri määrä kiinnittää huomion tässä Mitre ATT&CK:n matriisissa, eli pelkän haitallisen koodin ajamisen, ja C&C:n lisäksi hyökkääjällä on iso työkalupakki käytettävissään. Eli mm. käyttöoikeuksien laajennus, piiloutuminen puolustajilta, lisäkohteiden tunnistaminen, sivuttaisliike, tietojen kerääminen, sekä näihin liittyvät tekniikat ja alitekniikat sisältävät valtavan määrän erilaisia uhkia (tai mahdollisuuksia).

### a) The OS pwns you. Asenna Windows virtuaalikoneeseen samaan verkkoon hyökkäyskoneen (esim. Kali, Debian) kanssa. Kokeile, että saat koneen irrotettua Internetistä.

- Eli tuota yltä löytyvää sawulohen [ohjetta](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md) seuraten ensin tuo Win10-image lataukseen, ja seuraavaksi ohjeen mukaan VirtualBoxiin kliksuttelemaan asennusta.
- Asennuksen suorittaminen loppuun sujui ongelmitta, ja sammuttamisen ja host-adapterin määrittelyn jälkeen kone sujahti myös oikeaan verkkoon, josta ei yhteyttä internettiin.

    ![cmdd](https://i.imgur.com/B10zvhv.png)

### b) Trustme.lnk. Kokeile PhishSticksin revshell vihamielistä tiedostoa, joka avaa käänteisen shellin hyökkääjän koneelle. Selitä, mitä tapahtuu ja miksi. Testaa, että pysyt antamaan kohdekoneelle komentoja reverse shellin kautta.

- Varmaankin ihan alkuun on hyvä avata hyökkäyskoneelle tuo revshellin tarvitsema portti 9001.

    ![ufwf](https://i.imgur.com/fMNUAt4.png)

- Seuraavaksi tuo revshell-skripti tuonne kohdekoneelle, ja sinne määritetään oikea ip-osoite ja portti, johon reverse shelli sitten ottaa yhteyttä. Edge alkoi jo heti epäilemään, että kannattaako tälläisiä latailla koneelle :).

    ![edge](https://i.imgur.com/xYwLQL5.png)

    ![skript](https://i.imgur.com/x67jgC1.png)

- Seuraavana tuonne hyökkäyskoneelle laittelin nuo tarvittavat setit, eli latasin tuon Netcutin exen, ja loin sinne tuon raport.txt -tiedoston, ja loin niille oman kansion. Seuraava steppi herätti hieman ihmetystä itsessäni, koska tuossa revshell-skriptissä ei määritetä mitään polkua, ja itsellä ei ollut käsitystä miten tuo python3 http-server sen myöskään katsoo. [Demovideossa](https://www.youtube.com/watch?v=ll4ojo6q-rM) tuota komentoa ajetaan samassa kansiossa, jossa payloadit sijaitsevat, ja se käy myöskin järkeen joten tehdään niin.
- Eli tuolla luomassani phish-kansiossa komennolla `python3 -m http.server 80` saadaan web-palvelin päälle, josta tuo revshell-skripti saa ladattua setit. Tämän jälkeen voidaan myös netcut laittaa päälle ja kuuntelemaan tuota määriteltyä porttia komennolla: `nc -lvnp 9001`.
- Nyt kun ajetaan tuo revshell-skripti tuolta kohdekoneelta pitäisi sen ensinäkin ladata nc64.exe ja raport.txt, sekä käynnistää Netcut piilotetussa Powershell-ikkunassa, ja ottaa yhteys hyökkäyskoneelle, sekä tuon raport.txt -tiedoston pitäisi aueta käyttäjälle.
- Ja homma toimii, eli revshell-yhteys on auki ja sieltä saa komentoja ajettua, ja käyttäjälle aukesi näkyviin vain tuo tyhjä raport.txt -tiedosto.

![asdasasd](https://i.imgur.com/A3m7bLf.png)

![214asfa](https://i.imgur.com/aqKCQE9.png)


### d) PageRank. Laita linkki raporttiisi kurssisivun kommentiksi.
*Kannattaa mainita URL sekä leipätekstissä ("Comment") että kotisivun osoitteessa ("Homepage"), jotta sitä on helppo klikata.
Yksikin lause sisällöstä tai jostain kiinostavasta huomiosta lisännee klikkauksia*

### c) Attaaack! MITRE Attack Enterprise Matrix: Demonstroi viisi tekniikkaa viidestä eri taktiikasta.
*Tekniikkaa tulee kokeilla käytännössä, kuvailu ei riitä.
Asenna tarvittaessa omat harjoitusmaalit. Eristä tarvittaessa koneet Internetistä harjoittelun ajaksi.
Voit käyttää kurssilla jo opeteltuja työkaluja (helpompaa) tai kokeilla uusia. Aiemminkin käytetyistä tekniikoista pitää tehdä uusi demonstraatio tässä tehtävässä.
Mikäli haluat tehtävästä helpon version, tekniikoiden valinta auttaa. Tuolta löytyy myös tuttuja ja helppoja tekniikoita.
Selitä, mitä esimerkissä tapahtuu.
Nimeä käytetyt taktiikat, tekniikat ja alitekniikat. Merkitse myös numerot T0123.456.*

#### Initial Access & Exploit Public-Facing Application (T1190)

- Valitaan tässä ensimmäiseksi joku helppo, eli kun aiemmalla viikolla asennettiin tuota Metasploitable, niin sieltä voisi kokeilla jotain silloin havaittua, mutta silloin testaamatta jäänyttä haavoittuvuutta. Laitetaan alkuun tuoa Metasploitable päälle, ja suoritetaan porttiskannaus nmapilla (porttiskannauskin menisi kyllä tässä tuonne Discoveryn alle taktiikoissa, ja tekniikoissa varmaankin Network Service Discoveryn (T1046) alle, mutta ei nyt tässä lasketa niitä).
- Se mihin olin silloin aiemmin kiinnittänyt huomion olivat nuo porteista 512, 513, 514 löytyvät palvelut, eli valitsin niistä tuon lupaavimmalta kuulostavan eli *514 shell Netkit rshd:n*. Googlettamalla tämän löytyi [artikkeli](https://www.hackercoolmagazine.com/hacking-rexec-and-rlogin-services-on-ports-512-513-and-514/), jossa etenkin seuraava tieto käytännössä avasi meille oven.

*Rsh or Remote shell is a remote access service that allows users a shell on the target system. Authentication is not required for this service. By default it runs on port 514. Although Rsh doesn’t require a password, it requires the username belonging to the remote system.*

- Eli tarvitaan ainostaan tuo rsh-client, jonka saa aptista asennettua (*apt install rsh-client*), ja validi käyttäjänimi (kokeillaanpa rootilla), ja voidaan kokeilla yhdistää ja sehän toimii.

    ![as1234235r](https://i.imgur.com/9fzFE4T.png)

- Jos tätä hyökkäystä tarkastellaan tuon Mittre ATT&CK:n kehyksen kautta, niin tavoitehan tässä on päästä järjestelmään sisään, eli taktiikkana tässä on Initial Access, ja käytetty tekniikka hyödynsi ulospäin näkyvää palvelua, eli Exploit Public-Facing Application (T1190), mielestäni tähän nyt ei tarkempaa ali-tekniikkaa tai proseduuria löytynyt. Tietty myös tekniikkana oli ainakin tietyllä tapaa tuo Valid Accounts, kun tarvittiin käyttäjätunnus, mutta se nyt oli tässä tapauksessa enemmänkin sivujuonne.

#### Credential Access, Brute Force & Password Cracking (T1110.002)

- No seuraavaksi ajattelin, että koska otin tuolta Metasploitablesta talteen tuon etc/shadow:n, eli käyttäjät ja salasana-hashit, niin niiden kanssa voisi kokeilla tehdä jotain. Periaatteessa jo tuon salasanojen talteen ottamisen voisi myös määritellä tuon Mitre ATT&CK -kehyksen mukaan (Taktiikka -> Exfiltration, Tekniikka -> Exfiltration Over C2 Channel, Procedure -> ei löytynyt copy/pastelle omaa numeroa :p).
- Tuohon matriisin tämä salasanojen hashien murtaminen asettuu tuonne Credential Accessiin, ja siellä varmaankin lähimpänä on Brute Force, ja sieltä löytyisi Password Cracking (T1110.002).
- Mutta siis joskus aiemmin, kun vähän tutustuin tuohon Kaliin, niin selvisi että sieltä löytyy työkalu nimeltä hashcat, jolla siis voidaan noita salasana-hashejä murtaa. Tämän käyttöä siis aloin tutkia, ja nopeasti löytyikin ihan hyvännäköinen [ohje](https://www.freecodecamp.org/news/hacking-with-hashcat-a-practical-guide/), eli käytännössä Hashcatilla voidaan sanalistoja käyttäen koittaa löytää vastine hallusamme olevalle hashille, ja siten murrettua salasana.
- Toisinsanoen helppoa ja hauskaa, eli sen suurempia miettimättä latasin komennon `hashcat -m 0  -a 0 -o salaiset.txt etcshadow /usr/share/seclists/Passwords/500-worst-passwords.txt
` tulille. Tuossa komennossa sis *-m 0* merkkaa käytettyä salausalgoritmiä (MD5), ja *-a 0* hyökkäyksen tyyppiä (sanakirja), *-o* output-tiedostoa, ja seuraavana hashit sisältävä tiedosto, sekä käytetty sanakirja. Lopputulos oli se, että mitään ei saatu tehtyä, koska tokenit ovat väärän pituisia, eli oletettavasti yritetään murtaa väärällä algoritmillä.

  ![2345sdgsdfXDD](https://i.imgur.com/TzffE45.png)

- Seuraava kysymys olikin, että miten sitten voi tietää mitä algoritmiä käyttäen salaus on tehty, ja tähän vastaus vissinkin oli, että ei välttämättä mitenkään jos hallussa on pelkkä hash. Pelkkä trial & errorkaan ei välttämättä huvita, kun tarkastelee tuota Hashcatin sivuilta löytyvää [listaa](https://hashcat.net/wiki/doku.php?id=example_hashes). Tietty realistisessa skenaariossa tätä voisi varmaan automatisoida, ja testata sellaisessa järjestyksessä että käy ensiksi yleisimmin käytetyt algoritmit läpi.
- Mutta koska hallussamme on etc/shadow -filu löytyy siitä käytetty [algoritmi](https://www.cyberciti.biz/faq/understanding-etcshadow-file/) (`root:$1$/avpfBJ1$x0z8w5UF9Iv./DR9E9Lid`) ennen häshin alkua,  eli tässä tapauksessa *$1€*.
- Tätä kun verrataan tuohon Hashcatin dokumentaatioon löytyy se sieltä numerolla 500 (Unix MD5 nimellä), eli jos tuohon aiemmin ajettuun komentoon vaihdetaan *-m 500* pitäisi sen ainakin yrittää testata jotain tuota sanalistaa vastaan.
- Tosiaan näin kävi, eli tuosta tiedostosta löytyi seitsemän tätä algoritmiä käyttävää hashiä, joista näemmä yksi onnistuttiin myös murtamaan käyttäen tuota 500-worst-passwords -listaa.

  ![vsvsbsb](https://i.imgur.com/x6QRu1a.png)

  ![batman](https://i.imgur.com/KIp9TrI.png)

#### Defence Evasion & Template Injection

- Seuraavaksi voisi kokeilla tehdä aiempana viikkona tekemättä jääneen *Portswigger Lab: Server-side template injection with information disclosure via user-supplied objects*, joka siis on Template Injection ja sopii tuossa kehyksessä Defence Evasionin ja Template Injectionin (T1221) alle. Itseasiassa tämä tehtävä on ehkä vähän huono esimerkki näistä, koska nuo esimerkkiproseduurit tuolla keskittyvät enemmänkin esim. Office-templateihin, joita käytetään haitallisen koodin ajamiseen käyttäjällä. Toisaalta luulisi, että tässäkin olisi teoriassa sellaiseen mahdollisuus, vaikkei tämä tehtävä sellaista kuvaakaan.
- Sen verran muistin tästä, että ratkaisu liittyi jotenkin siihen, että käytössä oli Django-template, ja sen dokumentaatiosta löytyi jollain tapaa vastaukset. Koitin tutkia [dokumentaatiota](https://docs.djangoproject.com/en/4.2/ref/templates/builtins/), mutta ei kyllä senkään perusteella vielä oikein auennut, joten jouduin kurkkaamaan tuota ratkaisua tuolta labrasivulta.
- Tosiaan siellä maininta, että syöttämällä Djangon sisäänrakennettu *{% debug %}* -komento templateen edit-kohdassa saadaan näkyviin objektilista, johon meillä on pääsy. Tuolta olisi vielä pitänyt keksiä, että koska päästään käsiksi settings-objektiin sieltä voidaan pyytää secret key komennolla *{{settings.SECRET_KEY}}*. Syöttämällä tuon komennon templateen editissä sai tuon labran tosiaan ratkaistua, mutta joo ehkä turhan vaikea esimerkki tästä tekniikasta, vaikka itsessään se ei varsinaisesti ole mitään rakettitiedettä, mutta tässä tapauksessa kun kyseessä täysin uusi asia, niin vähän vaikeaa tajuta miten lähteä ratkaisemaan.

    ![asdasd](https://i.imgur.com/ak6QL0Q.png)

#### Discovery & File and Directory Discovery (T1083)

- Tässä päätin vähän palauttaa mieleen tuota fussaamista, eli nähdäkseni se kuuluu tuonne Discoveryn alle, ja tekniikoista File and Directory Discoveryyn, koska kansioitahan tässä nimenomaan etsiään web-palvelimelta. Proseduureja tähän en kyllä tuolta nopeasti löydä, kun nuo näkyvät keskittyvän webin sijasta enemmän hostin tiedostojen listaamiseen. Liekö niin, että tämä sopisi paremmin jonkun noista network-tekniikoista alle, mutta sielläkään ei sellaista 1:1 sopivaa kuvausta löydy.
- Anywho ajetaan nyt tuota FFUF:ia tuota Metasploitablea vastaan käyttäen sanalistana directory-list-2.3-mediumia `ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://192.168.99.4:80/FUZZ -v`. 

  ![asdasdad](https://i.imgur.com/NXJuEnD.png)



### Lähteet:

- https://attack.mitre.org/
- https://www.cyberciti.biz/faq/understanding-etcshadow-file/
- https://cs.stackexchange.com/questions/68070/given-a-string-is-it-possible-to-determine-which-hashing-algorithm-has-produced
- https://docs.djangoproject.com/en/4.2/ref/templates/builtins/
- https://github.com/therealhalonen/PhishSticks/tree/master
- https://hashcat.net/wiki/doku.php?id=example_hashes
- https://www.freecodecamp.org/news/hacking-with-hashcat-a-practical-guide/
- https://terokarvinen.com/2023/eettinen-hakkerointi-2023/

