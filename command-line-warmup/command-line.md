# Command Line Basics

## Karvinen, Tero, 2020: Command Line Basics Revisited - Summary
- The command line is a fast and effective way to operate
- In the command line, you will be moving around in directories 
- You can create and work with directories and files (e.g. create and manipulate files)
- You can connect to a remote machine in a remote command shell
- The command line logs your operations so you can easily repeat them
- Administrative commands require administrative rights (sudo = super user do)
 
(Karvinen, T. 2020)

## Hands-on homework  

28.01.2024, tehtävät aloitettu klo 13:10.  

- Kone: Dell Latitude 7280
- Suoritin: Intel(R) Core(TM) i7-7600U CPU @ 2.80GHz   2.90 GHz
- Asennettu RAM: 16,0 Gt 
- Windowsin määritykset: Windows 10 Pro, versio 22H2

### Micro

Ensimmäisenä tehtävänä oli asentaa Micro-editori. Aloitin luomalla tiedostot-kansion. Siirryin kansioon ja yritin copy-pastella siirtää isäntäkoneelta asennustekstin virtuaalikoneelle. Tämä ei onnistunut, joten yritin seuraavaksi Input - Keyboard - Insert Host Key Kombo. Tämäkään ei onnistunut, ja virtuaalikoneeni meni tilaan, jossa se ei ottanut lainkaan tekstiä vastaan. 

![vikatila](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/vika270123.JPG)

Lähetin koneelle sammutussignaalin ja sen jälkeen käynnistin koneen uudelleen. Klo 13:20 kone käynnistyi uudelleen ja menin tekemääni tiedostot-kansioon. Tällä kertaa kirjoitin komennot käsin. Komento: ``` curl https://getmic.ro | bash ```

Tällä kertaa Micro asentui oikein.

### Rauta

Asensin ensin lshw-ohjelmiston. Linuxin listaamat tiedot alla kuvassa:

![vikatila](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/lshw.JPG)

Järjestelmä on tietenkin VirtualBox, koska tietokone on luotu VirtualBoxin avulla. Muistiksi asetin virtuaalikonetta luodessani 4 GB, joka on kerrottu myös kuvassa. Prosessori on sama kuin oman fyysisen koneeni prosessori. Disk-kohdassa mainittu CD-ROM (jollaista asemaa läppäristäni ei löydy) on se, mihin virtuaalikoneessa "asetettiin" ISO-image. ISO-image toimii kuten järjestelmän asennus-CD, joten CD-ROM -asema on siksi tarpeellinen. Inputissa lukee mouse integration, joka lukee isäntäkoneen hiirenliikettä. Muita osioita en vielä osaa analysoida. 

### Apt (Debianin "Advanced Package Tool")

Tehtävässä piti asentaa kolme komentoriviohjelmaa. Asensin ohjelmat 
- Figlet
- Wikit
- Cowsay
- Fortune
- Oneko

Näistä ensimmäisenä asensin Figletin (fonttiohjelma) klo 13:39. Asensin sen sivun [installati](https://installati.one/install-figlet-debian-11/) ohjeiden mukaan. Komennot järjestyksessä 
``` sudo apt-get update ```; ``` sudo apt-get -y install figlet```

Figlet ei näkynyt kohdekansiossa komennolla "ls". Suullinen tiedonanto (Petri Sammakkosuo): Figlet asentui ns. systeemisovellukseksi ja on asentunut johonkin systeemikansioon. 

13:47 Aloitin asentamaan Wikitiä. Ensin asensin NodeJS:n sivun [Tecmint](https://www.tecmint.com/wikipedia-commandline-tool/) Debian-ohjeiden mukaan: ``` sudo apt-get install nodejs ```

NodeJS asentui onnistuneesti.
Seuraava komento ei kuitenkaan onnistunut:
``` sudo npm install wikit -g ```

Vikailmoitus klo 13:48:
![komentoa ei löytynyt](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/npm.JPG)

En tiennyt, että minulta puuttui npm. Asensin sen suullisen ohjeen (Petri Sammakkosuo) mukaan (sudo apt-get install npm). Tämän jälkeen yllä oleva komento toimi ja Wikit asentui.

Ohjelmien Cowsay, Fortune ja Oneko asentaminen sujui ongelmitta. Asensin kaikki yhtä aikaa:
![install-many](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/install_many.JPG)
![install-many](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/install_many2.JPG)

Fortune antoi jostain syystä lainauksia vain italiaksi. Halusin poistaa ohjelman ja tein sen käyttämällä sivun [cyberciti](https://www.cyberciti.biz/faq/delete-uninstall-software-linux-commands/) ohjeita. Ohjelma ei silti hävinnyt.

Command line -ohjelmat ovat periaatteessa tekstinkäsittelyä ym. varten. Halusin kuitenkin asentaa jotain, mitä voisi konkreettisesti "käyttää" terminaalista. Asensin nSnake-pelin [it's Foss](https://itsfoss.com/best-command-line-games-linux/) ohjeiden mukaan: ``` sudo apt install nsnake ```

Klo 14:08 olin asentanut ohjelmat 

## FHS - Filesystem Hierarchy Standard

Ensin käytin komentoa pwd. Olin äsken luomassani kansiossa /home/suvi/tiedostot. Menin komennolla cd .. kansioita ylöspäin. Juurihakemistossa listasin sisältöä rekursiivisesti antamatta muita parametrejä. Listattavaa oli todella paljon. Keskeytin listaamisen komennolla "q"

![ls -R | less](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/list_r.JPG)

Juurikansion alakansiot/tiedostot listasin helposti komennolla ls.
![ls -R | less](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/list_root.JPG)

Seuraavaksi lisäsin käyttäjän "kani":

![adduser_kani](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/adduser.JPG)

Tämän jälkeen home-kansiossa näkyi aakkosjärjestyksessä kaksi käyttäjää (kani, suvi).

![käyttäjät](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/home_kani_suvi.JPG)

Kotikansiossa listasin ls-komennolla alikansiot:

![home/suvi](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/suvihome_ls.JPG)

Etc-kansiossa listasin sisällön komennolla ls. Siellä luin kaksi lyhyttä tiedostoa komennolla "cat [tiedoston nimi]":

![cat_hosts](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/etc_read.JPG)

Media-kansiossa ei ollut mitään muuta kuin tyhjät cdrom-kansiot:

![cat_hosts](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/media.JPG)

Log-kansiossakaan ei ollut kovin paljon sisältöä. Luin sieltä README-tiedoston:

![cat_hosts](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/readme.JPG)

## Grep

Grep-komentoa varten loin lyhyen tekstitiedoston. Hain sieltä grepillä neljällä eri tavalla:
- sanan osalla (grep <teksti> <tiedosto>)
- ignore casella (grep -i <teksti> <tiedosto>)
- kaikki osumat (grep -o <teksti> <tiedosto>)
- rivinumeroilla ja ignore casella (grep -n -i <teksti> <tiedosto>)

Näistä grep -o palauttaa osumista vain haetun sanan, eli periaatteessa osumien määrän voisi laskea. En keksinyt, miksi haku olisi hyödyllinen. Muutoin pitkissä tekstitiedostoissa etenkin rivinumeron saaminen on erittäin hyvä ominaisuus (ne voi laittaa päälle myös itse tekstitiedostossa).

![cat_hosts](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/grep1.JPG)

## Pipe

Esimerkki putkista alla: grep | less. Less helpottaa pitkien tuloslistojen lukemista näyttämällä tulokset "sivu" kerrallaan. Less siis listaa tuloksia sen verran kuin näytölle mahtuu. Eteenpäin voi siirtyä välilyönnillä ja taaksepäin palata b-kirjaimella. Q-kirjain lopettaa selauksen.

![cat_hosts](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pipe.JPG)

## Tukki

Lokia pääsi Debianissa lukemaan komennolla ``` journalctl ``` (StackExchange, 2024). Viimeisimmät lokimerkinnät saa näkyviin komennolla ``` journalctl -f ```

Lokiin aiheutin virheen yllä luodun kani-käyttäjän avulla. Vaihdoin käyttäjäksi kani ja yritin käyttää sudo apt-get updatea. Sain virheen, joka raportoitiin adminille. Kani ei ole sudoers-tiedostossa.
![kani-sudo](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/kani_not_sudo.JPG)
![kani-sudo-fail](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/auth_fail.JPG)

Onnistunut toiminto puolestaan oli suvi-käyttäjän lisääminen adm-ryhmään. Ohje löytyi Debian Wikistä System Group -osion alta (Debian Wiki, 2024). Adm-ryhmäläiset näkevät järjestelmäviestejä lokissa.
![suvi-to-adm](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/admgroup.JPG)
![suvi-adm-log](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/adduser_log.JPG)



### Lähteet:
Karvinen, Tero 2020. Command Line Basics Revisited. https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited Luettu 27.01.2024.
Sammakkosuo, Petri. Suullinen tiedonanto 27.01.2024, 29.01.2024.
[Debian Wiki](https://wiki.debian.org/SystemGroups). Luettu 29.01.2024.
[StackExchange](https://unix.stackexchange.com/questions/422213/how-to-see-the-latest-x-lines-from-systemctl-service-log). Luettu 29.01.2024.
