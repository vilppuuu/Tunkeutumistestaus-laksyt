## H2 Läksyt - Sniff-n-Scan

Sniff-n-scan tutustuttaa uuteen lähteeseen, hakkeritapahtumien nauhoihin. Opit valvomaan hyökkäystyökalujen toimintaa snifferillä, ja tutustut Wiresharkiin. Wireshark osaa myös analysoida paketit automaattisesti. Weppiin murtautumista auttaa suomalainen, fuzzereiden huipulle noussut ffuf. Harjoitusmaalien asentamisesta kokeillaan paikallisia binäärejä (by yours truly) ja Dockeria._

#### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
   
- Hoikkala "joohoi" 2020: [Still Fuzzing Faster (U fool)](https://www.youtube.com/watch?v=mbmsT3AhwWU). In HelSec Virtual meetup #1. (about 1 hour)
   
- Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide:
        - [Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) (opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)
        - [Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html) (opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)
	
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
- Harjoituksen toisessa osuudessa täytyi ladata uusi harjoitus ja laittaa se päälle samalla tapaa kuin ensimmäinenkin, jonka jälkeen sieltä pitäisi saada löydettyä admin ja versionhallintaan liittyvät hakemistot.
- Looginen tapa aloittaa näiden etsiminen oli tietysti ajaa sama komento kun aiemminkin ja tuloksesta, ja tarkastella tuottaako se mitään. No tuottihan se eli seuraavaksi tarvitsee arvioida paras keino flitteröidä false positivet pois. Kun tuloksia tarkasteli, niin vilisi aika paljon 154 tavun kokoisia vastauksia, joten jos ne filtteröisi pois niin paljastuisiko jotain, eli lisätään -fs 154 aikaisempaan komentoon. Sieltähän löytyi etsityt hakemistot, eli wp-admin ja gitin hakemistoja, sekä yksi gitin redirecti.
		![fs154](https://i.imgur.com/Q4C78B0.png)

- #### b) Fuffme. Asenna [Ffufme harjoitusmaali](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/) paikallisesti omalle koneellesi. Ratkaise tehtävät (kaikki paitsi ei "Content Discovery - Pipes")
- Aloitin tehtävän lataamalla docker.io:n aptista sekä kloonaamalla tuon fuffme-hakemiston gitillä. ohjeen mukaisesti.  Tämän jälkeen laitoin Dockerin rullaamaan fuffme:n asennusta, mikä meni läpi sujuvasti. Tämän jälkeen koitin tuota ohjeestä lötyvää docker run -komentoa, joka tuotti seuraavan virheen:
  ``Error response from daemon: pull access denied for fuffme, repository does not exist or may require 'docker login': denied: requested access to the resource is denied.`
-  Tätä hetken pähkäiltyäni ja randomilla komentoja hakkailtuani avasin taas ohjeen, ja sieltähän löytyi troubleshooting- kohta, jossa mainittiin Apachen aiheuttavan ongelman palvelun kanssa, koska yhtä porttia voi kuunnella vain yksi Daemon kerrallaan. Mistä tulikin mieleeni, että tällä koneellahan on asennettuna nginx, jolla myös tuo samainen portti 80 käytössä, eli poistin sen ja testasin uudestaan tuota docker run -komentoa, ja palvelu lähti toimimaan. 
	 ![dockrun](https://i.imgur.com/Q4C78B0.png)
- Basic Content Discovery
	- Tämä tehtävä oli käytännössä sama, kuin ylempänä jo tehty Teron tehtävä, joten ei samoja kommentteja toiste.
		![yks](https://i.imgur.com/wBiWyLO.png)
- Content Discovery With Recursion
	- Tämäkin oli aika samaa kauraa, paitsi että komentoon lisättiin määrite -recursion, mikä siis seuraa hakuehdot täyttäviä kansioita, niiden alikansioihin, ja näyttää myös ne tuloksissa.
		![kaks](https://i.imgur.com/ZI6IqBL.png)
- Content Discovery With File Extensions
	-  Tässä näytettään tuota -e (extensions), johon siis voidaan määritellä tiedostotyyppi, kuten tässä tapauksessa etsitään lokitiedostoa .log päätteellä.
		![koli](https://i.imgur.com/odelfuz.png)
- No 404 Status
	 - ASD
	 ![neli](https://i.imgur.com/kvVBVaZ.png)
- Param Mining
	- burp
		[vii](https://i.imgur.com/ssvBYce.png)
- Rate Limited
- Subdomains - Virtual Host Enumeration
  
