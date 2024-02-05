# Tiivistelmät

## 1. Name-based Virtual Host Support 

IP-pohjaiset virtuaalikoneet:
- yhdellä palvelimella voi ylläpitää useita sivustoja
- IP-osoitteita on rajallinen määrä, joten parempi tapa on käyttää nimipohjaista palvelua
- palvelinohjelma osaa tunnistaa, mitä nimeä tai osoitetta loppukäyttäjä kutsuu
- osoitteen/nimen perusteella palvelin palauttaa oikean nettisivun 
- sivustot listataan virtuaalikoneen konfiguroinneissa
- jos kutsulle ei löydy täsmällistä vastaavuutta, palvelin palauttaa sen oletuskonfiguraatiossa määritellyn sivuston. Käytännössä oletuksena on localhost (000-default.conf)

(The Apache Software Foundation 2023)


## 2. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address
https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/

- Apachen avulla voi ylläpitää useita domaineja yhden IP-osoitteen avulla
- Apachen asentamisen jälkeen lisätään uusi nimipohjainen verkkopalvelu (name virtualhost)
- varsinainen nettisivu tulee luoda peruskäyttäjän alle
- Testitapauksissa nettisivun voi laittaa toimimaan localhostin alla

(Tero Karvinen 2018)

# Web-palvelin, Name Based Virtual Host

Koneelle asennettiin 30.1.2024 tunnilla Apache2 ja testattiin, että localhost vastaa. Lähdin siis liikkeelle kohdasta h3 b. 

Ensin tarkastelin access logia (sudo tail -f /var/log/apache2/access.log). 'tail'-komento pitää loggausta käynnissä virtuaalikoneen ruudulla ja rekisteröi tapahtumia ylläpidetyillä sivuilla.

![access_log](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/accesslog.JPG)

Access logissa näkyy, kuinka sivuille on lähetetty palvelupyyntöjä (GET-requests), ja mistä selaimesta niitä on lähetetty. Osa sivuista on antanut vikaviestejä (404) eräästä stylesheetistä, johon HTML-koodilla ei ollut pääsyä (esim. kuvan alin rivi).

Error logissa (alla) näkyi koneen sammutusta, konfigurointia ja käynnistystä, koska mitään ihmeempiä ongelmia ei ollut ehtinyt ilmaantua. 

![error_log](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/errorlog.JPG)

Kävin tekemässä useammankin eri nettisivun. Kotitehtävän "hattu.example.com" löytyy hostseissa tällä hetkellä kahdessa eri muodossa. Sieltä löytyvät myös muut tekemäni sivut. Kaikki ohjautuvat samaan osoitteeseen (localhost; 127.0.0.1). Hosteja pääsi muokkaamaan komennolla "sudoedit /etc/hosts".

![hosts](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/hosts.JPG)

hattu.example.com -sivun HTML-koodissa sivun nimi löytyy head>titlesta sekä bodyn ylätunnisteesta (alla). HTML-koodi on tehty sivun html5example.comin mukaan.

![hattu_pagesource](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/hattu_pagesource.JPG)

Curl-komento (Client URL) mahdollistaa tiedonsiirron koneen ja sivuston välillä. Käytännössä curl pyytää tietoa nettisivulta, saa joitakin sivun perustietoja, ja tulostaa ne käyttäjälle.

![curl](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/curl.JPG)

Curl -l:stä en ottanut kuvakaappausta, mutta mikäli haettu sivu on "muuttanut", "curl -l" ohjautuu uuteen kohteeseen tekemään curl-komennon. 

Tiedostojen pääsyoikeuksia jouduin muokkaamaan komennolla "chmod ugo+x $HOME $HOME/public_html/', 'ls -ld $HOME $HOME/publicsites/hattu.example.com'.
![paasyoikeudet](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/oikeudet.jpg)
Sitten annoin vielä rekursiivisesti oikeudet käyttäjälle suvi ("-R"). Käyttäjä sai siis oikeudet komennossa mainittuun kansioon ja kaikkiin sen alakansioihin & tiedostoihin:

![paasyoikeudet](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/suvi_suvi.jpg)

Lisätehtävä: Kuten hosts-kuvasta (yllä) näkyy, kone vastaa tällä hetkellä useammastakin nimestä.




## Lähteet:

The Apache Software Foundation 2023. Apache HTTP Server Version 2.4 Documentation. Luettavissa:   https://httpd.apache.org/docs/2.4/vhosts/name-based.html Luettu 4.2.2024.

Karvinen Tero 2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. Luettavissa:
https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/ Luettu 1.2.2024.

Sammakkosuo Petri. Suullinen tiedonanto. 4.2. ja 5.2.2024.




















