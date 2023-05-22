# Hello Django
## Djangon asennus - Tehtävänanto  

h1 Hello Django  
x) Lue (tiivistelmää ei tarvita, eli tästä lukutehtävästä ei palauteta mitään)  

[Karvinen 2022: Django 4 Instant Customer Database Tutorial](https://terokarvinen.com/2022/django-instant-crm-tutorial/)  
Tee ja raportoi  

a) Asenna Django-kehitysympäristö (ja testaa, että etusivu näkyy)  
b) Vapaaehtoinen: Tee yksinkertainen taulu tietokantaan (models.py)  
c) Vapaaehtoinen: Muokkaa tietokantaa Django Admin -weppiliittymällä  
d) Vapaaehtoinen: Tee tauluun (luokkaan) uusia kenttiä  
e) Vapaaehtoinen, vaikea: tee kaksi taulua/luokkaa ja niiden välille riippuvuus  
Kuten aina, palauta Laksuun klo 23 mennessä ja arvioi kaksi.  

---

### Ennen Djangoa
Koneellani oli jo valmiiksi debian-live-11.6.0-amd64-xfce+nonfree.iso -tiedosto (edellisen [Linux-kurssin](https://github.com/LiljestromNadja/DebianLinux/blob/main/h1_InstallingDebian.md) jäljiltä). 
Ajattelin virkistää muistiani ja asentaa uuden, tyhjän virtuaalikoneen kesäkurssikäyttöön. Asennukseen käytin  Oracle VM VirtualBox Manageria. 
Matkaan tuli muutama mutka, jotka kuitenkin sain ratkaistua opettajan ja internetin neuvoilla. 

Muistin virkistämistä ja pari ongelmaa siis: 


 #### Sudo-oikeuksien lisääminen käyttäjälle:  

    nadja@debian: su root
    root@debian:# sudo adduser sudo nadja

#### Päivitykset:  

    $ sudo-apt get update 
    $ sudo apt-get -y dist-upgrade  


#### Palomuuri:  

    $ sudo apt-get -y install ufw  
    $ sudo ufw enable
    
Tässä kohtaa tuli ongelma:

    "E: Package 'ufw' has no installation candidate"
    
Ensin kuitenkin kommunikaatio guestin ja hostin välille, niin on kivempi copy-pasteta. 
    
<br><br>

#### Skaalautuvuus ja leikepöytä hostin ja guestin välillä 

Virtual Box Guest Additions eli skaalautuvuus ja copy-paste isäntäjärjestelmän ja guestin välillä

Valikosta **Devices**    
    > insert guest additions cd image


Valikosta **Devices**  
    > shared clipboard -> bidirectional  
	  > drag & drop -> bidirectional
	


Komentorivillä:  
    
    $ cd
    
    $ cd /media (mennään kansioon /media)
    
    $ ls (katsellaan mitä löytyy)
    
    $ cd cdrom
    $ ls
    $ sudo bash VBoxLinuxAdditions.run 

	
	
Tämän jälkeen virtuaalikoneen uudelleenkäynnistys. 

<br><br>

#### Ongelma: Palomuuri 

... jonka jälkeen takaisin ufw-ongelman kimppuun. Tosiaan, tilanne oli tämä:   


    nadja@debian: sudo apt-get -y install ufw

    Reading package lists... Done
    Building depedency tree... Done
    Reading state information... Done
    Package ufw is not available, but it referred to by another package.
    This may mean that the package is missing, has been obsoleted, or 
    is only available from another source 

    E: Package 'ufw' has no installation candidate 
    



Ratkaisuksi tähän sekä opettaja että internetin ihmeellinen maailma tarjosivat tällaista:  

    $ sudoedit  /etc/apt/sources.list

    deb http://deb.debian.org/debian bullseye main contrib non-free
    deb-src http://deb.debian.org/debian bullseye main contrib non-free

    deb http://deb.debian.org/debian bullseye-updates main contrib non-free
    deb-src http://deb.debian.org/debian bullseye-updates main contrib non-free

    deb http://security.debian.org/debian-security/ bullseye-security main contrib non-free
    deb-src http://security.debian.org/debian-security/ bullseye-security main contrib non-free


Tämän jälkeen uudelleen:   

    $ sudo apt-get update
    $ sudo apt-get -y dist-upgrade  

    $ sudo apt-get -y install ufw  
    $ sudo ufw enable
    
    
Ja toimii: 


![UFW_ongelma_RATKAISTU](https://github.com/LiljestromNadja/Django_course/assets/118609353/d54ce6f2-29ad-434f-ba80-ab0aa389b72a)


<br><br>
#### Tree 

Asensin myös "tree":n, jonka avulla saa selkeämmän näkymän tiedostojen kansiorakenteeseen:  

    $ sudo apt install tree
    
 ![tree](https://github.com/LiljestromNadja/Django_course/assets/118609353/6e41d959-9a7e-40e4-bcef-237e8c499031)

---

### a) Asenna Django-kehitysympäristö (ja testaa, että etusivu näkyy)  

#### Kehitysympäristön asennus ja käyttöönotto  

Pip-paketit voivat sisältää mitä vain mm. haittaohjelmia, eli kannattaa olla tietoinen mitä pakettia on lataamassa ja sen lisäksi olla tarkkana että kirjoittaa paketin nimen oikein.  

Päivitykset:  

		nadja@debian:~$ sudo apt-get update
		
		nadja@debian:~$ ls
		
Luodaan django -kansio ja asennetaan virtual environment:  
		
		nadja@debian:~$ mkdir django
		nadja@debian:~$ cd django/
		
		nadja@debian:~/django$ sudo apt-get -y install virtualenv
		
		nadja@debian:~/django$ virtualenv --system-site-packages -p python3 env/  


Aktivoidaan kehitysympäristö:  
	
	nadja@debian:~/django$ source env/bin/activate
	
Testataan:  

	(env) nadja@debian:~/django$ which pip
	
	-> /home/nadja/django/env/bin/pip  
	
Asennetaan micro:  

	nadja@debian:~/django$ sudo apt-get install micro

Kehitysympäristön deaktivointi:  

	(env) nadja@debian:~/django$ deactivate  
	
	
Luodaan requirements.txt, johon kirjoitetaan "django":  

	(env) nadja@debian:~/django$ micro requirements.txt
	
	-> django
	
	CTRL + S
	CTRL + Q  
	
Asennetaan Django:  

	(env) nadja@debian:~/django$ pip install -r requirements.txt  
	
Katsotaan Djangon versio:  

	(env) nadja@debian:~/django$ django-admin --version
	-> 4.2.1
	
Luodaan django -kansioon (jossa on env ja requirements.txt) kansio public_sites: 

	(env) nadja@debian:~/django$ mkdir public_sites
	
Siirrytään public_sites -kansioon ja luodaan sinne projekti (sivusto):  

	(env) nadja@debian:~/django$ cd public_sites/
	
	(env) nadja@debian:~/django/public_sites$ django-admin startproject kesadjango  
	
Mennään yksi kansio ylöspäin:  

	(env) nadja@debian:~/django/public_sites$ cd ..
	
	(env) nadja@debian:~/django$ tree public_sites/  
	
![treepublicsites](https://github.com/LiljestromNadja/Django_course/assets/118609353/a96f9f75-4236-4d67-af86-f321e6b03fe0)


Testataan käynnistämällä kehityspalvelin(toimitaan tasolla missä on manage.py-tiestosto). Tämä on "päätaso", josta manage.py -tiedosto löytyy. Kaikki Djangon komennot komentoriviltä annetaan './manage.py':llä:    

	(env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py runserver  
	
![managepyrunserver1](https://github.com/LiljestromNadja/Django_course/assets/118609353/04786962-4441-43eb-8e39-6269caa67a34)

Selaimessa:  

![djangoonnistui](https://github.com/LiljestromNadja/Django_course/assets/118609353/d040ae01-9a71-4bfb-8d42-dba43cb4c40b)

Serverin sulkeminen:  

	CTRL + C  
	



---

### b) Vapaaehtoinen: Tee yksinkertainen taulu tietokantaan (models.py) 

Avataan uusi terminaali-ikkuna, mennään kansioon django ja aktivoidaan kehitysympäristö:  

	nadja@debian:~/django$ source env/bin/activate

	
Jonka jälkeen projektikansioon, missä on manage.py -tiedosto ja otetaan tietokanta käyttöön. Nämä komennot tulee muistaa aina kun tietokantaan tehdään muutoksia:  
	
	(env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py makemigrations  
	
	(env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py migrate  
	

![migrate](https://github.com/LiljestromNadja/Django_course/assets/118609353/f9bf6bf3-bfa8-4e2c-b40c-61d40568e7b7)
	
	
Seuraavaksi luodaan admin-käyttäjä:  

	(env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py createsuperuser
![superuser](https://github.com/LiljestromNadja/Django_course/assets/118609353/cabb3078-3cef-4d89-9347-4b8159d77656)

Kokeillaan kirjautua selaimella äsken luodulla admin-käyttäjällä "nadja" (http://127.0.0.1:8000/admin):  

![adminkirjautuminen](https://github.com/LiljestromNadja/Django_course/assets/118609353/69cc99bf-babc-4071-bf4b-716ea75932a2)


Lisätään käyttäjä "Kayttaja1":  

![lisaakayttaja](https://github.com/LiljestromNadja/Django_course/assets/118609353/9c827cd4-2206-4745-8c68-0f39d9b606f2)




	


	

	

	




### c) Vapaaehtoinen: Muokkaa tietokantaa Django Admin -weppiliittymällä  

### d) Vapaaehtoinen: Tee tauluun (luokkaan) uusia kenttiä  

### e) Vapaaehtoinen, vaikea: tee kaksi taulua/luokkaa ja niiden välille riippuvuus 





---
    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)
