## h7 Hash

### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**Karvinen 2022: Cracking Passwords with Hashcat**

**Karvinen 2023: Crack File Password With John**

### a) Hashcat. Asenna Hashcat ja testaa sen toimivuus ratkaisemalla tiiviste.

- Kerkesin jo viime viikolla tuossa [c-tehtävässä](https://github.com/vilppuuu/tunkeutumistestaus/blob/main/h6_laksyt.md#credential-access-brute-force--password-cracking-t1110002) testailla tätä jo Linuxilla, niin tällä kertaa voisi kokeilla käyttää Windowsissa, jolloin saa näyttiksellä murrettua tiivisteitä, mikä on huomattavasti tehokkaampaa.
- Googlella löytyi simppeli [video-ohje](https://www.youtube.com/watch?v=KLry7bf51QQ) hashcatin lataaamiseen ja käyttämiseen Windowsilla, joten seurailin sitä. Alkuun tuolta [hashcatin sivuilta](https://hashcat.net/hashcat/) *hashcat binaries* lataukseen, ja sen unzippaaminen. Ilmestyneeseen hashcat-kansioon täytyy luoda kaksi eri tekstitiedostoa, joista toinen tulee sisältämään murrettavat tiivisteet ja toinen toivottavasti murretut salasanat (nimetään nämä vaikka *hash.txt & salat.txt*).
- Testausta varten videossa kehotettiin luomaan helppo MD5-hash tällä [generaattorilla](https://www.md5hashgenerator.com/), ja saatu tiiviste sijoitettiin tuonne hash-tekstitiedostoon. Tämän jälkeen annettiin käytettävä komento: `hashcat -m0 -a3 -o salat.txt hash.txt`, jossa siis **-m0** on tiivisteessä käytetty algoritmi (MD5), ja **-a3** käytettävä hyökkäystyyli (3 = brute-force), ja **-o** on output-filun määrittely, ja viimeisenä siis annetaan tiedosto, jossa tiivisteet sijaitsevat.
- Ensiyrittämällä komento ei Windowsissa toiminut, vaan valitteli puutteellisesta komennosta, mutta suoraan valitusvirren alla oli ehdotus, että hashcat-niminen komento kyllä löytyy tästä lokaatiosta, ja jos luotat siihen niin sen saa ajettua lisäämällä **.\** sen alkuun.

  ![errrr](https://i.imgur.com/Bj8olxp.png)

- Tämä tosiaankin toimi, ja eipä tuollaisen kahdeksan merkkiä pitkän salasanan murtamisen aikana turhan paljon kerennyt vanhenemaan.

  ![crackkkk](https://i.imgur.com/vMOgzxN.png)

### b) John. Asenna Jumbo John ja testaa sen toimivuus murtamalla jonkin tiedoston salasana.

### c) f5bc7fcc7f5b3b6af7ff79e0feafad6d1a948b6a2c18de414993c1226be48c1f on erään tällä tehtäväsivulla olevan yksittäisen sanan tiiviste. Käytin hyvin yleistä ja tunnettua tiivistealgoritmia. Sanassa voi olla isoja kirjaimia, mutta ei erikoismerkkejä. Minkä sanan tiiviste on kyseessä?

### d) Cheatsheet. Kerää kurssilaisten raporteista käteviä tekniikoita. Kerää itse tekniikat ja komennot, älä pelkästään kuvaile. Muista lähdeviitteet. Tee tiivis ja selkeä cheatsheet, josta löydät tarvittavat tiedot lipunryöstössä. (Tässä alatehtävässä ei tarvitse tehdä testejä koneella)

### e) Viittaa. Tarkista, että jokaisessa raportissasi on lähdeviitteet kunnossa. Jokaisen raportin tulee viitata ainakin kurssiin / tehtäväsivuun. Kaikkiin muihinkin käytettyihin lähteisiin tulee viitata, kuten kurssikavereiden raportteihin, weppisivuihin, man-sivuihin... (Tässä alatehtävässä ei tarvitse tehdä testejä koneella).
