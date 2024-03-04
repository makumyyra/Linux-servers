# Django 3 setup

> Käytetyn koneen speksit:  
Kone: Dell Latitude 7280  
Suoritin: Intel(R) Core(TM) i7-7600U CPU @ 2.80GHz 2.90 GHz  
Asennettu RAM: 16,0 Gt  
Windowsin määritykset: Windows 10 Pro, versio 22H2  

## Djangon käyttöönotto

Toimin sivun [Deploy Django](https://terokarvinen.com/2022/deploy-django/?fromSearch=django) ohjeiden mukaan.

Ensin kotikansioon piti luoda kansio publicwsgi, sen alle vapaasti nimettävä kansio, ja sitten vielä static. Esimerkiksi 
```mkdir -p publicwsgi/suvis/static```

Static-kansiolle tehtiin VirtualHost:

> <VirtualHost *:80>  
	Alias /static/ /home/suvi/publicwsgi/jotain/static/  
	<Directory /home/suvi/publicwsgi/jotain/static/>  
		Require all granted  
	</Directory>  
</VirtualHost>  

Tämän jälkeen annettiin komennot 
```sudo a2ensite (omasivu.conf) & sudo a2dissite 000-default.conf```
Syntaksin voi tarkastaa komennolla ```/sbin/apache2ctl configtest```
Sitten ```sudo systemctl restart apache2```

Selaimen pääsyoikeudet näkee, kun hakee sivun tietoja curlilla.
```curl http://localhost/static/```

*HUOM: Tässä tapauksessa on merkitystä sillä, että osoitteen perässä on kautta-merkki. Koska en tajunnut laittaa sitä, luulin, että sivu ei toimi. Sen jälkeen käytin paljon aikaa vianselvitykseen: *

### Vianselvitys

#### Selaimessa osoitteena vahingossa http://localhost/static (ei kautta-merkkiä)

Tarkistin apachen version komennolla ```sudo apachectl status```. Versio on 2.4.57.
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/apversion.JPG)

Ensin selain ilmoitti, että "*file does not exist*". 
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/noexist.JPG)

Sain virheen myös access rightseista.

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/forbidden.JPG)

Tiedosto oli olemassa, ja alemman virheen vuoksi ajattelin, että selain ei vain pääse kansioon.

Yritin korjata tätä komennoilla 
```chmod ugo+r $HOME $HOME/publicwsgi/```
```ls -ld $HOME $HOME/publicwsgi/```
(^ Huom: Kaikilla oli tänne jo execute-oikeudet, joten lisäsin siksi varmuuden vuoksi myös lukuoikeuden ("r") (turhaan).

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/ugo_r.JPG)

Senkään jälkeen sivu http://localhost/static ei tietenkään toiminut. Virheilmoituksessa luki tällä kertaa, että "*client denied by server configuration*":
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/deniedbyserver.JPG)

Kysyin neuvoa kanssaopiskelijoilta opiskelijoiden WhatsAppissa. Sieltä tulikin pian neuvo, että osoitteessa pitää olla myös viimeinen kautta-merkki (WhatsApp 1.3.2024).

#### Selaimessa oikea osoite http://localhost/static/

Sivu ei vieläkään toiminut, joten kävin tarkistamassa konfiguroinnit (```sudoedit /var/www/sites-available/suvis.conf```). Sinne oli wsgi-poluksi määrittynyt vahingossa .../suvis/suvis.py. Korjasin polun lopuksi ...suvis/wsgi.py, jonka jälkeen käynnistin Apachen uudelleen. Nyt sivu toimi.
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/django_local.JPG)

Komennolla ```curl -sI localhost | grep Server``` sain vielä tarkistettua, että sivua pyöritti Apache2. Komennon vastaus oli Server: Apache/2.4.57 (Debian), mikä sen kuuluikin olla.

## VirtualEnv ja Django

Seuraavaksi piti asentaa virtuaaliympäristö VirtualEnv ja asentaa sinne Django:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/installvirtual.JPG)

> ```sudo apt-get -y install virtualenv ```  
```cd publicwsgi/```  
```virtualenv -p python3 --system-site-packages env``` 
```source env/bin/activate```  

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/envactivate.JPG)

Sitten tarkistettiin, että paketinhallintajärjestelmä pip (Package Installer for Python) oli oikeassa kansiossa (env/):
> ```which pip```

Sitten tehtiin requirements.txt-tiedosto, jonne kirjoitettiin sana django. Tämä siksi, että riippuvuudet asennetaan yleensä tiedostosta.
```pip install -r requirements.txt```

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/pipactivate.JPG)

## Uusi Django-projekti

Django-projekti aloitetaan suorittamalla komento 

```django-admin startproject projektinnimi```

Django-projektissa pitää määrittää VirtualHost. Se onnistuu tässä tapauksessa helpoiten käyttämällä muuttujia:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/conf.JPG)

Sen jälkeen asennettiin WSGI-moduuli, testattiin syntaksi ja käynnistettiin Apache2 uudelleen. 

> ```sudo apt-get -y install libapache2-mod-wsgi-py3```  
```/sbin/apache2ctl configtest```  
```sudo systemctl restart apache2```  

Sitten testattiin: 
```curl -s localhost|grep title```

Sain onnistumisviestin: 

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/django_local.JPG)

Seuraavaksi laitettiin nettiin publicwsgi/nimi/nimi/settings.py:ssä näkyvä debug pois päältä (DEBUG = False) ja määriteltiin sallitut hostit: 
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/allowedhosts.JPG)

Ilmoitin muutokset Apachelle komennolla ```touch suvis/wsgi.py``` ja käynnistin Apachen uudelleen ```sudo systemctl restart apache2```

Tämän jälkeen osoite http://localhost/admin toimi (muttei varsinaisesti tehnyt mitään):
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/localhost_admin.JPG)


## Stylesheet 

Seuraavaksi hain sivustolle käytettäväksi stylesheetin. Ensin suvis/settings.py, jonne lisättiin static root ```STATIC_ROOT = os.path.join(BASE_DIR, 'static/')```. Sen lisäksi piti importteihin lisätä "import os". Sitten CSS otettiin käyttöön komennolla ```./manage.py collectstatic``` ("yes"). Sivuston ulkoasu muuttui:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/stylesheet.JPG)

Jotta pääsin kirjautumaan sisään, tein uuden pääkäyttäjän. Sitä ennen piti kuitenkin ajaa migraatiot (koska käytiin muuttamassa settings.py): 
```./manage.py makemigrations```
```./manage.py migrate ```

Superuser luotiin komennolla ```./manage.py createsuperuser```


## Instant Customer DB


Kävin katsomassa konffitiedostoa ```sudoedit /etc/apache2/mods-available/alias.conf```, mutta siellä oli kaikk ikunnossa (Require all granted)

Tiedostoon suvis/suvis/settings.py piti laittaa sallittuihin hosteihin vielä erikseen "127.0.0.1". 
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/allowedhosts.JPG)


(Tajusin jossain vaiheessa tehtävää, että olin tehnyt uuden virtuaalikoneen. Siinä ei ollut palomuuri vielä päällä, joten asensin sen.)
Palomuuri: 

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/palomuuri1.JPG)
