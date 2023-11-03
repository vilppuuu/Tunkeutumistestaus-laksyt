## H2 Läksyt - Sniff-n-Scan

Sniff-n-scan tutustuttaa uuteen lähteeseen, hakkeritapahtumien nauhoihin. Opit valvomaan hyökkäystyökalujen toimintaa snifferillä, ja tutustut Wiresharkiin. Wireshark osaa myös analysoida paketit automaattisesti. Weppiin murtautumista auttaa suomalainen, fuzzereiden huipulle noussut ffuf. Harjoitusmaalien asentamisesta kokeillaan paikallisia binäärejä (by yours truly) ja Dockeria._

#### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
   
- Hoikkala "joohoi" 2020: [Still Fuzzing Faster (U fool)](https://www.youtube.com/watch?v=mbmsT3AhwWU). In HelSec Virtual meetup #1. (about 1 hour)
   
- Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide:
        - [Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) (opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)
        - [Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html) (opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)
	
#### a) Fuff. Ratkaise [Teron ffuf-haastebinääri](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge). Artikkelista [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) voi olla apua.
	
 - Harjoituksen ideana on löytää piilotettuja hakemistoja web-palvelimelta, jotta tätä olisi mitenkään järkevä toteuttaa tarvitaan siihen työkalu, jolla prosessi voidaan automatisoida.
		- Tällä kurssilla käytetty ja suositeltu työkalu on fuff, jota voidaan hakemistojen etsimisen lisäksi käyttää myös esim. virtuaalihostien etsimiseen, käyttäjien ja salasanojen syöttämiseen ja muuhun.
- Aloitin harjoituksen  lataamalla ja käynnistämällä harjoitusmaalin ohjeen mukaisesti, sekä ffuf:in (sai suoraan apt:ista asennettua, toisin kuin ohjeessa), ja testaamalla niiden toimivuuden.

	![fuff](https://i.imgur.com/Zj81xv9.png) 
		
	![test](https://i.imgur.com/NkYl0Kx.png)
  
- Olin jo ladannut tuon Githubista SecList.zipin (https://github.com/danielmiessler/SecLists) katsottuani tuon Still Fuzzing Faster (U fool) -esityksen, kun se siellä mainittiin ja linkki löytyi esityksen lopusta. Eli täytyi vain tuon common.txt etsiä sieltä laittaa sen polku tuohon komentoon.
-  Komennossa siis -w määrittelee käytettävän sanalistan, ja -u urlin, jota vastaan tuota sanalistaa ajetaan.
  
 	![fuzz](https://i.imgur.com/Tq4tv0f.png)

-  Tämä tulos ei varsinaisesti ole kovin hyödyllinen, koska se on vajaa 5000 riviä pitkä, joten tarvitaan keino filtteröidä sitä.
- ffuf:issa tuloksia voidaan filteröidä ainakin HTTP-statuksen (-fc), koon (-fs), sanojen (-fw), rivien (-lf), ja keston (-ft) mukaan.
- Tässä tapauksessa luultavasti järkevin tapa filtteröidä on koon mukaan, kun kaikki samanlaiset vastaukset tuntuvat olevan kooltaan 132 tavua, mutta sanojen tai rivienkin perusteella voisi toimia.
- Kokeillaan nyt filtteröidä vaikkapa tuon koon perusteella, eli aiemmin ajettuun komentoon lisätään -fs 132, jolloin suodatetaan 132 tavua pitkät vastaukset pois tuloksista.
- Näin saatiin siis suodatettua 'false positivet' pois tuloksista, ja jäljelle jäi meitä kiinnostava tieto, eli tässä tapauksessa admin-niminen hakemisto. Sama tulos tulisi myös, jos tuloksia filteröidään rivien (-fl 10), tai sanojen (-fw 6) perusteella.
- 
