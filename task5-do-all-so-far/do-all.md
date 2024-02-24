# Kaikki tähän mennessä

Käytetyn koneen speksit:

- Kone: Dell Latitude 7280
- Suoritin: Intel(R) Core(TM) i7-7600U CPU @ 2.80GHz 2.90 GHz
- Asennettu RAM: 16,0 Gt
- Windowsin määritykset: Windows 10 Pro, versio 22H2


Tehtävä aloitettu 23.2.24 klo 17:32
Vahingossa ensin Debian-32 bit, 
Kieli, location finland, locale EN-US UTF8, näppäinkartta suomi

ei domain-nimeä
root
suvi

partition - guided (all files in one partition)
network mirror - no
grub (käytä olemassaolevaa; dev/sda)


Tehtävä lopetettu 23.2.24 klo 18:05


Tehtävä aloitettu 23.2.24 klo 23:08

Guest additions:
Yläriviltä Devices -> Insert Guest Additions CD Image
Komento: mount /dev/cdrom /mnt
Komento (mene kys. kansioon): cd /mnt
Komento (aja CD): ./VBoxLinuxAdditions.run
Sen jälkeen käynnistä kone uudelleen ("reboot"), jolloin GA-CD poistuu levyasemasta.

![guest additions](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/guestadd.JPG)

Ohjelmia asentaessa törmään ongelmaan. Linux yrittää hakea medioita asennuslevyltä (jota ei ole). 

Komento: sudo apt-get update ->
"Ign: 1 cdrom [- -] bookworm InRelease: Please use apt-cdrom to make this CD-ROM recognized by APT." jne.

![apt error](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/error.JPG)

Komento: sudo nano -w /etc/apt/sources.list
Sources.listiin pitää lisätä seuraavat tiedot:

deb http://deb.debian.org/debian bookworm main
deb-src http://deb.debian.org/debian bookworm main

deb http://deb.debian.org/debian-security/ bookworm-security main
deb-src http://deb.debian.org/debian-security/ bookworm-security main

deb http://deb.debian.org/debian bookworm-updates main
deb-src http://deb.debian.org/debian bookworm-updates main

Lisäksi pitää laittaa sources.listissä ollut tieto kommentiksi, jotta se ei enää ole käytössä.

Näiden jälkeen ajan komennot 
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade

Tehtävä lopetettu 23.2.24 klo 23:51

24.2.24 klo 12:02

Tämän jälkeen toimin sivun [Name based virtual hosts](https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/) ohjeiden mukaan.

Minulla ei ollut oikeuksia muokata /var/www/html/index.html:ää. 

![file is not writable](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/notwritable.JPG)

Olin sudo-ryhmässä, mutta lisäsin nimeni vielä sudoers-tiedostoon. Komento: sudo visudo, jonka jälkeen käyttäjä lisätään haluttuine oikeuksineen tiedoston loppuun.

![sudoers](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/sudoers.JPG)

Tämäkään ei antanut oikeuksia tiedostoon, joten katsoin oikeuksia tarkemmin ```ls -l``` -komennolla.

![ls -l](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/lsl.JPG)

Kaiken tämän jälkeen tajusin *avata tiedoston sudo-ominaisuudessa*. Tämä tietenkin toimi.

![example.com](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/examplecom.JPG)

Tämän jälkeen yritin uudelleenkäynnistää apachen. Sain kuitenkin virheilmoituksen.

![Apache reload error](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/apachereload.JPG)

Kokeilin paikantaa virhettä komennolla ```sudo apache2ctl configtest```. Tämä kertoi, että tiedostossa oli virheellistä tekstiä. Kyseinen teksti pitäisikin löytyä käyttäjän oman kansion alta eikä sites-enabledista.

![Apache reload error](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/apachereload1.JPG)


Sivustoa en saanut näkymään millään keinoin. Yritin usealla tavalla antaa oikeuksia käyttäjän nettisivukansioon. Käytin esimerkiksi komentoa ```chmod ugo+x $HOME $HOME/public_html/```. 

```chmod``` -komento tuntui viimein toimivan.
![chmod](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini5/chmod.JPG)

Komennolla ```curl -H 'Host: reepen.example.com``` tulostuu sivun oikea sisältö. Jostain syystä en silti saa sitä auki netissä.

Lopetin tehtävän tekemisen 24.2.24 klo 13:28.