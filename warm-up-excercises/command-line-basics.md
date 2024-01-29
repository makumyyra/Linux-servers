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

Kone Dell Latitude 7280

Suoritin: Intel(R) Core(TM) i7-7600U CPU @ 2.80GHz   2.90 GHz

Asennettu RAM: 16,0 Gt 

Windowsin määritykset: Windows 10 Pro, versio 22H2

### Micro

Ensimmäisenä tehtävänä oli asentaa Micro-editori. Aloitin luomalla tiedostot-kansion. Siirryin kansioon ja yritin copy-pastella siirtää isäntäkoneelta asennustekstin virtuaalikoneelle. Tämä ei onnistunut, joten yritin seuraavaksi Input - Keyboard - Insert Host Key Kombo. Tämäkään ei onnistunut, ja virtuaalikoneeni meni tilaan, jossa se ei ottanut lainkaan tekstiä vastaan. 

![vikatila](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/vika270123.JPG)

Lähetin koneelle sammutussignaalin ja sen jälkeen käynnistin koneen uudelleen. Klo 13:20 kone käynnistyi uudelleen ja menin tekemääni tiedostot-kansioon. Tällä kertaa kirjoitin komennot käsin. Komento:
``` curl https://getmic.ro | bash ```

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
``` sudo apt-get update
sudo apt-get -y install figlet```

Figlet ei näkynyt kohdekansiossa komennolla ls. Suullinen tiedonanto (Petri Sammakkosuo): Figlet asentui ns. systeemisovellukseksi ja on asentunut johonkin systeemikansioon. 

13:47 asennettu NodeJS sivun [Tecmint](https://www.tecmint.com/wikipedia-commandline-tool/) Debian-ohjeiden mukaan.
Kirjoitin ensimmäisen komennon jälleen käsin:

```` sudo apt-get install nodejs ````

NodeJS asentui onnistuneesti.
Seuraava komento ei onnistunut:
```` sudo npm install wikit -g ````

Vikailmoitus klo 13:48:
![komentoa ei löytynyt](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/npm.JPG)

Asensin npm:n suullisen ohjeen (Petri Sammakkosuo) mukaan (sudo apt-get install npm). Tämän jälkeen yllä oleva komento toimi ja Wikit asentui.

Command line -ohjelmien piti ehkä olla tekstinkäsittelyä ym. varten. Halusin kuitenkin asentaa jotain, mitä voisi konkreettisesti "käyttää" terminaalista. Asensin nSnake-pelin [it's Foss](https://itsfoss.com/best-command-line-games-linux/) ohjeiden mukaan.

```` sudo apt install nsnake ````

Klo 14:08 olin asentanut ohjelmat 
- Figlet
- Wikit
- nSnake



Ensin käytin komentoa pwd. Olin äsken luomassani kansiossa /home/suvi/tiedostot. Menin komennolla cd .. kansioita ylöspäin.
Juurihakemistossa listasin sisältöä rekursiivisesti, joka oli huono idea. Listattavaa oli todella paljon. Keskeytin listaamisen komennolla "q"

[ls -R | less](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/list_r.JPG)

Juurikansion alakansiot/tiedostot listasin helposti komennolla ls.
[ls -R | less](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/list_root.JPG)






Lisäsin käyttäjän "kani":

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



e) The Friendly M. Näytä 2-3 kuvaavaa esimerkkiä grep-komennon käytöstä. Ohjeita löytyy 'man grep' ja tietysti verkosta.
f) Pipe. Näytä esimerkki putkista (pipes, "|").
g) Tukki. Aiheuta lokiin kaksi eri tapahtumaa: yksi esimerkki onnistuneesta ja yksi esimerkki epäonnistuneesta tai kielletystä toimenpiteestä. Analysoi rivit yksityiskohtaisesti.





Lokiin aiheutin virheen yllä luodun kani-käyttäjän avulla. Vaihdoin käyttäjäksi kani ja yritin käyttää sudo apt-get updatea. Sain virheen, joka raportoitiin adminille. Kani ei ole sudoers-tiedostossa.
![cat_hosts](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/kani-not-sudo.JPG)


### Lähteet:
Karvinen, Tero 2020. Command Line Basics Revisited. https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited Luettu 27.01.2024.
Sammakkosuo, Petri. Suullinen tiedonanto 27.01.2024, 29.01.2024.
