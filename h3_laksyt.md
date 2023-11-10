### h3 Lab kid

Irroita koneet Internetistä harjoittelun ajaksi. Ole huolellinen.

#### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
*€ Velu 2022: Mastering Kali Linux for Advanced Penetration Testing 4ed: Chapter 10 - Exploitation: kappaleet "The Metasploit Framework", "The exploitation of targets using Metasploit" ja "Using public exploits". Eli ei enää kappaletta "Developing a Windows exploit" ja siitä eteenpäin.*

*Vapaavalintainen läpikävely 0xdf tai ippsec (Kannattaa valita helppo; esim "Base Points: Easy")*

*Nyrkkeilysäkki ei kuulu. Etsi hakukoneesta kotitehtäväraportti, jossa Kali ja Metasploitable on asennettu samaan verkkooon VirtualBoxiin; sekä testaamalla osoitettu, että koneet on saatu irti Internetistä. Tällaiset tehtävät ovat olleet jonain vuonna esimerkiksi nimillä "Nyrkkeilysäkki" ja "Ei kuulu".*

#### a) Asenna Kali virtuaalikoneeseen

<p>Kerkesin tämän tehdä jo kurssin alussa, ja prosessi ei nyt muistaakseni ollut niin ihmeellinen, että sitä olisi tarpeen käydä uudestaan läpi askel askeleelta. Alkajaisiksi latasin tuon uusimman version Kalista Virtualboxille (https://www.kali.org/get-kali/#kali-virtual-machines), jonka jälkeen purin ladatun zipin, ja klikkasin kansiosta löytyvää Kali Linux Virtualbox Machine Definition -tiedostoa, minkä jälkeen Kali Linux ilmestyi VirtualBoxin näkymään.</p>

<p>Tästä sitten normaali käynnistys ja varmaan perus enterin hakkaamista, kunnes päästiin kirjautumisruutuun eli tarvittiin salasana ja käyttäjänimi, jotka jouduin googlettamaan, ja ne olivat niinkin ovelat kuin kali & kali (nämä olisivat olleet myös näkyvissä Machine Descriptionissa VirtualBoxissa). Muistaakseni perus update & upgraden lisäksi ainakin näppiksen layouttia piti vaihtaa, että käytöstä tuli yhtään mitään, ja taisin VirtualBoxin asetuksista myös käydä laittamassa lisää RAM:ia ja Video Memoryä.</p>



#### b) Asenna Metasploitable 2 virtuaalikoneeseen

<p>Tässäkään ei varsinaisesti ollut mitään ihmeellistä, eli aloitin lataamalla tuon Metasploitable 2 VM:n(https://sourceforge.net/projects/metasploitable/), ja purkamalla sen. Tämän jälkeen VirtualBoxista New machine -> Type: Linux, Version: Other Linux (64-bit) -> RAM:it + CPU:t -> Virtual Hard Diskiin valitetaan käytettävän olemassa olevaa tiedostoa, johon siis laitetaan tuolta aiemmin puretusta kansiosta tuo Metasploitable, jonka jälkeen next ja finish, ja kone voidaan käynnistää.</p>

#### c) Tee koneille virtuaaliverkko, jossa:

**Kali saa yhteyden Internettiin, mutta sen voi laittaa pois päältä**

**Kalin ja Metasploitablen välillä on host-only network, niin että porttiskannatessa ym. koneet on eristetty intenetistä, mutta ne saavat yhteyden toisiinsa**

**Osoita eri komennoilla, että Internet-yhteys katkeaa: 'ping 1.1.1.1', 'ping www.google.com', 'curl www.google.com'**
