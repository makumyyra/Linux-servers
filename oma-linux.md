
# Virtuaalikoneen luominen

Loin onnistuneesti virtuaalikoneen VirtualBoxilla. Annoin luotavalle koneelle tarvittavat tiedot Create Virtual Machine -expert modessa.

Valinnat:
- ISO image: debian-live-12.4.0-amd64-xfce.iso
- Type: Linux
- Version: Debian (64-bit)
- Skip Unattended Installation --> yes
- Base Memory: 4096 MB
- Processors: 1
- Size: 30 GB
- Hard disk file type and variant: VDI
- Pre-allocate: yes
--> Finish

Installerin puolella annoin seuraavat valinnat:

- kieli, sijainti: suomi
- näppäinkartta: suomalainen
- asetettu käyttäjänimet ja salasanat (järjestelmän pääkäyttäjä)
- tee levyosiot: ohjattu - käytä koko levyä
- vain yksi levyosio
--> Lopeta osioiden teko ja tallenna muutokset levylle: kyllä

Tämän jälkeen kysymykset
- käytetäänkö asennuspalvelujen kopiota: kyllä
- asennuspalvelujen maa: suomi
- asennuspalvelimen kopio: ftp.fi.debian.org
- välipalvelin: ei
- grub:kyllä
--> dev/Sda

Näiden jälkeen asennus oli valmis. Virtuaalikone käynnistyi itsestään uudelleen.
(Sammakkosuo, Petri 17.1.2024. Suullinen tiedonanto.)


# Artikkelitiivistelmät


## Artikkeli 1: Raportointi
[Karvinen, Tero: Raportin kirjoittaminen](https://terokarvinen.com/2006/raportin-kirjoittaminen-4/)

- raportti kirjoitetaan sekä itselle muistin tueksi että toisille dokumentiksi

- kuvaa tarkasti se, millä teit ja miten teit: 
laitteisto, ympäristö ja kellonaika sekä mitä painoit, mitä tapahtui

- kerro, miten testasit toimenpiteen onnistumisen/epäonnistumisen

- raportoi tarkasti odottamattomat tulokset

- kirjoita huolellisesti ja selkeästi, otsikoi

- käytä viittauksia, mainitse lähteet

Artikkeli luettu 17.01.2024.


## Artikkeli 2: Vapaa ohjelmisto
[GNU Operating System: What is Free Software?](https://www.gnu.org/philosophy/free-sw.html)

- vapaa ohjelmisto, free software, antaa käyttäjälle mahdollisuuden muokata, käyttää ja jakaa ohjelmistoa vapaasti

- vapaan ohjelmiston määrittelyssä on neljä keskeistä tekijää:
    - vapaus käyttää ohjelmaa haluamallaan tavalla mihin tahansa tarkoitukseen
    - vapaus tutkia ohjelman toimintaa ja muokata sitä omiin tarkoituksiin (+ pääsy lähdekoodiin)
    - vapaus jakaa kopioita ohjelmasta
    - vapaus jakaa kopioita omasta muokatusta ohjelmasta muiden hyödyksi (+ pääsy lähdekoodiin)


- vapaa ohjelmisto voi olla kaupallista

- kehitys voi johtaa maksulliseen palveluun (esim. tekninen tuki)

- artikkelissa kerrotaan tarkkaan, miten vapaa ohjelmisto "pysyy" vapaana

- copyleft tarkoittaa, että vapaasta ohjelmistosta kehitettyjen versioiden kuuluu myös olla vapaita käyttöön, levitykseen ja muokkaukseen

Artikkeli luettu 17.01.2024.
