## H2 Läksyt - Sniff-n-Scan

Sniff-n-scan tutustuttaa uuteen lähteeseen, hakkeritapahtumien nauhoihin. Opit valvomaan hyökkäystyökalujen toimintaa snifferillä, ja tutustut Wiresharkiin. Wireshark osaa myös analysoida paketit automaattisesti. Weppiin murtautumista auttaa suomalainen, fuzzereiden huipulle noussut ffuf. Harjoitusmaalien asentamisesta kokeillaan paikallisia binäärejä (by yours truly) ja Dockeria._

- x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
    - Hoikkala "joohoi" 2020: [Still Fuzzing Faster (U fool)](https://www.youtube.com/watch?v=mbmsT3AhwWU). In HelSec Virtual meetup #1. (about 1 hour)
    - Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide:
        - [Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) (opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)
        - [Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html) (opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)
- a) Fuff. Ratkaise [Teron ffuf-haastebinääri](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge). Artikkelista [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) voi olla apua.
	- Harjoituksen ideana on löytää piilotettuja hakemistoja web-palvelimelta, jotta tätä olisi mitenkään järkevä toteuttaa tarvitaan siihen työkalu, jolla prosessi voidaan automatisoida.
		- Tällä kurssilla käytetty ja suositeltu työkalu on fuff, jota voidaan hakemistojen etsimisen lisäksi käyttää myös esim. virtuaalihostien etsimiseen, käyttäjien ja salasanojen syöttämiseen ja muuhun.
	- Aloitin harjoituksen lataamalla ja käynnistämällä harjoitusmaalin, sekä ffuf:in, ja testaamalla niiden toimivuuden.
		[](https://i.imgur.com/is7WnN9.png) 
		[](https://i.imgur.com/NkYl0Kx.png)
	- 
