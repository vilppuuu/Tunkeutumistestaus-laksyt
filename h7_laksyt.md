## h7 Hash

### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**Karvinen 2022: Cracking Passwords with Hashcat**

- Alussa asennellaan aptista ihan normaalisti hashcat & hashid, sekä ladataan tuolta SecLististä salasanalista.
- Seuraavaksi on tuota hashid:n käyttöä, jolla voidaan ainakin jotenkuten tunnistaa tiivisteessä käytetty algoritmi, ja se myös antaa hashcatiin murtamisessa tarvittavan 'moden' numeron. En kyllä ymmärrä millä perusteella tämä näitä tunnistaa, muuten kuin merkkijonon pituuden perusteella (esim. MD5 32 vs SHA1 40)? Ei varmaan mitenkään muuten, eli loppujenlopuksi sopivin algoritmi täytyy valita yleisyyden ja kontekstin perusteella.
- Loppuartikkeli kuvaa tuota hashcatin käyttöä, jota setvin jo tarkemmin tuossa alapuolella ja viime viikon [tehtävissä](https://github.com/vilppuuu/tunkeutumistestaus/blob/main/h6_laksyt.md#credential-access-brute-force--password-cracking-t1110002), joten skipedi skip tähän, ettei tule turhaa toistoa.

**Karvinen 2023: Crack File Password With John**

- 

### a) Hashcat. Asenna Hashcat ja testaa sen toimivuus ratkaisemalla tiiviste.

- Kerkesin jo viime viikolla tuossa [c-tehtävässä](https://github.com/vilppuuu/tunkeutumistestaus/blob/main/h6_laksyt.md#credential-access-brute-force--password-cracking-t1110002) testailla tätä jo Linuxilla, niin tällä kertaa voisi kokeilla käyttää Windowsissa, jolloin saa näyttiksellä murrettua tiivisteitä, mikä on huomattavasti tehokkaampaa.
- Googlella löytyi simppeli [video-ohje](https://www.youtube.com/watch?v=KLry7bf51QQ) hashcatin lataaamiseen ja käyttämiseen Windowsilla, joten seurailin sitä. Alkuun tuolta [hashcatin sivuilta](https://hashcat.net/hashcat/) *hashcat binaries* lataukseen, ja sen unzippaaminen. Ilmestyneeseen hashcat-kansioon täytyy luoda kaksi eri tekstitiedostoa, joista toinen tulee sisältämään murrettavat tiivisteet ja toinen (toivottavasti) murretut salasanat (nimetään nämä vaikka *hash.txt & salat.txt*).
- Testausta varten videossa kehotettiin luomaan helppo MD5-hash tällä [generaattorilla](https://www.md5hashgenerator.com/), ja saatu tiiviste sijoitettiin tuonne hash-tekstitiedostoon. Tämän jälkeen annettiin käytettävä komento: `hashcat -m0 -a3 -o salat.txt hash.txt`, jossa siis **-m0** on tiivisteessä käytetty algoritmi (MD5), ja **-a3** käytettävä hyökkäystyyli (3 = brute-force), ja **-o** on output-filun määrittely, ja viimeisenä siis annetaan tiedosto, jossa tiivisteet sijaitsevat.
- Ensiyrittämällä komento ei Windowsissa toiminut, vaan valitteli puutteellisesta komennosta, mutta suoraan valitusvirren alla oli ehdotus, että hashcat-niminen komento kyllä löytyy tästä lokaatiosta, ja jos luotat siihen niin sen saa ajettua lisäämällä **.\** sen alkuun.

  ![errrr](https://i.imgur.com/Bj8olxp.png)

- Tämä tosiaankin toimi, ja eipä tuollaisen kahdeksan merkkiä pitkän salasanan murtamisen aikana turhan paljon kerennyt vanhenemaan.

  ![crackkkk](https://i.imgur.com/vMOgzxN.png)

### b) John. Asenna Jumbo John ja testaa sen toimivuus murtamalla jonkin tiedoston salasana.
- Alkuun tuon ylhäällä olevan [Teron artikkelin](https://terokarvinen.com/2023/crack-file-password-with-john/l) mukaisesti aloin asentelemaan noita tarvittavia riippuvuuksia, ja koska copy/paste VM:lle ei toiminut asentelin manuaalisesti vain nuo välttämättömät build-essential, libssl-dev, ja zlib1g zlib1g-dev zlib1g-gst (git, wget & bash-completion olivat muutenkin jo asennettuina).
- Seuraavaksi saman ohjeen mukaisesti kloonasin tuon 'johnin' gitillä, ja siellä suuntasin john/src- hakemistoon, jossa ajoin tuon ./configure-komennon. Tämä itseasiassa antoi pari valitusta puuttuvista riippuvuuksista, eli 7z ei tue BZIP2:sta koska unohdin asentaa libbz2 tuossa aiemmin, ja myös useampi rivi valitusta liittyen pkcs12-tiedostoihin. Tuo configure kuitenkin näytti menevän läpi ihan ok, joten päätin olla välittämättä noista virheistä. Lähinnä siksi, että en ajatellut tehdä nyt mitään pkcs12 liittyvää ja tuo 7z virhe ei myöskään vaikuta tähän. Muistaakseni noihin pkcs12-tiedostoihin voidaan tallentaa sertifikaatteja ja avaimia samaan tiedostoon (joskus piti töissä purkaa tuollainen OpenSSL:llä, että sai sertifikaatin sieltä kaivettua).
- Eli ajoin tuon make-komennon, ja se meni läpi ilman valituksia, ja tuo john:kin toimii.

  ![asfag235](https://i.imgur.com/eBYxn3D.png)

  ![asdasdad](https://i.imgur.com/ymmDAbJ.png)
  
### c) f5bc7fcc7f5b3b6af7ff79e0feafad6d1a948b6a2c18de414993c1226be48c1f on erään tällä tehtäväsivulla olevan yksittäisen sanan tiiviste. Käytin hyvin yleistä ja tunnettua tiivistealgoritmia. Sanassa voi olla isoja kirjaimia, mutta ei erikoismerkkejä. Minkä sanan tiiviste on kyseessä?

- Hashid:llä kun tsekkaa tuon tiivisteen, niin antaa alla olevan listan algoritmejä. Jos kerran kyseessä on yleisesti käytetty tiivistealgoritmi, niin voisi veikata sen olevan tuo **SHA-256**, koska se nyt on ainoa mikä noista itsellä soittaa mitään kelloja, ja eipä tuolla listallakaan ole kuin kahdelle muulle algoritmille edes tuota hashcat-modea.

    ![hasdasdas](https://i.imgur.com/scIt4R3.png)

- 

### d) Cheatsheet. Kerää kurssilaisten raporteista käteviä tekniikoita. Kerää itse tekniikat ja komennot, älä pelkästään kuvaile. Muista lähdeviitteet. Tee tiivis ja selkeä cheatsheet, josta löydät tarvittavat tiedot lipunryöstössä. (Tässä alatehtävässä ei tarvitse tehdä testejä koneella)

### e) Viittaa. Tarkista, että jokaisessa raportissasi on lähdeviitteet kunnossa. Jokaisen raportin tulee viitata ainakin kurssiin / tehtäväsivuun. Kaikkiin muihinkin käytettyihin lähteisiin tulee viitata, kuten kurssikavereiden raportteihin, weppisivuihin, man-sivuihin... (Tässä alatehtävässä ei tarvitse tehdä testejä koneella).


### Lähteet:

- https://terokarvinen.com/2023/eettinen-hakkerointi-2023/
- https://terokarvinen.com/2023/crack-file-password-with-john/
- https://terokarvinen.com/2022/cracking-passwords-with-hashcat/
