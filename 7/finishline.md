# Finish Line - Linux Basics 2024

Aluksi koneessani ei ollut Guest Additionsia asennettuna, joten komennot

> ```sudo mount /dev/cd /mnt```  
```cd /mnt```  
```sudo ./VBoxLinuxAdditions.run```  
```sudo reboot```


## a) Hei, maailma!

Tehtävä: Käännä "Hei, maailma!" haluamallasi kielellä.


## b) Uusi komento Linuxiin

Tehtävä: Laita Linuxiin uusi komento niin, että kaikki käyttäjät voivat ajaa sitä.

Aloitin asentamalla python3:n.

```sudo apt-get python3```

Sen jälkeen tarkistin, mihin python3 asentui:

```which python3```

Python3 näytti olevan sijainnissa /usr/bin/python3.

Tein omalla käyttäjätunnuksellani kansion scripts, jonne tein tiedoston hello.py. Ohjelman alkuun tuli shebang-tiedoksi yllä mainittu !#/usr/bin/python3

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/usrbin.JPG)


Ohjelma tulostaa käyttäjälle päivämäärän muodossa pp.kk.vvvv ja kellonajan tt:mm:ss. Shebangin lisäyksen jälkeen ohjelman voi ajaa joko muodossa 
> ```python3 hello.py``` tai  
```./hello.py```  

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/hellopy.JPG)

Kuvasta näkyy, että koodi toimii (ei ilmoitusta virheistä + haluttu tulostus). Tämän jälkeen siirsin koodin kansioon /usr/local/bin ja tarkistin pääsyoikeudet tiedostoon:

```sudo cp hello.py /usr/local/bin```
```ls -l /usr/local/bin/hello.py```

Käyttäjillä ei ollut toteutusoikeutta ko. tiedostoon = ei mahdollisuutta ajaa komentoa omilla käyttäjätunnuksillaan. Annoin toteutusoikeuden komennolla 
```sudo chmod u+x /usr/local/bin/hello.py```

Tämän jälkeen kuka tahansa pystyi kutsumaan ohjelmaa komennolla ```hello.py```

Tarkistin, että sama toimii sekä suvina että eri käyttäjällä (eijav):
Käyttäjänä Suvi:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/hellopy_asuser.JPG)

Käyttäjä Eija V.

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/hellopy_eijav.JPG)


# Laboratorioharjoitus 1

Aloitin labratehtävän luomalla uuden virtuaalikoneen. Valinnat lyhennettynä (kuvakaappauksia löytyy aiemmista raporteista):

> ISO image: debian-live-12.4.0-amd64-xfce.iso  
Type: Linux  
Version: Debian (64-bit)  
Skip Unattended Installation --> yes  
Base Memory: 4096 MB  
Processors: 1  
Size: 30 GB  
Hard disk file type and variant: VDI  
Pre-allocate: no  
--> Finish  

> Valinnat Installerin puolella:  
kieli: englanti  
sijainti: Suomi  
näppäinkartta: suomalainen  
levyosiot: käytä koko levyä  
vain yksi levyosio --> Lopeta osioiden teko ja tallenna muutokset levylle: kyllä  
käytetäänkö asennuspalvelujen kopiota: kyllä  
asennuspalvelujen maa: suomi  
asennuspalvelimen kopio: deb.debian.org  
välipalvelin: ei  
grub:kyllä (dev/Sda)

Ensin annoin itselleni roottina sudo-oikeudet: 
```sudo adduser suvi sudo```
```sudo adduser suvi adm```

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/sudorights.JPG)

Tämän jälkeen tein päivitykset:
```sudo apt-get update```
```sudo apt-get upgrade```

Sitten asensin palomuurin:
```sudo apt-get install ufw```
-->
```sudo ufw enable```

Tämän jälkeen tein tarvittavat reiät palomuuriin.
```sudo ufw allow http```
```sudo ufw allow https```
```sudo ufw allow ssh```

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/sudorights.JPG)

Sen jälkeen aloin tehdä tehtävää [laboratorioharjoitus](https://terokarvinen.com/2017/arvioitava-laboratorioharjoitus-linux-palvelimet-ict4tn021-5-torstai-alkusyksy-2017-5-op/). Ensimmäinen tehtävä oli:

> ## Weppipino ##  
Eija Vähäkari ryhtyy kehittämään weppipalveluita.  
Tee hänelle käyttäjä, ja tee hänen kotihakemistoonsa esimerkkisovellus valitsemallasi pinolla.  
Laita kaikki käyttäjätunnukset ja salasanat kotihakemistossasi olevaan lab.txt -tiedostoon, ja
suojaa se chmod:lla.  

Emme olleet käyneet läpi pinoja, joten loin pelkästään käyttäjän eijav. Annoin salasanan väärin, jonka johdosta eijav:llä ei ollut salasanaa. Jouduin siis vielä sudona asettamaan sen. Komentoina siis
```sudo adduser eijav```
```sudo passwd eijav```
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/passwdeijav.JPG)

Seuraava tehtävä oli:

> ## Etätyötä ##  
Haluamme työskennellä Hawajilta. Asenna mahdollisuus etäkäyttöön.

Sitten päästiin tekemään webbisivuja. Seuraava tehtävä kuului:

> ## TurvallinenSipuli.example.com ##  
Haluamme kotisivut osoitteeseen TurvallinenSipuli.example.com.  
Asenna meille sisällönhallintajärjestelmä, jolla työntekijämme voivat päivittää
kotisivuja.  
(Voit simuloida nimipalvelun toimintaa hosts-tiedoston avulla).  

Ensin asensin Apachen:
```sudo apt-get apache2```

Sen jälkeen tein konffitiedoston sijaintiin /etc/apache2/sites-available/turvallinensipuli.example.com.conf

Sinne sisällöksi kirjoitin
> <VirtualHost *:80>  
 ServerName turvallinensipuli.example.com  
 ServerAlias www.turvallinensipuli.example.com  
 DocumentRoot /home/eijav/publicsites/turvallinensipuli.example.com  
 <Directory /home/eijav/publicsites/turvallinensipuli.example.com>  
   Require all granted  
 </Directory>  
</VirtualHost>  

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/tursipconf.JPG)

Seuraavaksi loin tarvittavat käyttäjät (```sudo adduser nimi```):

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/users.JPG)

Kuten aina, käytin hyviä salasanoja.

Sen jälkeen tein kaikille käyttäjille kotisivun.

Käytin [TecAdmin.netin](https://tecadmin.net/setup-apache-userdir-on-ubuntu/) ohjeita.

Ensin asetin päälle userdir-moduulin ja uudelleenkäynnistin Apachen:
```sudo a2enmod userdir```
```sudo systemctl restart apache2```

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/userdir.JPG)

Sitten avasin userdir:in:

```sudo nano /etc/apache2/mods-available/userdir.conf```

Sieltä näkyi, että käyttäjien html-kansion nimen pitää olla public_html:
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/userdirconf/.JPG)




# Laboratorioharjoitus 2

Vaihdoin labraharjoituksesta [toiseen](https://terokarvinen.com/2017/arvioitava-laboratorioharjoitus-linux-palvelimet-ict4tn021-4-tiistai-alkusyksy-2017-5-op/), kun olin saanut ensimmäisestä tehtyä kaikki sopivat osat.

Tehtävänanto 1/4:

> ### Käyttäjät  
Työntekijämme ovat toimitusjohtaja Nakke Nertola, Håkan Värs, Einari Mikkonen, Einari Öljysaari ja Eija Vähäkäähkä. Tee kaikille käyttäjille esimerkkikotisivut.  
  
### Suuri muuri  
Suojaa kone tulimuurilla.  

Jatkoin saman koneen käyttämistä, jota käytin ensimmäisessä harjoituksessa. Siihen oli jo luotu kaikki tarvittavat käyttäjät ja palomuuri, joten näitä ei tarvinnut tehdä erikseen.

Tehtävänanto 2/4:

### WhoWhere
Tee kaikkien käyttäjien käyttöön komento, joka tulostaa Ninjamaisen tervehdyksen “Hello Ninja”, koneen IP-osoitteen ja komentoa ajavan käyttäjän nimen.

Koodi:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/hello_py.JPG)

Esimerkkiajo kolmella eri käyttäjällä:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/hello_ajo.JPG)

Tehtävänanto 3/4

> ### SneakyGarden.Example.com  
Virallinen ninjaliikesivumme tulee Eijan ylläpidettäväksi. Tee Eijalle valmis esimerkkisivu, jossa tietokannassa on seuraavat esimerkkiliikkeet vaikeustasoineen  
  
Hyppykiertopotku 27  
Kuperkeikka 3  
Karjaisu 1  
Sivun tulee olla Eijan muokattavissa ja ninjaliikkeiden näkyä osoitteessa http://SneakyGarden.Example.com. Nimipalvelun toimintaa voit simuloida hosts-tiedostolla.  


Olin tehnyt Eija Vähäkäähkälle käyttäjätunnuksen jo edellisessä laboratorioharjoituksessa. Käyttäjätunnus oli eijav.
Kävin rootin ominaisuudessa tekemässä eijav:lle public_html -kansion ja sinne index.html -tiedoston.

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/index.JPG)

Sen jälkeen kävin muokkaamassa hosts-tiedostoa, jotta http://sneakygarden.example.com ohjautuu osoitteeseen 127.0.0.1 (localhost):

```sudoedit /etc/hosts```

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/hosts.JPG)

Hostin määrittämisen jälkeen kävin luomassa sneakygardenin konfigurointitiedoston. Komennot:

>```sudoedit /etc/apache2/sites-available/sneakygarden.example.com.conf ```  
```sudo a2ensite sneakygarden.example.com```  
```sudo systemctl restart apache2```

Konfigurointitiedoston sisältö oli [Name Based Virtual Hosts](https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/) -sivulta löytyvän esimerkin ohjaamana:

> <VirtualHost *:80>  
 ServerName sneakygarden.example.com  
 ServerAlias www.sneakygarden.example.com  
 DocumentRoot /home/eijav/public_html/sneakygarden.example.com  
 <Directory /home/eijav/public_html/sneakygarden.example.com>  
   Require all granted  
 </Directory>  
</VirtualHost>  

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/newhost.JPG)

Seuraavaksi hain sivua curl-komennolla: 

```curl -H 'Host: sneakygarden.example.com' localhost```
Tuloksena oli forbidden:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/forbidden.JPG)

En jäänyt tähän kiinni, koska sen voi korjata myöhemmin.

Kävin katsomassa, että sneakygarden oli sekä sites-availablessa että sites-enabledissa:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/avenable.JPG)

Löytyi. Nyt yritin korjata 403 Forbidden -ongelman. Ensin kokeilin roottina ```sudo chmod ugo+x eijav eijav/public_html``` ja ```ls -ld eijav eijav/public_html/``` Sivu ei silti lähtenyt toimimaan.

Käyttäjänä eijav kokeilin komentoa ```chmod ugo+x $HOME $HOME/public_html``` ja sen jälkeen ```ls -ld $HOME $HOME/public_html/```. Nämäkään eivät saaneet nettisivua toimimaan. Virhe on kuitenkin jossain vaiheessa matkaa vaihtunut muotoon "Server not found".

Virheilmoituksista ei selvinnyt mitään ("Shutdown gracefully" ym). Menin tarkistamaan log levelin.
```sudoedit /etc/apache2/apache2.conf```

Log level näytti olevan warn. Vaihdoin sen tasolle info, jotta saan virheilmoituksista enemmän irti.

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/loglevel.JPG)

Tälläkään en kuitenkaan saanut mitään infoa siitä, miksi sivu ei toimi. Osoiterivillä localhost ja 127.0.0.1 kuitenkin toimivat ihan oikein.

Curl-kyselyllä sivun tuloksena tulee "Bad Request - Your browser sent a request that this server could not understand."

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/badcurl.JPG)


Lisäksi eijav/public_html/index.html ei ole käyttäjän eijav muokattavissa. En tiedä, miten asian saisi korjattua. 
TODO: Etsi joku, joka osaa auttaa. Selasin nettiä todella kauan saadakseni tämän toimimaan. Tarvitsen kuitenkin jonkun osaajan neuvomaan konkreettisesti.


# Tenttikone

Näiden jälkeen tein vielä kokeen tiistain tenttiä varten.

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/tenttikone/.JPG)

Valinnat:

> ISO image: debian-live-12.4.0-amd64-xfce.iso  
Type: Linux  
Version: Debian (64-bit)  
Skip Unattended Installation --> yes  
Base Memory: 4096 MB  
Processors: 1  
Size: 30 GB  
Hard disk file type and variant: VDI  
Pre-allocate: yes --> Finish  

Installerin puolella annoin seuraavat valinnat:

> kieli, sijainti: englanti, US  
näppäinkartta: suomalainen  
nimi: **tenttidebian  **
domain: ei  
asetettu käyttäjänimet ja salasanat (järjestelmän pääkäyttäjä)  
tee levyosiot: ohjattu - käytä koko levyä  
vain yksi levyosio --> Lopeta osioiden teko ja tallenna muutokset   levylle: kyllä  

Tämän jälkeen kysymykset
> käytetäänkö asennuspalvelujen kopiota: kyllä  
asennuspalvelujen maa: suomi  
asennuspalvelimen kopio: deb.debian.org  
välipalvelin: ei  
grub:kyllä --> dev/Sda  

Asensin koneelle dist-upgradet ja palomuurin:

```sudo apt-get dist-upgrade```
```sudo apt-get install ufw```
```sudo ufw enable```
```sudo ufw allow http```
```sudo ufw allow https```
```sudo ufw allow ssh```
```sudo ufw status```

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/finishline7/labra2/palomuuri/.JPG)

Tämän jälkeen lopetin tehtävät. En ottanut aikaa, mutta sitä meni arviolta noin 5-6 h. Sen lisäksi yritän vielä selvittää nettisivuongelman (eli miksi en saa luotua nettisivuja käyttäjille).