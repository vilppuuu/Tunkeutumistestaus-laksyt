### h3 Lab kid

Irroita koneet Internetistä harjoittelun ajaksi. Ole huolellinen.

#### x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
*€ Velu 2022: Mastering Kali Linux for Advanced Penetration Testing 4ed: Chapter 10 - Exploitation: kappaleet "The Metasploit Framework", "The exploitation of targets using Metasploit" ja "Using public exploits". Eli ei enää kappaletta "Developing a Windows exploit" ja siitä eteenpäin.*

*Vapaavalintainen läpikävely 0xdf tai ippsec (Kannattaa valita helppo; esim "Base Points: Easy")*

*Nyrkkeilysäkki ei kuulu. Etsi hakukoneesta kotitehtäväraportti, jossa Kali ja Metasploitable on asennettu samaan verkkooon VirtualBoxiin; sekä testaamalla osoitettu, että koneet on saatu irti Internetistä. Tällaiset tehtävät ovat olleet jonain vuonna esimerkiksi nimillä "Nyrkkeilysäkki" ja "Ei kuulu".*

<p>Googlesta löytyi Sami Kulonpään ratkaisu Nyrkkeilysäkki -tehtävään syksyltä 2021 (https://kulonpaa.com/?p=87), jossa siis VirtualBoxin asetuksissa Metasploitable-koneelta otetaan tuosta default NAT-verkkoadapterista 'piuha irti', jolloin sillä ei enää ole pääsyä interwebsiin. Tämän jälkeen otetaan käyttöön toinen adapteri, joka on 'Host-only adapter', jolla siis voidaan simuloida lähiverkkoa, joista koneella ei ole pääsyä internettiin, mutta ne voivat viestiä keskenään. Kun sama temppu tehdään tuolle Kali koneelle tuloksena on juurikin edellä mainittu tilanne, ja probleema on solved. </p>

#### a) Asenna Kali virtuaalikoneeseen

<p>Kerkesin tämän tehdä jo kurssin alussa, ja prosessi ei nyt muistaakseni ollut niin ihmeellinen, että sitä olisi tarpeen käydä uudestaan läpi askel askeleelta. Alkajaisiksi latasin tuon uusimman version Kalista Virtualboxille (https://www.kali.org/get-kali/#kali-virtual-machines), jonka jälkeen purin ladatun zipin, ja klikkasin kansiosta löytyvää Kali Linux Virtualbox Machine Definition -tiedostoa, minkä jälkeen Kali Linux ilmestyi VirtualBoxin näkymään.</p>

<p>Tästä sitten normaali käynnistys ja varmaan perus enterin hakkaamista, kunnes päästiin kirjautumisruutuun eli tarvittiin salasana ja käyttäjänimi, jotka jouduin googlettamaan, ja ne olivat niinkin ovelat kuin kali & kali (nämä olisivat olleet myös näkyvissä Machine Descriptionissa VirtualBoxissa). Muistaakseni perus update & upgraden lisäksi ainakin näppiksen layouttia piti vaihtaa, että käytöstä tuli yhtään mitään, ja taisin VirtualBoxin asetuksista myös käydä laittamassa lisää RAM:ia ja Video Memoryä.</p>



#### b) Asenna Metasploitable 2 virtuaalikoneeseen

<p>Tässäkään ei varsinaisesti ollut mitään ihmeellistä, eli aloitin lataamalla tuon Metasploitable 2 VM:n(https://sourceforge.net/projects/metasploitable/), ja purkamalla sen. Tämän jälkeen VirtualBoxista New machine -> Type: Linux, Version: Other Linux (64-bit) -> RAM:it + CPU:t -> Virtual Hard Diskiin valitetaan käytettävän olemassa olevaa tiedostoa, johon siis laitetaan tuolta aiemmin puretusta kansiosta tuo Metasploitable, jonka jälkeen next ja finish, ja kone voidaan käynnistää.</p>

#### c) Tee koneille virtuaaliverkko, jossa:

**Kali saa yhteyden Internettiin, mutta sen voi laittaa pois päältä**

- Tuota aiempaa ohjetta mukaillen otin siis tuon Host-only adapterin käyttöön Kalille, ja tuosta defaultti NAT-adapterista piuhan irti, jolloin koneella ei siis pääsyä internettiin, mutta yhteys Metasploitable koneeseen onnistuu. Kun halutaan päästä takaisin internettiin tehdään päinvastoin, eli irroitetaan VirtualBoxin Network -asetuksista piuha Host-only -adapterista, ja kytketään piuha kiinni NAT-asetuksiin, jolloin internettiin yhdistäminen onnistuu.

  ![interwebz](https://i.imgur.com/zUbJfmy.png)

**Kalin ja Metasploitablen välillä on host-only network, niin että porttiskannatessa ym. koneet on eristetty intenetistä, mutta ne saavat yhteyden toisiinsa**

- Tässä siis laitetaan molemmissa koneissa tuo Host-only adapteri tulille, ja irroitetaan NAT-adapteri, jolloin ne ovat samassa verkossa, ja ilman pääsyä interwebziin. Kuten kuvista näkyy pingit eivät mene läpi muualle, kuin noihin 192.168 alkuisiin osoitteisiin, jotka siis kuuluvat yksityisiin ip-avaruuksiin (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) joita ei reititetä internettiin.
  
  ![hostadapter](https://i.imgur.com/RasDJxy.png)
  
  ![hostonly](https://i.imgur.com/yJEh8jK.png)
 

**Osoita eri komennoilla, että Internet-yhteys katkeaa: 'ping 1.1.1.1', 'ping www.google.com', 'curl www.google.com'**

![curlping](https://i.imgur.com/VvnRJkL.png)
