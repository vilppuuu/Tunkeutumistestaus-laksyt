### H2 Läksyt - Sniff-n-Scan

*Edit: Sunnuntaiaamuna (5.11.2023) klo. 10:25 täytetty c)-kohtaan 'alakohdat' 3-7*

Sniff-n-scan tutustuttaa uuteen lähteeseen, hakkeritapahtumien nauhoihin. Opit valvomaan hyökkäystyökalujen toimintaa snifferillä, ja tutustut Wiresharkiin. Wireshark osaa myös analysoida paketit automaattisesti. Weppiin murtautumista auttaa suomalainen, fuzzereiden huipulle noussut ffuf. Harjoitusmaalien asentamisesta kokeillaan paikallisia binäärejä (by yours truly) ja Dockeria.

#### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.
   
**Hoikkala "joohoi" 2020: [Still Fuzzing Faster (U fool)](https://www.youtube.com/watch?v=mbmsT3AhwWU). In HelSec Virtual meetup #1. (about 1 hour)**
	- Alussa nopea info siitä mihin fuff:ia oikeastaan käytetään, eli mitä syötteitä annetaan (salasanat, käyttäjät, yleiset tiedostopolut ja parametrit etc.), ja millä tapaa näitä käytetään (GET parameterit, headerit, POST data).
 	- Eri filtteröionti tapojen läpikäyntiä esim. tiettyjen http-koodien avulla, false positive -tuloksien suodattaminen vastauksen koon, sanamäärän pohjalta.
  	- Loppu esitys oli oikeastaan demo fuffin käytöstä, jossa käytiin läpi sen eri ominaisuuksia.
   
**Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide:**

**[Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) (opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)**
- Kun TCP-portteja skannataan yleisimmin käytetään SYN-skannausta, ja riippuen vastauksesta skanniin portit jaotelaan kuuteen eri tilaan: *open, closed, filtered, unfiltered, open|filtered, or closed|filtered*.
- Portin ollessa open skannaukseen saadaan ACK-vastaus, mikä tarkoittaa että portissa on palvelu, johon voitaisiin yhdistää. Portin ollessa closed SYN-sanomaan tulee RST-vastaus, eli portti kyllä vastaa mutta siellä ei ole palvelua. Filtered tilassa taas sanomaan ei tule vastausta, koska palomuuri (tai joku muu konfiguraatio, IPS etc.) estää pääsyn portille.
  
**[Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html) (opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)**
- sS, (TCP SYN scan) Tässä skannauksessa SYN-paketti lähetetään samoin kuin oltaisiin avaamassa yhteyttä tavallisesti, ja jos siihen tulee ACK-vastaus portti on avoinna, jos taas RST portin voidaan olettaa olevan kiinni, ja jos vastausta ei kuulu lainkaan portti merkataan filtered-tilaan.
- sT, (TCP connect) käyttää käyttöjärjestelmän connect system callia muodostaakseen yhteyden. Käytetään yleensä vain, jos SYN-skannaus ei ole mahdollinen, sillä sT on tehottomampi, ja siitä jää enemmän jälkiä sekä se on helpompi havaita.
- sU eli UDP-skannaus voidaan suorittaa samanaikaisesti TCP-skannauksen kanssa. On ongelmallinen luotettavuuden suhteen, koska UDP on yhteydetön protokolla, joten porttien vastauksia on vaikeampi kategorisoida.
  			
#### a) Fuff. Ratkaise [Teron ffuf-haastebinääri](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge). Artikkelista [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) voi olla apua.
	
 - Harjoituksen ideana on löytää piilotettuja hakemistoja web-palvelimelta, jotta tätä olisi mitenkään järkevä toteuttaa tarvitaan siihen työkalu, jolla prosessi voidaan automatisoida. Tällä kurssilla käytetty ja suositeltu työkalu on fuff, jota voidaan hakemistojen etsimisen lisäksi käyttää myös esim. virtuaalihostien etsimiseen, käyttäjien ja salasanojen syöttämiseen ja muuhun.
- Aloitin harjoituksen lataamalla harjoitusmaalin ja fuff:in ohjeen mukaisesti, paitsi fuff:ista löytyi githubista uudempi versio (2.1), kun kävin siellä kurkkaamassa (https://github.com/ffuf/ffuf/releases/tag/v2.1.0), jonka siis latasin.
  
	![fuff](https://i.imgur.com/Zj81xv9.png) 
		
	![test](https://i.imgur.com/NkYl0Kx.png)
  
- Olin jo ladannut tuon Githubista SecList.zipin (https://github.com/danielmiessler/SecLists) katsottuani tuon Still Fuzzing Faster (U fool) -esityksen, kun se siellä mainittiin ja linkki löytyi esityksen lopusta. Eli täytyi vain tuon common.txt etsiä sieltä laittaa sen polku tuohon komentoon.
-  Komennossa siis -w määrittelee käytettävän sanalistan, ja -u urlin, jota sanalistalla pommitellaan.-
  
 	![fuzz](https://i.imgur.com/Tq4tv0f.png)

-  Tämä tulos ei varsinaisesti ole kovin hyödyllinen, koska se on vajaa 5000 riviä pitkä, joten tarvitaan keino filtteröidä sitä. Ffuf:issa tuloksia voidaan filteröidä ainakin HTTP-statuksen (-fc), koon (-fs), sanojen (-fw), rivien (-lf), ja keston (-ft) mukaan. Tässä tapauksessa luultavasti järkevin tapa filtteröidä on koon mukaan, kun kaikki samanlaiset vastaukset tuntuvat olevan kooltaan 132 tavua, mutta sanojen tai rivienkin perusteella voisi toimia.
- Kokeillaan nyt filtteröidä vaikkapa tuon koon perusteella, eli aiemmin ajettuun komentoon lisätään -fs 132, jolloin suodatetaan 132 tavua pitkät vastaukset pois tuloksista. Näin saatiin siis suodatettua 'false positivet' pois tuloksista, ja jäljelle jäi meitä kiinnostava tieto, eli tässä tapauksessa admin-niminen hakemisto. Sama tulos tulisi myös, jos tuloksia filteröidään rivien (-fl 10), tai sanojen (-fw 6) perusteella.
  
  ![132](https://i.imgur.com/Fgvn4yE.png)
  
- Harjoituksen toisessa osuudessa täytyi ladata uusi harjoitus ja laittaa se päälle samalla tapaa kuin ensimmäinenkin, jonka jälkeen sieltä pitäisi saada löydettyä admin ja versionhallintaan liittyvät hakemistot.
- Looginen tapa aloittaa näiden etsiminen oli tietysti ajaa sama komento kun aiemminkin, ja tarkastella tuottaako se mitään mielenkiintoista. No tuottihan se eli seuraavaksi tarvitsee arvioida paras keino flitteröidä false positivet pois. Kun tuloksia tarkasteli, niin vilisi aika paljon 154 tavun kokoisia vastauksia, joten jos ne filtteröisi pois niin paljastuisiko jotain, eli lisätään -fs 154 aikaisempaan komentoon. Sieltähän löytyi etsityt hakemistot, eli wp-admin ja gitin hakemistoja, sekä yksi gitin redirecti.
  
	![fs154](https://i.imgur.com/Q4C78B0.png)

- #### b) Fuffme. Asenna [Ffufme harjoitusmaali](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/) paikallisesti omalle koneellesi. Ratkaise tehtävät (kaikki paitsi ei "Content Discovery - Pipes")
- Aloitin tehtävän lataamalla docker.io:n aptista sekä kloonaamalla tuon fuffme-hakemiston gitillä. ohjeen mukaisesti.  Tämän jälkeen laitoin Dockerin rullaamaan fuffme:n asennusta, mikä meni läpi sujuvasti. Tämän jälkeen koitin tuota ohjeestä lötyvää docker run -komentoa, joka tuotti seuraavan virheen:
  ```Error response from daemon: pull access denied for fuffme, repository does not exist or may require 'docker login': denied: requested access to the resource is denied.```
-  Tätä hetken pähkäiltyäni ja randomilla komentoja hakkailtuani avasin taas ohjeen, ja sieltähän löytyi troubleshooting- kohta, jossa mainittiin Apachen aiheuttavan ongelman palvelun kanssa, koska yhtä porttia voi kuunnella vain yksi Daemon kerrallaan. Mistä tulikin mieleeni, että tällä koneellahan on asennettuna nginx, jolla myös tuo samainen portti 80 käytössä, eli poistin sen ja testasin uudestaan tuota docker run -komentoa, ja palvelu lähti toimimaan.
	 ![dockrun](https://i.imgur.com/U77G3qB.png)

- Basic Content Discovery
	- Tämä tehtävä oli käytännössä sama, kuin ylempänä jo tehty Teron tehtävä, joten ei samoja kommentteja toiste.
		![yks](https://i.imgur.com/wBiWyLO.png)

- Content Discovery With Recursion
	- Tämäkin oli aika samaa kauraa, paitsi että komentoon lisättiin määrite -recursion, mikä siis seuraa hakuehdot täyttäviä kansioita, niiden alikansioihin, ja näyttää myös ne tuloksissa.
		![kaks](https://i.imgur.com/ZI6IqBL.png)

- No 404 Status
	 - Tässä taas käytetään tuota filtteröintiä, eli 404 statuksen sisältävät vastaukset filtteröidään tuloksista pois niiden koon perusteella, eli käytetään -fs 669 (nice).
	 ![neli](https://i.imgur.com/kvVBVaZ.png)

- Param Mining
	- Tässä etsitään sopivaa parametriä datalle, joten tarvitaan ensinäkin eri sanalista hakuun, joka oli tehtävän ohjeissa parameters.txt, mutta itse kaivoin tuolta SecLististä sellaisen kuin burp-parameter-names, joka myös ajoi asian. Lisäksi itse komento muuttuu hieman, kun siihen tulee FUZZ=1 pelkän FUZZ:in sijaan. Kun tuon listan ajaa läpi löytyy debug-parametri, jonka voi syöttää tuon osoiteriville tuon data?:n jälkeen, jolloin päästään kyseiselle sivulle.
		![vii](https://i.imgur.com/AoSMnAA.png)
- Rate Limited
	- Tämä harjoitus itseasiassa vastaa kysymykseen, mikä itselläni heräsi katsoessani tuota joohoin Still Fuzzing Faster than (U Fool) -esitystä, sillä ymmärtääkseni edes vähänkään sinnepäin konffattu web-palvelin antaa jonkinlaista bannia, jos laitat sinne 5000 requestia sekunnissa. Ffuf:issa siis saa määriteltyä ensinäkin monta requestia (-p) se lähettää per sekunti, ja kuinka monesta 'threadista' (-t, säie, suomeksi??) niitä lähetetään samanaikaisesti. 
	- Tässä harjoituksessa siis palvelimelta tulee http 429-koodia (too many requests) vastaukseksi, kun komentoa koitetaan ajaa samalla tavalla kuin aiemmissa tehtävissä. Tulokset on myös filteröity http-status-koodien mukaan (-mc), eli vain 200 & 429 vastaukset näkyvät.
	- Kun komentoon lisätään -t 5 (oletus 40), ja -p 0.1 lähetetään pyyntöjä siis vain viidestä threadista, ja niiden välillä on 0.1 sekunnin viive, eli haku on huomattavasti hitaampi, mutta se menee läpi, eli takaisin ei tule litaniaa 429:sta, vaan löydetään oracle-niminen hakemisto.
		![kuu](https://i.imgur.com/6pVeaFH.png)

- Subdomains - Virtual Host Enumeration
	-  Tässä etsitään subdomaineja virtual hostien ja pääasiallisen hostien headereitä muokkaamalla (toimintaperiaate jäi kyllä vähän ymmärtämättä itseltäni).
	- Koska etsitään subdomaineja tarvitaan siihen sopiva sanalista, eli tehtävässä annettiin subdomains.txt, mutta itse taas kaivoin tuolta SecLististä sopivan kuuloisen listan, eli subdomains-top1million-5000.txt -nimisen listan. Lisäksi komentoon tarvitsee määritellä, että etsitään headereistä (-H), ja domain mistä etsitään (Host: "esi.merk.ki").
	- Hieman yllättävää oli se, että tuolla top5000-listalla ei löytynyt mitään, joten täytyi vaihtaa tuosta samasta listasta top110000, jolla löytyi redhat-niminen alidomain.
		![see](https://i.imgur.com/R7bXQIB.png)

#### c) Porttiskannaa paikallinen kone (127.0.0.2 tms), sieppaa liikenne snifferillä, analysoi.
**nmap TCP SYN "used to be stealth" scan, -sS (tätä käytetään skannatessa useimmin)**
- Suoritin normaalin skannauksen nmapilla localhostille, eli se skannaa tuhat yleisintä tcp-porttia SYN-skannauksella, ja samanaikaisesti kaappasin liikenteen Wiresharkilla. Tässä skannauksessa SYN-paketti lähetetään samoin kuin oltaisiin avaamassa yhteyttä tavallisesti, ja jos siihen tulee ACK-vastaus portti on avoinna, jos taas RST portin voidaan olettaa olevan kiinni, ja jos vastausta ei kuulu lainkaan portti merkataan filtered-tilaan.
- Kun kaapattua liikennettä tarkastellaan Wiresharkissa ensimmäisenä huomio kiinnittyy siihen, että ruutu on täynnä punaisia ja harmaita rivejä, jos kenttiä tarkastellaan vasemmalta oikealle saadaan ensinäkin selville lähettäjän ja vastaanottajan ip-osoite (localhost molemmissa), käytetty protokolla (TCP), pakettien koko tavuina, ja sen jälkeen tietoa itse paketista, joka on tässä tapauksessa se mikä meitä kiinnostaa. Jos nyt valitaan sattumanvaraisesti yksi harmaa rivi ja tarkastellaan tuota info-kohtaa, niin siitä saamme selvillee ensinäkin lähde- ja kohdeportit, jotka ovat tässä SRC: 51620 ja DST: 1417. Tuosta lähdeportin numerosta voimme päätellä, että se on ns. dynaaminen portti (portit 49152-65535), ja taas kohdeportti kuulunee jollekkin palvelulle (Timbuktu Pro Windows), minkä voimme päätellä alhaisesta porttinumerosta (ja siitä, että suoritimme skannauksen 1000:lle yleisimmälle portille). Seuraavana rivillä näkyy SYN, eli kyseessä on SYN-paketti, ja Seq=0 kertoo, että se on tämän TCP-yhteyden ensimmäinen paketti. Seuraava Win=1024 kertoo sallitun TCP-ikkunan koon, eli kuinka paljon dataa voidaan siirtää, ennenkuin tarvitaan ACK takaisin (ikkunan koko neuvotellaan dynaamisesti tässä tcp 3way handshaken yhteydessä). Len=0 taas kertoo, että paketilla ei tässä tapauksessa ole muuta sisältöä, kuin headerit, ja MSS=1460 kertoo paketin segmentin maksimikoon.
- Jos tarkastellaan tähän pakettiin tullutta vastausta, niin lähde- ja kohdeportit ovat kääntyneet toisinpäin (sama ip:eille yleensä), vastauksessa on RST -ja ACK-liput, eli yhteys suljetaan ja viestitään, koska portissa ei ole mitään palvelua, ja että edellinen paketti tuli perille. Seq on saanut arvokseen 1, koska tämä on yhteyden seuraava paketti ja tcp-ikkuna on näemmä 0 (varmaankin koska yhteys suljetaan), ja paketin sisältö on kooltaan 0, koska viestimisessä käytetään pelkästään headereitä.
	![syn1417](https://i.imgur.com/wLJp1TU.png)

  **nmap TCP connect scan -sT**
    - Jos tarkastellaan tämän skannauksen eroavaisuuksia SYN-skannauksesta huomataan, että Wiresharkiin on ilmestynyt pari uutta arvoa info-kenttään. Tämä johtunee siitä, että skannaus käyttää käyttöjärjestelmän connect callia skannauksen toteuttamiseen, jolloin olettaisin että käytössä on käyttöjärjestelmän TCP/IP-pinon mukaiset paketit, joiden headerit sisältävät enemmän kenttiä yhteyden parantamiseksi. Esim. kuvassa näkyvä SACK on packet lossia vähentämään kehitetty menetelmä, ja TSval & TSecr ovat aikaleimoja. Myöskin jos tarkastellaan Wiresharkissa sellaista tilannetta, jossa skannatussa portissa on palvelu, niin huomataan, että -sT skannauksessa 3way handshake tehdään loppuun asti, eli kun portti on jo vastannut siihen lähetetään vielä ACK-sanoma.
      ![sTxDD](https://i.imgur.com/0Vzz7A1.png)
      
      **nmap ping sweep -sn**
      - Mielenkiintoisesti kun suoritan tämän ping sweep skannauksen, niin Wiresharkissa ei näy mitään. Vaikea kyllä keksiä mistä johtuu, kun skannaus kuitenkin nmapissa menee läpi ja normi pingaus näkyy Wiresharkissa. Itseasiassa tuo pingikin näkyy vain ICMPv6 protokollana (eli IPv6 käytössä), eli olisikohan tässä nyt vain joku filtteröinti ongelma mulla Wiresharkissa, ei kyllä nyt kerkeä enempää tutkimaan.
        
        ![sad](https://i.imgur.com/2FHqaTX.png)
        
      **nmap don't ping -Pn**
        
      - Mielestäni tulos näyttää ihan tavalliselta SYN skannaukselta, ja niin se vissiin onkin, koska -pn toimii siten, että se skippaa muutoin skannauksissa oletuksena suoritettavan host discoveryn, ja olettaa suoraan että kaikki skannattavat hostit ovat online.

        **nmap version detection -sV (esimerkki yhdestä palvelusta yhdessä portissa riittää)**
        
      - Kun tarkastellaan tätä skannausta Wiresharkilla skannaus alkaa tavallisella SYN-skannauksella, jolla saadaan selville, että portti on auki, jonka jälkeen sille lähetetään uusi hieman erilainen SYN-paketti, minkä vastauksen perusteella luultavasti tehdään jonkinlaisia päätelmiä, ja tässä tapauksessa suoritettiin 3way handshake loppuun, josta saatiin selville palvelun versio ja käyttöjärjestelmä.
     
        ![12414](https://i.imgur.com/RXpwrNP.png)

        **nmap output files -oA foo. Miltä tiedostot näyttävät? Mihin kukin tiedostotyyppi sopii?**
        
      - Tuolla -oA:lla tuli tuonne kolmea eri tiedostomuotoa (.nmap, .gnmap, .xml), joista ainakin .nmap ja .gnmap näyttivät tulokset luettavassa muodossa, ja .xml:hän on strukturoitua dataa, joten sitä varmaan voi käyttää kaikenlaisessa automatisoidussa vertailussa ja analysoinnissa.
      
      **nmap ajonaikaiset toiminnot (man nmap: runtime interaction): verbosity v/V, help ?, packet tracing p/P, status s (ja moni muu nappi)**
      
      - Vähän kerkesin näitä näpytellä, niin statuksella näkyy meneillä oleva skannaus, sen tila prosentteina, kulunut aika, sekä loppuun asti skannattujen hostien määrä. P taas enabloi packet tracingin eli terminaalissa näkyy mihin porttiin ja palveluun pakettejä lähetetään, ja v:llä voidaan lisätä verbosity leveliä, eli kuinka paljon settiä tuloksessa näkyy
        
      **Ninjojen tapaan. Piiloutuuko nmap-skannaus hyvin palvelimelta? Vinkkejä: Asenna Apache. Aja nmap-versioskannaus -sV tai -A omaan paikalliseen weppipalvelimeen. Etsi Apachen lokista tätä koskevat rivit. Wiresharkissa "http" on kätevä filtteri, se tulee siihen yläreunan "Apply a display filter..." -kenttään. Nmap-ajon aikana p laittaa packet tracing päälle. Vapaaehtoinen lisäkohta: jääkö Apachen lokiin jokin todiste nmap-versioskannauksesta?**
        
	    - Aiemmin kun oli jo asennettu tuo fuffme, jossa siis pyörii ngnix, niin ajattelin sitä pommitella, kun tuolla Wiresharkissakin näkyi kaapattavissa interfaceissa tuo docker, joten valitsin sen, avasin selaimen käväisin siellä fuffme-sivulla, josta sain ip-osoitteen näkyviin Wireshark kaappaukseen. Kysymykseen piiloutuuko nmap skannaus hyvin web-palvelimelta voisin veikata vastaukseksi, että ei piiloudu. Ainakin kun kaapattuja paketteja selaa http-filtteri päällä vastauksina on tullut paljon 405 (not allowed), joten luulenpa että skannaus koittaa tehdä vähän kaikenlaista jännää, mistä jää jälki.
        
	       ![assda](https://i.imgur.com/8a46O9k.png)
        
	    - Eihän se nmapin -A skannaus kovinkaan huomaamaton tosiaan ollut, kun access-logista löytyy suoraan mainittuna nmap scripting engine.
        
	       ![log](https://i.imgur.com/8iLsPse.png)
        
**UDP-skannaus. UDP-skannaa paikkalinen kone (-sU). "Mulla olis vitsi UDP:sta, mutta en tiedä menisikö se perille"**
  	
   - tba

**Miksi UDP-skannaus on hankalaa ja epäluotettavaa? Miksi UDP-skannauksen kanssa kannattaa käyttää --reason flagia ja snifferiä? (tässä alakohdassa vain vastaus viitteineen, ei tarvita testiä tietokoneella)**
- Koska UDP on yhteydetön protokolla siitä puuttuu yhteyden muodostus osio (TCP:ssä 3way handshake), jolloin siinä ei luonnostaa ole yhtä yksinkertaista ja helppoa tapaa määrittää porttien tilaa. Myöskään UDP:ssä ei ole pakettien perille menon varmistamiseksi mitään tapaa, joten jos portti ei vastaa siitä on vaikea päätellä mitään suoraan, koska portti voi olla joko kiinni, palomuurin filtteröimä, tai auki mutta palvelu ei vastaa jostain muusta syystä. Tästä syystä on siis hyödyllistä käyttää snifferiä, koska kaapatuista paketeista voidaan päätellä enemmän portin tilasta.

#### Lähteet

- https://nmap.org/book/man-port-scanning-basics.html
- https://nmap.org/book/man-port-scanning-techniques.html
- https://nmap.org/book/man-output.html
- https://en.wikipedia.org/wiki/Ephemeral_port
- https://www.networkcomputing.com/data-centers/network-analysis-tcp-window-size
