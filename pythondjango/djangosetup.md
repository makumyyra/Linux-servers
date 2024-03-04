***Huom: Laksu-palautuksessa ilmoitettu versio ei ole enää kesken. Valmis 4.3.2024 klo 17:42.***


# Django 3 setup

> Käytetyn koneen speksit:  
Kone: Dell Latitude 7280  
Suoritin: Intel(R) Core(TM) i7-7600U CPU @ 2.80GHz 2.90 GHz  
Asennettu RAM: 16,0 Gt  
Windowsin määritykset: Windows 10 Pro, versio 22H2  

Tehtävä aloitettu 1.3.2024 klo 10:16.

# Tekstitiivistelmät

## Karvinen 2021: Django 4 Instant Customer Database Tutorial
- Django on Python-pohjainen laajasti käytetty verkkokehys (framework)
- Django-projektit ovat yleensä "isoja", esimerkiksi nettisivusto
- Artikkelissa kuvataan, miten saadaan käyttöön Djangon valmiina tarjoilema hallintanäkymä (perusasioita mm. login ja käyttäjien hallinta tietokannassa)

## Karvinen 2021: Deploy Django 4 - Production Install

# Djangon käyttöönotto

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

(Karvinen 2021 b.)

## Vianselvitys

### Selaimessa osoitteena vahingossa http://localhost/static (ei kautta-merkkiä)

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

Kysyin neuvoa kanssaopiskelijoilta opiskelijoiden WhatsAppissa. Sieltä tulikin pian neuvo, että osoitteessa pitää olla myös viimeinen kautta-merkki 

Tehtävä lopetettu 1.3.2024 klo 13:25.

### Selaimessa oikea osoite http://localhost/static/

Tehtävää jatkettu 2.3.2024 klo 12:02.

Sivu ei vieläkään toiminut, joten kävin tarkistamassa konfiguroinnit (```sudoedit /var/www/sites-available/suvis.conf```). Sinne oli wsgi-poluksi määrittynyt vahingossa .../suvis/suvis.py. Korjasin polun lopuksi ...suvis/wsgi.py, jonka jälkeen käynnistin Apachen uudelleen. Nyt sivu toimi.
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/django_local.JPG)

Komennolla ```curl -sI localhost | grep Server``` sain vielä tarkistettua, että sivua pyöritti Apache2. Komennon vastaus oli Server: Apache/2.4.57 (Debian), mikä sen kuuluikin olla.

(Karvinen 2021 b., Sammakkosuo 2024., WhatsApp 2024.)

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

(Karvinen 2021 b.)

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

(Karvinen 2021 b.)

## Stylesheet 

Seuraavaksi hain sivustolle käytettäväksi stylesheetin. Ensin suvis/settings.py, jonne lisättiin static root ```STATIC_ROOT = os.path.join(BASE_DIR, 'static/')```. Sen lisäksi piti importteihin lisätä "import os". Sitten CSS otettiin käyttöön komennolla ```./manage.py collectstatic``` ("yes"). Sivuston ulkoasu muuttui:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/stylesheet.JPG)

Jotta pääsin kirjautumaan sisään, tein uuden pääkäyttäjän. Sitä ennen piti kuitenkin ajaa migraatiot (koska käytiin muuttamassa settings.py): 
```./manage.py makemigrations```
```./manage.py migrate ```

Superuser luotiin komennolla ```./manage.py createsuperuser```

(Karvinen 2021 b.)

Tehtävät lopetettu 2.3.2024 klo 14:48.

## Instant Customer DB

Tehtävä aloitettu 3.3. klo 19:04.

Suurin osa sivuston [Django CRM](https://terokarvinen.com/2022/django-instant-crm-tutorial/) listaamista toimista oli jo tehty aiemmassa harjoituksessa (yllä). Pystyin siis skippaamaan ohjeita ensiksi "Hello DJ Ango"-osioon asti.

Jostain syystä projekti ei avautunut oikealla URL:illa.  Kävin katsomassa konffitiedostoa ```sudoedit /etc/apache2/mods-available/alias.conf```, mutta siellä oli kaikki kunnossa (Require all granted).

Arvelin, että ehkä selain ei tunnista harjoituksessa annettua osoitetta http://127.0.0.1:8000 VAIKKA settings.py:ssä oli jo annettu sallituksi hostiksi "localhost". Laitoin suvis/suvis/settings.py -tiedostoon vielä erikseen "127.0.0.1".
![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/allowedhosts.JPG)

Sen jälkeen etusivu lähti toimimaan. Sivustolle localhost oli jo ylempänä määritelty admin, joten en luonut toista. Myös tyylisivu oli haettu, joten en tarvinnut sitäkään. Osoitteessa http://127.0.0.1:8000 tyylisivu ei jostain syystä näytä toimivan, mutta osoitteessa localhost toimii sattumalta tyylisivu sekä koko CRM. En tiedä, miten sivut menevät sekaisin.

(Karvinen 2021 a.)

## CRM

Ensiksi loin ohjeiden mukaan projektin ```./manage.py startapp crm``` Tämä teki crm-sovellukseen valmiiksi crm/ -kansion. Projektin nimi ('crm') piti lisätä tiedostoon suvis/settings.py kohtaan INSTALLED_APPS.

Sen jälkeen loin Customer-mallin crm/models.py -tiedostoon. Komennot:

> ```from django.db import models```  

class Customer(models.Model):  
   name = models.CharField(max_length=300)  
     
   Näiden alle kirjoitetaan:  
   def __str__(self):  
		return self.name  
   
Lisätyt mallit luovat automaattisesti taulun tietokantaan. Tauluun rakentui sarake "name". 
Funktion "__str__" määrittelyllä saadaan asiakaslistassa näkymään objektin nimi-ominaisuus ("Suvi Sammakkosuo"). Ilman tätä listassa lukisi taulun nimi+id (esim. "Customer 1").

"Tallennetaan" muutokset:

> ```$ ./manage.py makemigrations```  
```./manage.py migrate```  

Tämän jälkeen rekisteröin vielä muutokset admin.py -tiedostossa:

> ```crm/admin.py```  
from django.contrib import admin  
from . import models  
  
admin.site.register(models.Customer)  


Näiden jälkeen sivusto (sis. Customer-lista) näkyy osoitteessa localhost/admin/:

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/local_adm.JPG)

...mutta ei osoitteessa http://127.0.0.1:8000/admin/

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/127admin.JPG)

En tiedä, mistä tämä johtuu.

(Karvinen 2021 a.)

Tehtävä lopetettu 3.3. klo 20:11.

#### Palomuuri

(Tajusin jossain vaiheessa tehtävää, että olin tehnyt uuden virtuaalikoneen. Siinä ei ollut palomuuri vielä päällä, joten asensin sen.)
Palomuuri: 

![image](https://raw.githubusercontent.com/makumyyra/Linux-servers/main/md_images/pingviini6/palomuuri1.JPG)


## Lähteet:

Karvinen 2021 a. Django 4 Instant Customer Database Tutorial. Luettavissa: https://terokarvinen.com/2022/django-instant-crm-tutorial/ Luettu 2.3.2024.

Karvinen 2021 b. Deploy Django 4 - Production Install. Luettavissa https://terokarvinen.com/2022/deploy-django/?fromSearch=django Luettu 1.3.2024.

Sammakkosuo Petri 2024. Suullinen tiedontanto 2.3.2024.

WhatsApp 2024. Suullinen tiedonanto 1.3.2024: Osallistuja nimeltä Ilona. 
