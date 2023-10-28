# h1 Hacker Warmup

Tässä harjoituksessa on tiedustelua ja lämmittelyä. Pääset lukemaan sen kuuluisan paperin kybertappoketjusta. Tutustut O'Reilly Learning -alustan videokursseihin. Kokeilet montaa harjoitusympäristöä. Jos hakkerointi sujuu jo, näillä samoilla alustoilla voit harjoitella myös kurssin ulkopuolella.

### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

#### € Santos et al: The Art of Hacking (Video Collection): [..] 4.3 Surveying Essential Tools for Active Reconnaissance. Sisältää porttiskannauksen. 5 videota, yhteensä noin 20 min.
- Ensimmäisessä videossa selitetään mitä aktiivinen tiedustelu oikeastaan on, eli kuten nimestäkin voi arvata siinä suoritetaan aktiivisesti toimenpiteitä kohteeseen (esim. porttiskannausta). Tämä tarkoittaa sitä, että olemme myös havaittavissa, minkä takia aktiivista tiedustelua kannattaa tehdä asteittain.
- Aktiivisen tiedustelun ensimmäisessä vaiheessa on hyvä pyrkiä varmentamaan passiivisessa tiedusteluvaiheessa kerättyjen tietojen oikeellisuus (esim. mahdolliset avoimet portit).
- Kolmas video käy läpi porttiskannaukseen eri työkaluja: nmap (yleisin ja monikäyttöisin), masscan (nopea, laajoihin skannauksiin), udppprotoscanner (udp-porttejen skannaamiseen, nopea)
- Nmapin läpikäyntiä: skannaukselle annettavia yleisiä parametrejä mm. -sS (TCP-skannaus, lähettää vain SYN-paketin), -vv (verbose), -T4 (nopeampi skannaus), -A (OS & version detection). Oletuksena nmap skannaa 1000 yleisintä porttia.
- Verkkopalveluiden skannaamiseen esitellään EyeWitness-ohjelmaa, joka käy ip-osoitteiden pohjalta verkkosivuja läpi ja kerää tietoja niiden headereistä ja kuvakaappaukset itse sivuista, jotta voidaan valita otollinen kohde.
- Erilaisten haavoittuvuusskannereiden esittelyä, jaoteltuna verkko- tai web-haavoittuvuksia etsiviin ohjelmiin. Nmapia voi myös toimia kevyenä haavoittuvuusskannerissa, sillä se sisältää joukon skriptejä, joilla voidaan etsiä tiettyjä haavoittuvuuksia.
#### Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.

- Paperin lähtökohtana on, että perinteinen tapa suojautua ei ole enään riittävä, kun uhkana on Advanced Presistent Threat (APT) 'edistynyt pitkäkestoinen uhka'. Tällä käsitteellä tarkoiteaan toimijaa, jolla on osaamista, resursseja, ja halua toteuttaa pitkäkestoisia ja hyvin kohdennettuja hyökkäyksiä kohteisiin, jotka sisältävät erittäin arvokasta tietoa.
- Ratkaisuna tähän ehdotetaan 'intelligence-driven computer network defence' -strategiaa, joka sisältää analyysin hyökkääjien kyvykkyyksistä, päämääristä, toimintatavasta, ja heikkouksista. Tässä mallissa tunkeutumisyritykset eivät ole yksittäisiä tapauksia, vaan niitä tarkastellaan osana hyökkääjien kokoajan edistyvää jatkumoa.
- 
### a) Ratkaise Over The Wire: Bandit kolme ensimmäistä tasoa (0-2).

- Level 0 alkaa tosiaan ottamalla ssh-yhteys koneeseen, jossa 'peli' sijaitsee, eli yksinkertaisin lienee Linuxissa tai Windowsissa terminaalissa käyttöää ssh:ta. Eli komennolla *ssh bandit0@bandit.labs.overthewire.org -p 2220* päästään yhdistämään, eli tuossa
näkyy tuo bandit0, joka on käyttäjänimi ja kone, johon yhdistetään on @:n jälkeen oleva rimpsu, ja tosiaan -p määritetään portti, jota tässä oli ohjeistettu käyttämään oletusportin (22) sijasta. Tämä kun on laitettu terminaaliin, yhteys kysyy salasanaa, joka tässä oli turvallisesti määritelty samaksi kuin käyttäjänimi.

- Level 1 piti löytää salasana readme-nimisestä tiedostosta, joka löytyy koneen johon juuri yhdistimme home directorystä. Komennoilla pwd, ls, ja cat saamme selville hakemiston, siinä sijaitsevat tiedoston ja tiedoston sisällön, joka oli siis salasana seuraavaan leveliin kirjautumiseen.

- Level 2 siis otettiin ssh-yhteys, muuuten samalla tavalla kuin aiemmin, mutta käyttäjällä bandit1 ja edellisessä levelissä löydetyllä salasanalla. Tässä ideana on kaivaa salasana tiedostosta, joka on nimetty "-". Pelkästään 'cat -' ei toimi, koska se lukee - jonain STDIN(?):nä ja odottaa käyttäjältä lisää syötettä. Sen sijaan, jos laittaa tiedoston polun cat-komentoon, eli esim cat ./- se lukee sen normaalisti, ja salasanan saa esiin.

![lvl1](https://github.com/vilppuuu/tunkeutumistestaus/assets/103587907/18769ff3-8bfc-46a1-a631-3a96a783724d)

![lvl2](https://github.com/vilppuuu/tunkeutumistestaus/assets/103587907/83e2b05f-8cde-4493-897c-0001b07b6adb)

### b) Ratkaise Challenge.fi:stä yksi tehtävä, esim. Challenge.fi 2021 Where was this picture taken, Encoding basics. Tai joku Challenge.fi 2022. (Nimi vaihtui sattuneesta syystä, se on nykyisin Next Gen Hack Challenge)
-  Where was this picture taken? 2/4 (195)
-  Kohtuu helppo ratkaista googlen (reverse) image searchilla, eli image.google.com & search by image, ja drag & drop ladatun kuvan siihen. Antaa tulokseksi Blackpool Toweria, mikä tuntuu matchäävän hyvin.

### c) Ratkaise PortSwigger Labs: Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. (Edellyttää ilmaista rekisteröitymistä. Tehtävän voi ratkaista pelkästään selaimen osoitekenttää muokkaamalla.)

### d) Asenna Linux virtuaalikoneeseen. Suosittelen joko Kali (viimeisin versio) tai Debian 12-Bookworm.

![deebian12](https://github.com/vilppuuu/tunkeutumistestaus/assets/103587907/eb70155d-a047-4053-a9e9-8527ec014653)

### e) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (localhost). Analysoi tulokset.
- Oletuksenani oli, että ei varmaankaan pitäisi löytyä pahemmin avonaisia portteja, kun tuoreeltaan asennettu VM ja käyttis, ja eihän sieltä juuri mitään löytynytkään, paitsi tuo portti 631 ipp (Internet Printing Protocol), joka näemmä on oletuksena auki, jotta tulostuspalveluiden käyttö onnistuisi ilman säätöä.
![nmap1](https://github.com/vilppuuu/tunkeutumistestaus/assets/103587907/72bf3932-135b-4fc3-b1e9-2f952b1ae414)


### f) Porttiskannaa kaikki koneesi (localhost) tcp-portit. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin).
- Lisäämällä aiemmassa tehtävässä käytettyyn komentoon määrite -p 1-65535 saadaan skannattua koko tcp-porttiavarauus, mikä ei tässä tapauksessa tarjonnut mitään uutta informaatiota. Huvikseni tein skannasin myös udp-portit, joista löytyi portit 631 (ipp), 5353 (multicast DNS), ja 45681 (määrittelemätön portti, ei myöskään palvelun nimi ei tiedossa, vähän kysymysmerkki miksi auki, en tiedä olisiko jotain virtualboxin juttuja?)
  
### g) Tee laaja porttiskanaus (nmap -A) omalle koneellesi (localhost), kaikki portit. Selitä, mitä -A tekee. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin.).
- Tuo -A sallii agressiivisen skannauksen, eli porttiskannauksen tehdään erilaisia skanneja, joilla koitetaan määritellä esimerkiksi käyttöjärjestelmää, avoimissa porteissa olevia palveluita ja niiden versioita, ja lisäksi se ajaa joukon nmapin default-skriptejä, jotka voivat esim. skannata haavoittuvuuksia. Tässä tapauksessa tuo -A sai aiempiin skannauksiin lisätietona tuotettua käyttöjärjestelmän ja sen version, ja tcp/ip-stäkin sormenjäljen.
- 
### h) Asenna ja käynnistä jokin palvelin (apache, ssh...) koneellesi. Vertaile, miten porttiskannauksen tulos eroaa.
### i) Kokeile ja esittele jokin avointen lähteiden tiedusteluun sopiva weppisivu tai työkalu. Hyviä esimerkkejä löytyy Bazzel: IntelTechniques: Tools ja Bellingcat: Resources, voit myös käyttää muuta itse valitsemaasi työkalua. Työkalua pitää siis myös kokeilla, pelkkä nimen mainitseminen ei riitä. Pidä esimerkit harmittomina, älä julkaise kenenkään henkilökohtaisia salaisuuksia raportissasi.
j) Vapaaehtoinen: Tee lisää harjoituksia alustoilta, joihin tässä on tutustuttu: PortSwigger Academy, Over the Wire, Challenge.fi. Hakkeroimaan oppii hakkeroimalla.
