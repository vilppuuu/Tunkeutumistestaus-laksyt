## h0 Alkutesti

## a) Sieppaa ja analysoi verkkoliikennettä.

*Raportin saa julkaista heti ja sitä mukaa, kun se valmistuu. Muista viitata tämän kurssin tehtäväsivuun, ohjekirjoihin, raportteihin ja kaikkiin muihinkin käyttämiisi lähteisiin. Analyysi on tärkeä osa työtä. Voit käyttää haluamiasi työkaluja. Seuraa ajan kulkua, jotta ehdit palauttaa raportin ja rinnakkaisarvioida kaksi työtä. Yksilötyö. Kaikkia julkisessa Internetissä olevia lähteitä saa käyttää. Keskustelu ja yhteistyö on alkutestissä kielletty. Tehtäviä saa aloittaa vasta, kun on hyväksynyt kurssin säännöt Moodlessa.
Kun olet palauttanut työn Laksuun, rinnakkaisarvioi kaksi työtä.*

- Eli liikenteen kaappaamiseen valitsin tcpmdumpin, koska sitä olen joskus aiemmin myös testaillut ja muistaakseni oli suhteellisen helppokäyttöinen. Ainakin asennus ja pakettien tallennuksen aloittaminen oli helppoa, kuten muistelinkin eli terminaaliin *sudo apt install tcpdump* & *sudo tcpdump --interface any*, niin saadaan ainakin liikennettä näkyviin.
- Tällä tyylillä liikennettä meinaa tarttua vähän turhan paljon haaviin, kun käytännössä blancolla Linux-asennuksellakin ruutuun pamahtaa joku 100 riviä alle minuutissa, kun avaa selaimella jonkun verkkosivun.
- Liikennettä saa tcpdumpilla filtteröityä monellakin tapaa, mutta itse nyt yksinkertaisuuden vuoksi halusin tarkastella https-liikennettä, jonka sais siis rajattua lisäämällä aiemmin mainittuun komentoon parametriksi *-number port 443*.
- Jos alla olevan kuvan liikennettä tarkastellaan, niin ensimmäiseksi riviltä löytyy timestamp, verkkointerface, liikenteen tyyppi (sisään/ulos), ip-osoitteet ja käytetty portti, lopussa noita tcp-settejä syn/ack jne.
  
![Screenshot 2023-10-23 120209](https://github.com/vilppuuu/tunkeutumistestaus/assets/103587907/bc6f2a4d-2ef5-480a-a31f-e21f251c56bb)
