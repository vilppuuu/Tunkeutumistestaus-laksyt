## h6 Attaaack
**Tämä läksy sisältää PhishSticks-tiimin (Halonen, Rajala ja Ollikainen) ehdotusten pohjalta tehtyjä läksyjä.*

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
Erityisesti kehikon omat määritelmät termeille tactics, techniques and procedures

- Mitre ATT&CK on kyberhyökkääjien toiminnan, käytäntöjen, ja käytöksen seurantaan ja analysointiin tarkoitettu viitekehys.
- Tässä viitekehyksessä hyökkääjien tavoitteet ja toiminta on jaoteltu kolmeen eri vaiheeseen: taktiikoihin, tekniikoihin ja proseduureihin.
- Taktiikka tässä yhteydessä hieman paradoksaalisesti tarkoittaa hyökkääjän tavoitetta laajassa mittakaavassa, eli minkä takia hyökkäys tehdään (esim. halutaan saada pääsy tiettyyn dataan).
- Tekniikka taas tässä yhteydessä tarkoittaa sitä miten tähän tavoitteeseen voidaan päästä (jos termit olisivat loogisia tämä vaihe olisi taktiikka xD). Mutta siis tekniikat ovat käytännössä hyökkäystapoja (esim. bruteforce, phishing etc.) ja valitusta tekniikasta voidaan eritellä vielä tarkempi tekniikka (sub-technique, esim. Spearphishing Attachment olisi alitekniikka phishingiin).
- Proseduurit taas

**MITRE Att&ck Enterprise Matrix**
Silmäile, poimi muutama esimerkki. Koko kehikko on laaja, eikä sitä tarvitse lukea tässä kokonaan.

### a) The OS pwns you. Asenna Windows virtuaalikoneeseen samaan verkkoon hyökkäyskoneen (esim. Kali, Debian) kanssa. Kokeile, että saat koneen irrotettua Internetistä.

### b) Trustme.lnk. Kokeile PhishSticksin revshell vihamielistä tiedostoa, joka avaa käänteisen shellin hyökkääjän koneelle. Selitä, mitä tapahtuu ja miksi. Testaa, että pysyt antamaan kohdekoneelle komentoja reverse shellin kautta.

### d) PageRank. Laita linkki raporttiisi kurssisivun kommentiksi.
Kannattaa mainita URL sekä leipätekstissä ("Comment") että kotisivun osoitteessa ("Homepage"), jotta sitä on helppo klikata.
Yksikin lause sisällöstä tai jostain kiinostavasta huomiosta lisännee klikkauksia

### c) Attaaack! MITRE Attack Enterprise Matrix: Demonstroi viisi tekniikkaa viidestä eri taktiikasta.
Tekniikkaa tulee kokeilla käytännössä, kuvailu ei riitä.
Asenna tarvittaessa omat harjoitusmaalit. Eristä tarvittaessa koneet Internetistä harjoittelun ajaksi.
Voit käyttää kurssilla jo opeteltuja työkaluja (helpompaa) tai kokeilla uusia. Aiemminkin käytetyistä tekniikoista pitää tehdä uusi demonstraatio tässä tehtävässä.
Mikäli haluat tehtävästä helpon version, tekniikoiden valinta auttaa. Tuolta löytyy myös tuttuja ja helppoja tekniikoita.
Selitä, mitä esimerkissä tapahtuu.
Nimeä käytetyt taktiikat, tekniikat ja alitekniikat. Merkitse myös numerot T0123.456.
d) Vapaaehtoinen: Total attak! Demonstroi yksi tekniikka jokaisesta MITRE Attack -taktiikasta. (Noista aiemmin tehdystä viidestä ei tarvitse tehdä uutta demonstraatiota.) MITRE Attack Enterprise Matrix
e) Vapaaehtoinen: Kokeile jokin toinen hyökkäystekniikka PhishSticks-projektista.
f) Vapaaehtoinen, vaikea: Tee oma RAT tai malware ja testaa sitä. Miten tunnistaisit oman haittaohjelmasi ja estäisit sen toiminnan? (Kotitehtävissä ei tehdä matoja tai muita itsestään leviäviä ohjelmia. Katso turvallisuusvinkit alta).
