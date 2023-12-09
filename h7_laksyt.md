## h7 Hash

- Tehtävät suoritettu käyttäen seuraavaa kokoonpanoa:
    - Hardware: AMD Ryzen 5 5600X, Nvidia GeForce RTX 3070, 32 GB RAM
    - Host OS: Windows 11 Home 22H2
    - Virtualization Software: Oracle VM VirtualBox Version 7.0.12 r159484 (Qt5.15.2)
    - VMs Used: Debian 12, Kali Linux 2023.3

### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

**Karvinen 2022: Cracking Passwords with Hashcat**

- Ártikkelin alussa asennellaan aptista ihan normaalisti hashcat & hashid, sekä ladataan tuolta SecLististä salasanalista.
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
- Alkuun tuon ylhäällä olevan [Teron artikkelin](https://terokarvinen.com/2023/crack-file-password-with-john/l) mukaisesti aloin asentelemaan noita tarvittavia riippuvuuksia, ja koska copy/paste VM:lle ei toiminut asentelin manuaalisesti vain nuo välttämättömät riippuvuudet eli: build-essential, libssl-dev, ja zlib1g zlib1g-dev zlib1g-gst (git, wget & bash-completion olivat muutenkin jo asennettuina).
- Seuraavaksi saman ohjeen mukaisesti kloonasin tuon 'johnin' gitillä, ja siellä suuntasin john/src- hakemistoon, jossa ajoin tuon ./configure-komennon. Tämä itseasiassa antoi pari valitusta puuttuvista riippuvuuksista, eli 7z ei tue BZIP2:sta koska unohdin asentaa libbz2 tuossa aiemmin, ja myös useampi rivi valitusta liittyen pkcs12-tiedostoihin. Tuo configure kuitenkin näytti menevän läpi ihan ok, joten päätin olla välittämättä noista virheistä. Lähinnä siksi, että en ajatellut tehdä nyt mitään pkcs12 liittyvää ja oletin että tuo 7z virhe ei myöskään vaikuta pahemmin mihinkään. Muistaakseni noihin pkcs12-tiedostoihin voidaan tallentaa sertifikaatteja ja avaimia samaan tiedostoon (joskus piti töissä purkaa tuollainen OpenSSL:llä, että sai sertifikaatin sieltä kaivettua).
- Eli ajoin tuon make-komennon, ja se meni läpi ilman valituksia, ja tuo john:kin toimii.

  ![asfag235](https://i.imgur.com/eBYxn3D.png)

  ![asdasdad](https://i.imgur.com/ymmDAbJ.png)

- Testasin ensiksi toimivuutta luomallani salasanasuojatulla zipillä. Ilmeisestikin tuo johnin tässä käytetty toimintaperiaate on sellainen, että ensiksi valitusta tiedostosta/tiedostotyypistä kaivetaan tiiviste, joka tallennetaan tiedostoon, ja jota vastaan sitten suoritetaan sanakirjahyökkäys, ja sehän näköjään toimii.

  ![asdsa22](https://i.imgur.com/ZELGRNq.png)

- Huvikseni ajattelin vielä testata jotain muutakin tiedostotyyppiä, ja tuolta listalta pisti silmään pdf, jota on ainakin helppo testata ja yleisyytensä vuoksi myös ihan mielenkiintoinen tapaus. Eli loin salasanasuojatun pdf-tiedoston, ja nopeasti googletin miten sen murtaminen johnilla toimii, ja siihen löytyikin [hyvät vinkit](https://security.stackexchange.com/questions/82222/how-can-i-extract-the-hash-inside-an-encrypted-pdf-file).
- Eli tarvisee ajaa tuo pdf2john.pl -skripti, joka tosiaan on perlillä kirjoitettu, joten se täytynee olla asennettu, mutta ainakin Debianissa löytyi oletuksena. Tuolla tosiaan saatiin tuon pdf:n tiiviste selville, jota voidaan sitten koittaa murtaa vapaasti valitulla tavalla.

  ![as561das](https://i.imgur.com/2Batkrk.png)

- Kokeillaanpa huvikseen saako tätä tiivistettä bruteforcattua helposti hashcatilla, eli etsitään tuolle pdf:lle oikea mode, ja laitetaan hashcat rullaamaan.

  `./hashcat -a 3 -m 10500 -o pdf.txt '$pdf$4*4*128*-1060*1*16*4a913eeaf18b9247a5110d5cd2a15b8d*32*a784cd4fd8096166c276442aee9749c600000000000000000000000000000000*32*cc8d78d9ffc684f83b46aa836a2e6894cff794c50c3832b21aba41785b361214'
hashcat (v6.2.6) starting`

- Tässä vierähti jo hieman enemmän aikaa, vaikka tuo salasana oli vain seitsemän merkkiä, ja ehkä huomionarvoista on kun tuota tulosteen **Progress** -kohtaa tarkastelee, niin huomataan että vajaassa kahdeksassa minuutissa tälläinen keskiluokan näyttis kerkesi käydä reilu 19 miljardia vaihtoehtoa läpi.

    ![juuuuuuh](https://i.imgur.com/Q01zq6X.png)

### c) f5bc7fcc7f5b3b6af7ff79e0feafad6d1a948b6a2c18de414993c1226be48c1f on erään tällä tehtäväsivulla olevan yksittäisen sanan tiiviste. Käytin hyvin yleistä ja tunnettua tiivistealgoritmia. Sanassa voi olla isoja kirjaimia, mutta ei erikoismerkkejä. Minkä sanan tiiviste on kyseessä?

- Hashid:llä kun tsekkaa tuon tiivisteen, niin antaa alla olevan listan algoritmejä. Jos kerran kyseessä on yleisesti käytetty tiivistealgoritmi, niin voisi veikata sen olevan tuo **SHA-256**, koska se on noista itselle ainoa vähänkään tutulta kuulostava, ja eipä tuolla listallakaan ole kuin kahdelle muulle algoritmille edes tuota hashcat-modea.

    ![hasdasdas](https://i.imgur.com/scIt4R3.png)

- Seuraavaksi varmaankin tarvitaan sanalista sivun sanoista, sillä jos sanan pituus alkaa olla siinä kymmenen merkin tienoilla muuttuu brute forcaaminen suht aikaa vieväksi, tai sitten täytyisi hankkia lisää computea. Tähän tarkoitukseen ehkä helppkäyttöisin työkalu on tuo [Cewl](https://github.com/digininja/CeWL), joka siis Github-kuvauksen mukaan on: *CeWL is a ruby app which spiders a given URL to a specified depth, optionally following external links, and returns a list of words which can then be used for password crackers such as John the Ripper.*
- Cewlin asennus ainakin Debianille onnistui suoraan aptista, ja sen käyttöönkään ei tarvinnut sen suuremmin perehtyä, muuten kuin nopeasti lukaisemalla helpin (*cewl -h*). Komennolla *cewl -d 0 -m 5 -with-numbers <kohde-url> -w <output.txt>* tuo siis kerää annetusta osoitteesta kaikki sanat, jotka ovat yli viisi merkkiä pitkiä (**m -5**), ja tuolla **-d 0** määritetään syvyys, eli tässä tapauksessa ei seurata sivun linkkejä ja kerätä niistä tietoja, **with-numbers** ottaa mukaan merkkijonot, joissa myös numeroita seassa (otin tässä varmuudeksi mukaan, kun numeroista ei ollut mainintaa tehtävänannossa), ja **-w** kirjoittaa outputin tiedostoon.

    ![asdasdasdasdasdasdasd](https://i.imgur.com/hZEPTIh.png)

- Seuraavaksi voidaankin hashcatilla ajaa tuo luomamme lista tuota tiivistettä vastaan, eli **-a 0** määrittää hyökkäyksen tyypiksi sanakirjahyökkäyksen **-m 1400** algoritmiksi tuon SHA2-256, ja komennon lopussa annetaan käytettävä sanakirja. 
`.\hashcat -a 0 -m 1400 -o asddas.txt 'f5bc7fcc7f5b3b6af7ff79e0feafad6d1a948b6a2c18de414993c1226be48c1f' lista.txt`

    ![asdasdasdaasd124](https://i.imgur.com/Clx5qaO.png)

### d) Cheatsheet. Kerää kurssilaisten raporteista käteviä tekniikoita. Kerää itse tekniikat ja komennot, älä pelkästään kuvaile. Muista lähdeviitteet. Tee tiivis ja selkeä cheatsheet, josta löydät tarvittavat tiedot lipunryöstössä. (Tässä alatehtävässä ei tarvitse tehdä testejä koneella)

- [Metasploitin](https://github.com/jjenska/PenTest/blob/main/h3/h3.md#f-murtaudu-metasploitablen-vsftpd-palveluun-metasploitilla-search-vsftpd-use-0-set-rhosts---varmista-osoite-huolella-exploit-id) ja [FUFF:in](https://github.com/jjenska/PenTest/blob/main/h2/h2.md#b-asenna-ffufme-harjoitusmaali-paikallisesti-omalle-koneellesi-ratkaise-teht%C3%A4v%C3%A4t-paitsi-content-discovery---pipes) käyttöä à la Jenni.
- Janalta vähän [ZAP:ia](https://github.com/JanaHalt/Ethical-Hacking-2023/blob/main/h4%20Totally%20Legit%20Sertificate.md#portswigger-labs), ja muutenkin hyvää oppia rauhallisesta, selkeästä, ja vaihe-vaiheelta tapahtuvasta ongelmanratkaisusta ja raportoinnista.
- 

### e) Viittaa. Tarkista, että jokaisessa raportissasi on lähdeviitteet kunnossa. Jokaisen raportin tulee viitata ainakin kurssiin / tehtäväsivuun. Kaikkiin muihinkin käytettyihin lähteisiin tulee viitata, kuten kurssikavereiden raportteihin, weppisivuihin, man-sivuihin... (Tässä alatehtävässä ei tarvitse tehdä testejä koneella).

- Lisätty h4:seen kokonaan puuttuneet lähteet, sekä yhdenmukaistettu muiden viikkojen lähdeviitteitä.

### f) Vapaaehtoinen: Kertaa. Harjoittele. Kannattaa kerrata tai täydentää eritoten h4 Totally Legit Sertificate.

- Täytyy katsoa, jos myöhemmin tänään (9.12) tai huomenna sunnuntaina kerkeää täydentää, etenkin tuota h4:sta, mutta palautetaan nyt ilman näitä (klo. 13:05).

### Lähteet:

- https://terokarvinen.com/2023/eettinen-hakkerointi-2023/
- https://terokarvinen.com/2023/crack-file-password-with-john/
- https://terokarvinen.com/2022/cracking-passwords-with-hashcat/
- https://nicholaslyz.com/blog/2021/07/23/cracking-pdf-hashes-with-hashcat/
- https://security.stackexchange.com/questions/82222/how-can-i-extract-the-hash-inside-an-encrypted-pdf-file
- https://github.com/digininja/CeWL
- https://github.com/jjenska/PenTest/blob/main/h3/h3.md
- https://github.com/JanaHalt/Ethical-Hacking-2023
