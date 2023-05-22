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


### a) Asenna Django-kehitysympäristö (ja testaa, että etusivu näkyy)  

### b) Vapaaehtoinen: Tee yksinkertainen taulu tietokantaan (models.py)  




### c) Vapaaehtoinen: Muokkaa tietokantaa Django Admin -weppiliittymällä  

### d) Vapaaehtoinen: Tee tauluun (luokkaan) uusia kenttiä  

### e) Vapaaehtoinen, vaikea: tee kaksi taulua/luokkaa ja niiden välille riippuvuus 





---
    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)
