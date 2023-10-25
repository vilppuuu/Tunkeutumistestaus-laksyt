# h1 Hacker Warmup

Tässä harjoituksessa on tiedustelua ja lämmittelyä. Pääset lukemaan sen kuuluisan paperin kybertappoketjusta. Tutustut O'Reilly Learning -alustan videokursseihin. Kokeilet montaa harjoitusympäristöä. Jos hakkerointi sujuu jo, näillä samoilla alustoilla voit harjoitella myös kurssin ulkopuolella.

### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
€ Santos et al: The Art of Hacking (Video Collection): [..] 4.3 Surveying Essential Tools for Active Reconnaissance. Sisältää porttiskannauksen. 5 videota, yhteensä noin 20 min.
Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.
### a) Ratkaise Over The Wire: Bandit kolme ensimmäistä tasoa (0-2).

- Level 0 alkaa tosiaan ottamalla ssh-yhteys koneeseen, jossa 'peli' sijaitsee, eli yksinkertaisin lienee Linuxissa tai Windowsissa terminaalissa käyttöää ssh:ta. Eli komennolla *ssh bandit0@bandit.labs.overthewire.org -p 2220* päästään yhdistämään, eli tuossa
näkyy tuo bandit0, joka on käyttäjänimi ja kone, johon yhdistetään on @:n jälkeen oleva rimpsu, ja tosiaan -p määritetään portti, jota tässä oli ohjeistettu käyttämään oletusportin (22) sijasta. Tämä kun on laitettu terminaaliin, yhteys kysyy salasanaa, joka tässä oli turvallisesti määritelty samaksi kuin käyttäjänimi.

- Level 1 piti löytää salasana readme-nimisestä tiedostosta, joka löytyy koneen johon juuri yhdistimme home directorystä. Komennoilla pwd, ls, ja cat saamme selville hakemiston, siinä sijaitsevat tiedoston ja tiedoston sisällön, joka oli siis salasana seuraavaan leveliin kirjautumiseen.

- Level 2 siis otettiin ssh-yhteys, muuuten samalla tavalla kuin aiemmin, mutta käyttäjällä bandit1 ja edellisessä levelissä löydetyllä salasanalla. Tässä ideana on kaivaa salasana tiedostosta, joka on nimetty "-". Pelkästään 'cat -' ei toimi, koska se lukee - jonain STDIN(?):nä ja odottaa käyttäjältä lisää syötettä. Sen sijaan, jos laittaa tiedoston polun cat-komentoon, eli esim cat ./- se lukee sen normaalisti, ja salasanan saa esiin.

![lvl1](https://github.com/vilppuuu/tunkeutumistestaus/assets/103587907/18769ff3-8bfc-46a1-a631-3a96a783724d)

![lvl2](https://github.com/vilppuuu/tunkeutumistestaus/assets/103587907/83e2b05f-8cde-4493-897c-0001b07b6adb)

b) Ratkaise Challenge.fi:stä yksi tehtävä, esim. Challenge.fi 2021 Where was this picture taken, Encoding basics. Tai joku Challenge.fi 2022. (Nimi vaihtui sattuneesta syystä, se on nykyisin Next Gen Hack Challenge)
c) Ratkaise PortSwigger Labs: Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. (Edellyttää ilmaista rekisteröitymistä. Tehtävän voi ratkaista pelkästään selaimen osoitekenttää muokkaamalla.)
d) Asenna Linux virtuaalikoneeseen. Suosittelen joko Kali (viimeisin versio) tai Debian 12-Bookworm.
e) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (localhost). Analysoi tulokset.
f) Porttiskannaa kaikki koneesi (localhost) tcp-portit. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin).
g) Tee laaja porttiskanaus (nmap -A) omalle koneellesi (localhost), kaikki portit. Selitä, mitä -A tekee. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin.).
h) Asenna ja käynnistä jokin palvelin (apache, ssh...) koneellesi. Vertaile, miten porttiskannauksen tulos eroaa.
i) Kokeile ja esittele jokin avointen lähteiden tiedusteluun sopiva weppisivu tai työkalu. Hyviä esimerkkejä löytyy Bazzel: IntelTechniques: Tools ja Bellingcat: Resources, voit myös käyttää muuta itse valitsemaasi työkalua. Työkalua pitää siis myös kokeilla, pelkkä nimen mainitseminen ei riitä. Pidä esimerkit harmittomina, älä julkaise kenenkään henkilökohtaisia salaisuuksia raportissasi.
j) Vapaaehtoinen: Tee lisää harjoituksia alustoilta, joihin tässä on tutustuttu: PortSwigger Academy, Over the Wire, Challenge.fi. Hakkeroimaan oppii hakkeroimalla.
