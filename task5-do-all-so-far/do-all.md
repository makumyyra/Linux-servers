# Kaikki tähän mennessä

## Fyysisen koneen speksit:

- Kone: Dell Latitude 7280
- Suoritin: Intel(R) Core(TM) i7-7600U CPU @ 2.80GHz 2.90 GHz
- Asennettu RAM: 16,0 Gt
- Windowsin määritykset: Windows 10 Pro, versio 22H2

## Virtuaalikone:

Tehtävä aloitettu 23.2.24 klo 17:32
Vahingossa ensin Debian-32 bit, jonka jälkeen suoraan uusi kone.

Valitut speksit lyhyesti, koska itse virtuaalikoneen luominen on käyty läpi jo aiemmilla tunneilla: 

- kieli englanti
- location finland
- locale EN-US UTF8
- näppäinkartta suomi
- ei domain-nimeä
- asetettu käyttäjät root ja suvi
- partition - guided (all files in one partition)
- network mirror - no **huom: tämä aiheutti ongelmia myöhemmin, ks. alla**
- grub (käytä olemassaolevaa; dev/sda)

Tehtävä lopetettu 23.2.24 klo 18:05

Tehtävää jatkettu 23.2.24 klo 23:08

Seuraavaksi asensin koneelle guest additionsit, jotta ruutua saa skaalattua ja kirjoituksessa voi käyttää tabulaattoria täydentämiseen.

### Guest Additions

Guest additions:
- Yläriviltä Devices -> Insert Guest Additions CD Image
- Komento: ```mount /dev/cdrom /mnt ```
- Komento (mene kys. kansioon): ```cd /mnt```
- Komento (aja CD): ```./VBoxLinuxAdditions.run```
Sen jälkeen käynnistä kone uudelleen ("reboot"), jolloin GA-CD poistuu levyasemasta.

![guest additions](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/guestadd.JPG)


### Ongelma: Asennus ilman mirroria

Asensin toisen virtuaalikoneen vahingossa ilman network mirroria. Näin ollen Linux ei päivitellyt paketteja netistä vaan yritti hakea medioita asennuslevyltä (jota ei tietenkään ollut.) Niinpä ohjelmia asentaessa törmäsin ongelmaan.

Komento: ```sudo apt-get update ``` ->

*Ign: 1 cdrom [- -] bookworm InRelease: Please use apt-cdrom to make this CD-ROM recognized by APT."* jne.

![apt error](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/error.JPG)

Ongelman pystyy korjaamaan käsin, kun muokkaa sources.listiä. 

Komento: ```sudo nano -w /etc/apt/sources.list```
Sources.listiin pitää lisätä seuraavat tiedot:

- ```deb http://deb.debian.org/debian bookworm main```
- ```deb-src http://deb.debian.org/debian bookworm main```

- ```deb http://deb.debian.org/debian-security/ bookworm-security main```
- ```deb-src http://deb.debian.org/debian-security/ bookworm-security main```

- ```deb http://deb.debian.org/debian bookworm-updates main```
- ```deb-src http://deb.debian.org/debian bookworm-updates main ```

Lisäksi pitää laittaa sources.listissä ollut tieto kommentiksi, jotta se ei enää ole käytössä.

Näiden jälkeen ajan onnistuneesti komennot 
- ```sudo apt-get update```
- ```sudo apt-get upgrade```
- ```sudo apt-get dist-upgrade```

Tehtävä lopetettu 23.2.24 klo 23:51

### Name based virtual host 

24.2.24 klo 12:02

En jatkanut eilen luodulla virtuaalikoneella eteenpäin vaan poistin sen. Loin uuden samoilla spekseillä, tällä kertaa kuitenkin käyttäen network mirroria (Finland, deb.debian.org).

Sen jälkeen tein päivitykset:
- ```sudo apt-get update```
- ```sudo apt-get upgrade```
- ```sudo apt-get dist-upgrade```

Sen jälkeen asensin apachen:
```sudo apt-get install apache2```


Tämän jälkeen toimin sivun [Name based virtual hosts](https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/) ohjeiden mukaan. 

#### Komennot lyhenneltynä versiona: 

```sudoedit /etc/apache2/sites-available/sivunnimi.com.conf```
Sisällöksi:
<VirtualHost *:80>
 ServerName pyora.example.com
 ServerAlias www.pyora.example.com
 DocumentRoot /home/käyttäjä/publicsites/sivunnimi.com
 <Directory /home/käyttäjä/publicsites/sivunnimi.com>
   Require all granted
 </Directory>
</VirtualHost>
```sudo a2ensite pyora.example.com```
```sudo systemctl restart apache2```
```mkdir -p /home/käyttäjä/publicsites/sivunnimi.com/```
```echo teksti > /home/käyttäjä/publicsites/sivunnimi.com/index.html```

```curl -H 'Host: sivunnimi.com' localhost```
```curl localhost```

```sudoedit /etc/hosts```
127.0.0.1 localhost
127.0.1.1 käyttäjä
127.0.0.1 sivunnimi.com
(Karvinen, 2018.)

Kotisivua ei päässyt lukemaan, joten lisäsin kansioon oikeuksia komennolla ```chmod ugo+x $HOME $HOME/publicsites/```. 

Tämän jälkeen sivu näkyi oikein.

### Mokia

Aluksi yritin vahingossa muokata /var/www/html/index.html:ää, mutta minulla ei ollut tarvittavia oikeuksia.

![file is not writable](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/notwritable.JPG)

Olin sudo-ryhmässä, mutta lisäsin nimeni vielä sudoers-tiedostoon. Komento: ```sudo visudo```, jonka jälkeen käyttäjä lisätään haluttuine oikeuksineen tiedoston loppuun.

![sudoers](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/sudoers.JPG)

Tämäkään ei antanut oikeuksia tiedostoon, joten katsoin oikeuksia tarkemmin ```ls -l``` -komennolla.

![ls -l](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/lsl.JPG)

Kaiken tämän jälkeen tajusin **avata tiedoston sudo-ominaisuudessa**. Tämä tietenkin toimi. Kirjoitin tiedostoon yksinkertaisen lauseen.

![example.com](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/examplecom.JPG)

Tämän jälkeen yritin uudelleenkäynnistää apachen. Sain kuitenkin virheilmoituksen.

![Apache reload error](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/apachereload.JPG)

Kokeilin paikantaa virhettä komennolla ```sudo apache2ctl configtest```. Tämä kertoi, että tiedostossa oli virheellistä tekstiä. Kyseinen teksti pitäisikin löytyä **käyttäjän oman kansion alta eikä tietenkään sites-enabledista**.

![Apache reload error](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/apachereload1.JPG)

Kun yllä oleva asia oli korjattu, menin katsomaan sivua selaimesta. **Huomautus itselle: tässä tulee tietysti käyttää virtuaalikoneen selainta eikä oman koneen selainta**

Lopetin tehtävän tekemisen 24.2.24 klo 13:28.

### SSH
25.2.24 klo 14:01
Jatkoin samalla virtuaalikoneella.
Otin ensiksi ssh-yhteyden DigitalOceanin koneeseeni:
```ssh suvi@omavirtuaalikone.com```

Siellä muutin käyttäjän rootiksi komennolla sudo su --> salasana, jonka jälkeen ei tarvitse kirjoittaa sudoa joka kerta. Tämän huomaa myös ruutumerkistä "sijainti"tilassa:
![no 'sudo'](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/nosudo.JPG)
**huom: root on käytössä normaalisti, mutta sillä ei voi sisäänkirjautua, eli aiemmin tehty 'sudo usermod --lock root'**

Oman nettisivun (suvisammakko.com) lokien lukeminen:
```[tarvittaessa: sudo] journalctl --since yesterday|grep -i ssh |tail -5```

Huom. esim "failed password for root", eli joku oli yrittänyt murtautua sisään koneelle root-tunnuksella.
![ssh logs](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/journalctl.JPG)

Dig- ja host-komentoja varten piti ensin asentaa bind9-dnsutils ja bind9-host:
```sudo apt-get install bind9-dnsutils bind9-host```

Dig-komennon tulokset:
![dig]https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/dig.JPG)

Host-komento (sivuston IP-osoite ja vaihtoehtoisia saapuvan postin palvelimia. Jos yksi on kiireinen, tehtävä siirtyy toiselle.)
![host](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/host.JPG)

Loin ssh-avaimen komennolla ```ssh-keygen```. Sen jälkeen liitin sen DigitalOcean-sivulleni komennolla ```ssh-copy-id suvi@suvisammakko.com```.
![copy id](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/copyidssh.JPG)

Tämän jälkeen voi kirjautua ssh-yhteydellä passphrasella. Oma julkinen ssh-avain tallentuu esim. DigitalOceanin koneelle .ssh-tiedostoon (.-alku = salattu tiedosto).

### CertBot

Lisäksi asensin DigitalOceanin nettisivulle certbot-sertifikaatin.
```apt-get install certbot```
```apt-get install python3-certbot-apache```
```certbot --apache --agree-tos --redirect --hsts --staple-ocsp --email esim@esimerkki.com -d omadomain.com ```

![certificate registered](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/certreg.JPG)

### Misc

Harjoittelin myös ryhmäoikeuksien antamista kansioon. Aiemmin käyttäjä 'mikko' on lisätty puput-ryhmään (```sudo adduser mikko puput```). Tässä puput-ryhmä saa käyttöoikeuden kansioon (mikko.example.com), jossa sijaitsee peruskäyttäjien muokattava nettisivu (index.html).

![chown](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/chmod_chown.JPG)


### Lähteet

Karvinen Tero 2018. Name Based Virtual Hosts. Luettavissa: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/ Luettu 24., 25.2.24.


Sammakkosuo Petri. Suullinen tiedonanto 24., 25., 26.2.24.





