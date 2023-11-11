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

#### d) Etsi Metasploitable porttiskannaamalla (db_nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable. Katso, ettei skannauspaketteja vuoda Internetiin - kannattaa irrottaa koneet netistä skannatessa.

<p>Alkuun täytyy käynnistää tuo Metasploit, mikä tapahtuu ensiksi käynnistämällä sen tarvitsema tietokanta: postgresql, jonka jälkeen käynnistetään tuoa msfdb, ja käynnistetään msf-konsoli, josta porttiskannauskomento voidaan ajaa. Suorittamalla tuo -sn, eli ping sweep -skannaus tässä meidän hostiverkossa (192.168.56.0/24) löydetään sieltä tosiaan tuo Metasploitable osoitteesta, joka loppuu .103, joka voidaan siis vielä testata selaimella. Noista kahdesta muusta osoitteesta ei oikeastaan tarvitse välittää, sillä tuo .1 on tietty DGW, ja tuo .100 on VirtualBoxin tähän verkkoon luoma DHCP (löytyy VB:n Network Managerista, Host-only Networks kohdan alta). </p>

![msfdb](https://i.imgur.com/v8b6Upq.png)

![metass](https://i.imgur.com/K7M9b2p.png)


#### e) Porttiskannaa Metasploitable huolellisesti (db_nmap -A -p0-). Analysoi tulos. Kerro myös ammatillinen mielipiteesi (uusi, vanha, tavallinen, erikoinen), jos jokin herättää ajatuksia.

<p>Avoimia portteja, ja niissä olevia palveluita löytyi melkoinen määrä, mikä on melko epätavallista, kun otetaan huomioon että skannasimme vain yhden IP:n. Kun palveluita ja niiden versioita hieman tutkii, niin huomataan, että useat niistä ovat vanhoja ja haavoittuvia versioita esim. OpenSSH 4.7p1 & Apache 2.2.8 ovat tällaisiä. Myös jo ihan pelkistä palveluista löytyy mielenkiintoa herättäviä tapauksia esim. porteissa 512, 513, 514 on auki palvelut nimillä exec, login, ja shell, mikä kuulostaa kohtalaisen lupaavalta. Tai portissa 1542 nmap olevan bindhsellin nmap kertoo suoraan olevan 'Metasploitable root shell'. Sitten tuolta löytyy myös noita sertejä, jotka on salattu 1024 bittisellä sha1WithRSAEncryption algoritmillä, mikä ei nyt nopean googlen perusteella ole mikään turvallisin vaihtoehto, ja myöskin nykyään noiden pituudeksi taidetaan suositellaan minimissään 2048 bittiä. </p>

![nmapA](https://i.imgur.com/9MuTZH7.png)

#### f) Murtaudu Metasploitablen VsFtpd-palveluun Metasploitilla (search vsftpd, use 0, set RHOSTS - varmista osoite huolella, exploit, id)


#### g) Parempi sessio. Tee vsftpd-hyökkäyksestä saadusta sessiosta parempi. (Voit esimerkiksi päivittää sen meterpreter-sessioksi, laittaa tty:n toimimaan tai tehdä uuden käyttäjän ja ottaa yhteyden jollain tavallisella protokollalla)


#### h) Etsi, tutki ja kuvaile jokin hyökkäys ExploitDB:sta. (Tässä harjoitustehtävässä pitää hakea ja kuvailla hyökkäys, itse hyökkääminen jää vapaaehtoiseksi lisätehtäväksi)


#### i) Etsi, tutki ja kuvaile hyökkäys 'searchsploit' -komennolla. Muista päivittää. (Tässä harjoitustehtävässä pitää hakea ja kuvailla hyökkäys, itse hyökkääminen jää vapaaehtoiseksi lisätehtäväksi. Valitse eri hyökkäys kuin edellisessä kohdassa.)


#### j) Kokeile vapaavalintaista haavoittuvuusskanneria johonkin Metasploitablen palveluun. (Esim. nikto, wpscan, openvas, nessus, nucleus tai joku muu)


#### k) Kokeile jotain itsellesi uutta työkalua, joka mainittiin x-kohdan läpikävelyohjeessa.
