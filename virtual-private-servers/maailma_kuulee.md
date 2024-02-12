# Maailma kuulee

## Tekstitiivistelmät
## 1. Teoriasta käytäntöön pilvipalvelimen avulla (Lehto 2022)

### a) Pilvipalvelimen vuokraus ja asennus
- Palvelimia (droplet) voi vuokrata useasta eri yrityksestä, esimerkiksi DigitalOcean (vaatii rekisteröitymisen ja luottokortin)
- Kurssia varten palvelin tehdään Debian-kuvakkeen perusteella
- Datacenter kannattaa tiedonsiirron nopeuden vuoksi valita Euroopasta. Datakeskusalueen alla näkyy alueen datakeskusten määrä ja tilanne
- Autentikointimenetelmänä SSH on turvallisempi kuin salasana
- Koneen luomisen jälkeen sinulla on virtuaalikone ja IP-osoite!

### d) Palvelin suojaan palomuurilla

- Yhteys ostettuun virtuaalipalvelimeen otetaan terminaalin kautta kirjoittamalla ``` ssh *käyttäjänimi*@*IP-osoite ``` Käyttäjänimi on tässä tapauksessa käytännössä aina root.
- SSH-yhteyteen tarvitaan virtuaalipalvelimelle asetettua salasanaa
- Koneelle haetaan päivitykset normaalisti (```sudo apt-get update```) ja sen jälkeen asentaa palomuuri (``` sudo apt-get install ufw```)
- Palomuuriin tehdään reiät portteja varten, esimerkiksi portti 22: ```sudo ufw allow 22/tcp ``` ja palomuuri laitetaan päälle ``` sudo ufw enable ```
- Myöhemmin SSH-yhteyden saa muodostettua ```$ sudo systemctl start ssh``` 

### e) Kotisivut palvelimelle

- Palvelimelle luodaan käyttäjä ```sudo adduser *nimi* ```
- Käyttäjälle annetaan sudo-oikeudet (=pääsy ryhmään sudo) ja suljetaan root-tunnus:
``` sudo adduser *nimi* sudo ``` <- sudo adduser *nimi* *ryhmännimi*
```sudo usermod -lock root ```
- Koneelle asennetaan Apache 2 -palvelin (```sudo apt-get install apache2```)
- Koneella näkyy oletuksena Apache 2 -testisivu. Se korvataan kirjoittamalla tekstiä /var/www/html/index.html -tiedostoon
- Palvelimella ei ole valmiiksi moniakaan ohjelmia. Esim. micro pitää asentaa sinne erikseen (```sudo apt-get install micro```)
- Käyttäjän henkilökohtaiset sivut tulevat esim. käyttäjä/publicsites -kansioon (-> esim. index.html)


### f) Palvelimen ohjelmien päivitys
- Virtuaalikoneelle haetaan päivitykset normaalisti 
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```
Murtautumisyrityksiä voi tarkastella esimerkiksi komennolla ```sudo less /var/log/auth.log | grep 10540``` 


## 2. First Steps on a New Virtual Private Server (Karvinen 2012)

- Virtuaalipalvelimen oston jälkeen omalta koneelta kirjaudutaan virtuaalikoneelle roolissa "root" ja annetaan hyvä salasana. Tämän jälkeen:
	- pystytä palomuuri
	- tee palomuuriin tarvittavat reiät
	- tee "oikea" käyttäjä ja anna sille sudo- sekä admin-oikeudet
	- sulje root-käyttäjä
- Käytä aina, AINA, hyviä salasanoja. Älä käytä huonoja salasanoja edes "aluksi". Salasanojen lisäksi päivitä paketit aina, kun kirjaudut sisään.
- Virtuaalipalvelimen oston jälkeen voisi olla mukavaa esimerkiksi hankkia sille nimi. Tämän voi tehdä esim. NameCheapista.


## Virtuaalipalvelimen vuokraus käytännössä

Tunnilla harjoiteltiin yllä mainitun [DigitalOceanin](https://cloud.digitalocean.com/) käyttöä. Olin ns. koekaniini ja tein vuokrauksen jakamalla samalla omaa ruutua. Tästä haipakasta johtuen en tajunnut ottaa kuvakaappauksia. Törmäsin kuitenkin (samasta syystä) toiseen ongelmaan, jota käsittelen alla. Kyseessä siis

## Unohtunut salasana

Koska jaoin tunnilla ruutua, en ilmeisesti tarpeeksi keskittynyt omiin valintoihin. Näin ollen en päässyt sisälle omalla tunnuksella. Root-tunnukseen olisin muistanut salasanan, mutta koska lukittiin, en tiennyt miten palvelimelle pääsee.

Suullisen ohjeistuksen (Sammakkosuo 2024) myötä sain kuitenkin tämän tehtyä. Ensimmäiseksi tuli mennä oman dropletin hallintaan DigitalOceanissa. Sieltä valittiin Manage Droplets.
Ensiksi tilattiin rootille uusi salasana. 
![reset_root](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/reset_root.jpg)

Tämän jälkeen mentiin kohtaan Log in as root.
![droplet_recovery](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/droplet_recovery.jpg)

Tämän jälkeen menin konsolissa ensiksi vaihtamaan rootin salasanan. Sen jälkeen vaihdoin rootin ominaisuudessa salasanan aiemmin luomalleni käyttäjälle suvi.
![change_pswd](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/change_psw.jpg)

Sen jälkeen avasin konsolin. Huom: vastaavan virtuaalipalvelimen kohdalla myös powershell toimii suoraan (alla). Menin varmistamaan, että käyttäjällä suvi pääsee nyt sisään.
![ssh powershell](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/ssh_pwsh.jpg)

Varmistuksen jälkeen vaihdoin käyttäjän jälleen rootiksi ja menin muokkaamaan käyttäjän asetuksia (```nano /etc/ssh/sshd_config```) (tekstieditorina nano). Tiedosto avautui tekstitiedostona. Vaihdoin sieltä PermitRootLogin yes -> no. 
![permit login](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/permit_root.jpg)

Sitten kirjauduin root-käyttäjänä palvelimelta ulos. Tämän jälkeen rootilla ei enää päässyt sisään.

![root login denied](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/root_deny.jpg)

Sen jälkeen tein normaalit päivitykset ja lähdin miettimään mahdollista DNS-nimeä.



## Lähteet:

Karvinen Tero 2012: First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. Luettavissa https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/ Luettu 10.2.2024.

Lehto Susanna 2022: Teoriasta käytäntöön pilvipalvelimen avulla. Luettavissa https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/ Luettu 10.2.2024.

Sammakkosuo Petri (Linux admin @NeuralDSP) 2024. Suullinen ohjeistus 11.2.2024.