## h6_Prod 

### Tehtävänanto  

x) Lue (Tiivistelmää ei tarvita, eli tästä lukutehtävästä ei palauteta mitään.)  
[Lehto 2022: Teoriasta käytäntöön pilvipalvelimen avulla kohta "a) Pilvipalvelimen vuokraus ja asennus"](https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/).  
[Karvinen 2022: Deploy Django 4 - Production Install](https://terokarvinen.com/2022/deploy-django/)  
[Karvinen 2017: First Steps on a New Virtual Private Server](https://terokarvinen.com/2017/09/19/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/)  
a) Vuokraa pilvipalvelin.  
b) Tee Djangosta tuotantotyyppinen asennus pilvipalvelimellesi.  
c) Vapaaehtoinen: vuokraa nimi ja laita se osoittamaan omaan palvelimeesi.  

Vinkit:

Tiistaina katsomme yhdessä pilvipalveluiden vuokrausta ja tuotantoasennuksia. Tee siis kaikki tehtävät niin pitkälle kuin suinkin osaat, jotta pääsemme konkreettisiin kysymyksiin.
Omia projekteja viimeistellään huomenna. Muista tehdä ensin valmista ja sitten vasta hienoa, jos aikaa jää.
GitHub Education tarjoaa pilvipalveluita ja nimen halvalla tai ilmaiseksi, mutta eivätpä nuo paljoa maksa markkinahintaankaan. Jos otat tämän, kannattaa olla etukäteen Github-profiilissa vahvistettu Haaga-Helian sähköpostiosoite.
Näkyykö nimi jo palveluntarjoajan omilla palvelimilla? Namecheapilla 'host www.gorasdhoo.com dns2.registrar-servers.com'




---
### a) Vuokraa pilvipalvelin.  

Olen tehnyt tunnuksen ja vuokrannut pilvipalvelimen aikaisemmin Teron [Linux-palvelimet](https://github.com/LiljestromNadja/DebianLinux/blob/main/h7_Based-Real_Internet.md#a-vuokraa-oma-virtuaalipalvelin-haluamaltasi-palveluntarjoajalta-vaihtoehtona-voit-k%C3%A4ytt%C3%A4%C3%A4-ilmaista-kokeilujaksoa-github-education-krediittej%C3%A4-tai-jos-mik%C3%A4%C3%A4n-muu-ei-onnistu-voit-kokeilla-vagrantia-paikallisesti) -kurssilla. Käytän siis tässäkin projektissa [DigitalOceanin](https://www.digitalocean.com/) dropletia. 

Dropletia varten tarvitaan SSH-avain, joten luodaan sellainen omassa terminaalissa:  

        nadja@debian:~$ ssh-keygen
        
        nadja@debian:~$ micro /home/nadja/.ssh/id_rsa.pub  
        
 Kopioi koko id_rsa.pub -tiedoston teksti ja liitä Dropletia luodessa: 
 
    > New SSH-key > SSH key content  




**Droplet luotu:**    

![droplet](https://github.com/LiljestromNadja/Django_course/assets/118609353/dd00b78c-cdb9-45a1-9700-9058526a2b03)


Otetaan SSH-yhteys:  

        nadja@debian:~$ ssh root@64.227.117.15
        
        Are you sure you want to continue connecting? 
        -> Yes

        -->
        root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# 
        
Päivitykset:  

        root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo apt-get update

        root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo apt-get -y dist-upgrade  
        
Palomuuri:  

    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo apt-get install ufw

Portti 22:  

    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo ufw allow 22/tcp  
    
Palomuuri päälle:  

    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo ufw enable
    
    -->
    Command may disrupt existing ssh connections. Proceed with operation (y|n)? 
    -> 
    y
    
    -->
    Firewall is active and enabled on system startup  
    
    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo ufw status (tai sudo ufw status verbose)
    
    -->
    Status: active

    To                         Action      From
    --                         ------      ----
    22/tcp                     ALLOW       Anywhere                  
    22/tcp (v6)                ALLOW       Anywhere (v6)       
    

Luodaan käyttäjä, jolla on sudo-oikeudet, jotta voidaan laittaa root kiinni:  

    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo adduser nadjaeta
    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo adduser nadjaeta sudo
    
Testataan: 

    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# su nadjaeta  
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:/root$ exit  
    
Sallitaan PassWordAuthentication:  

        root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudoedit /etc/ssh/sshd_config
        ->

        PasswordAuthentication yes

        root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# sudo systemctl restart ssh
    
Avataan uusi paikallinen terminaali:         

        nadja@debian:~$ ssh nadjaeta@64.227.117.15
        nadjaeta@64.227.117.15's password: 
        
        

![nadjaeta](https://github.com/LiljestromNadja/Django_course/assets/118609353/8cf99828-2fe8-4207-8f00-100ad5aceab4)

Kokeilun jälkeen poistutaan: 

        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ exit
        
Paikallisessa terminaalissa:  

        nadja@debian:~$ ssh-copy-id nadjaeta@64.227.117.15  

Viimeisen kerran salasana:  

        nadjaeta@64.227.117.15's password: 
        
        -->
        Number of key(s) added: 1
        
        Now try logging into the machine, with:   "ssh 'nadjaeta@64.227.117.15'"
        and check to make sure that only the key(s) you wanted were added.
        
..jonka jälkeen kirjautumaan pääsee SSH:n avulla:  

        nadja@debian:~$ ssh nadjaeta@64.227.117.15
        
        -->
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$
        
Rootin sulkeminen (kun ainakin yksi sudokäyttäjä on luotu ja testattu, sekä ssh-kirjautuminen lisätty kyseiselle käyttäjälle):  

        $ sudoedit /etc/ssh/sshd_config
        
        ->
        PermitRootLogin no
        PasswordAuthentication no (jos ei enää tarvitse päästä salasanalla kirjautumaan, avaa jos lisäät käyttäjiä)
        
        $ sudo systemctl restart ssh (vanha sudo service ssh restart)  
        $ sudo systemctl status ssh (vanha sudo service ssh status)


    
Poistuminen: 

    root@debian-s-1vcpu-1gb-fra1-01-summerdjango:~# exit  

    

---

### b) Tee Djangosta tuotantotyyppinen asennus pilvipalvelimellesi.   

#### Apache:  

        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo apt-get update
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo apt-get install apache2  (sudo apt-get -y install apache2)
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo systemctl start apache2



Testisivun korvaaminen: 

        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudoedit /var/www/html/index.html 
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ curl localhost
        -->Hello World! Great summer time!  
        
Käyttäjän kotisivu:  

        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo a2enmod userdir
        -->
        Enabling module userdir.
        To activate the new configuration, you need to run:
          systemctl restart apache2
          
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo systemctl restart apache2
        
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ pwd
        -->
        /home/nadjaeta
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ mkdir public_html
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ micro public_html/index.html   
        
        ->
        This is nadjaeta's Page
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo systemctl restart apache2
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ curl 'localhost/~nadjaeta/'  
        
        -->This in nadjaeta's Page!
        (huom ~nadjaeta/ ja /~nadjaeta ero)
        
Virheiden metsästys:   

        sudo tail /var/log/apache2/error.log  

        
Staattiset sivut:  

    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ cd
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ mkdir -p publicwsgi/nlilj/static
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ echo "this is static page" | tee publicwsgi/nlilj/static/index.html
    ->this is static page  
    
    curl localhost/static
    --> <title>404 Not Found</title>
    
    
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ sudoedit /etc/apache2/sites-available/nlilj.conf  
    
    ->
    <VirtualHost*:80>
        Alias /static/ /home/nadja2/publicwsgi/nli/static/
        <Directory /home/nadja2/publicwsgi/nli/static/>
            Require all granted
        </Directory>
    </VirtualHost> 
    
    
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ sudo a2ensite nlilj.conf 
    -->
    Enabling site nlilj.
    To activate the new configuration, you need to run:
      systemctl reload apache2
      
      
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ sudo a2dissite 000-default.conf 
    -->
    Site 000-default disabled.
    To activate the new configuration, you need to run:
      systemctl reload apache2
      
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ /sbin/apache2ctl configtest
    -->
    apache2: Syntax error on line 225 of /etc/apache2/apache2.conf: Syntax error on line 6 of /etc/apache2/sites-enabled/nlilj.conf: Expected </VirtualHost*:80> but saw </VirtualHost>
    Action 'configtest' failed.
    The Apache error log may have more information.  
    
    
    
Korjataan virhe lisäämällä välilyönti:  

            ->
    <VirtualHost *:80>
        Alias /static/ /home/nadja2/publicwsgi/nli/static/
        <Directory /home/nadja2/publicwsgi/nli/static/>
            Require all granted
        </Directory>
    </VirtualHost> 
    
    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ /sbin/apache2ctl configtest 
    
    -->
    AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
    Syntax OK
    
    
Käynnistetään Apache2 uudelleen:  

    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ sudo systemctl restart apache2


Testataan:  

    nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ curl localhost/static/
    --> this is static page
    
    
#### Virtualenv:  

        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj/static$ cd
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ pwd
        --> /home/nadjaeta  
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo apt-get -y install virtualenv  
        
        (nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ cd) 
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ cd publicwsgi/
        
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ virtualenv -p python3 --system-site-packages env  
        
        
Virtual envin aktivoiminen:  

        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ source env/bin/activate
        --> (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ 
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ which pip
        --> /home/nadjaeta/publicwsgi/env/bin/pip  
        
#### Django:  

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ micro requirements.txt
        -> django
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ pip install -r requirements.txt 
        -->
        Successfully installed asgiref-3.7.2 django-4.2.1 sqlparse-0.4.4 typing-extensions-4.6.2

Djangon versio:  

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ django-admin --version
        --> 4.2.1  
        
Uusi Django-projekti(sivusto):  

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ django-admin startproject nlilj
        --> CommandError: '/home/nadjaeta/publicwsgi/nlilj' already exists
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ ls
        --> env  nlilj  requirements.txt

        
        
Koska aiemmin luotiin kansio nlilj, jossa /static/index.html, samannimistä kansiota ei pysty luomaan. Poistetaan kansio (palautetaan projektin luomisen jälkeen):  

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ rm -r nlilj/
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ ls
        --> env  requirements.txt

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ django-admin startproject nlilj
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ ls
        --> env  nlilj  requirements.txt

        
Palautetaan static: 
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ mkdir nlilj/static
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ micro nlilj/static/index.html

        -> new static! 
        
Testataan:  

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ sudo systemctl restart apache2
        --> [sudo] password for nadjaeta:
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ curl localhost/static/
        --> new static !  
        
        
#### Pythonin ja Apachen kommunikaatio = mod.wsgi  

    (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ sudoedit /etc/apache2/sites-available/nlilj.conf 
     
    -> 
    Define TDIR /home/nadjaeta/publicwsgi/nlilj                                     
    Define TWSGI /home/nadjaeta/publicwsgi/nlilj/nlilj/wsgi.py                      
    Define TUSER nadjaeta                                                           
    Define TVENV /home/nadjaeta/publicwsgi/env/lib/python3.9/site-packages          

    <VirtualHost *:80>                                                              
            Alias /static/ ${TDIR}/static/                                          
            <Directory ${TDIR}/static/>                                             
                    Require all granted                                             
            </Directory>                                                            

            WSGIDaemonProcess ${TUSER} user=${TUSER} group=${TUSER} threads=5 python
            WSGIScriptAlias / ${TWSGI}                                              
            <Directory ${TDIR}>                                                     
                 WSGIProcessGroup ${TUSER}                                          
                 WSGIApplicationGroup %{GLOBAL}                                     
                 WSGIScriptReloading On                                             
                 <Files wsgi.py>                                                    
                    Require all granted                                             
                 </Files>                                                           
            </Directory>                                                        

        </VirtualHost>                                                                  

        Undefine TDIR                                                                   
        Undefine TWSGI                                                                  
        Undefine TUSER                                                                  
        Undefine TVENV                                                                  
                                                                                
                                                                                
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ sudo apt-get -y install libapache2-mod-wsgi-py3 
        
        -->
        Reading package lists... Done
        Building dependency tree... Done
        Reading state information... Done
        libapache2-mod-wsgi-py3 is already the newest version (4.7.1-3+deb11u1).
        0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

Syntaksin tarkistus:  

    (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ /sbin/apache2ctl configtest 
    
    -->
    AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
    Syntax OK  
    
    (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ sudo systemctl restart apache2
    
    
Testataan:    

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ curl localhost | grep title
        

          % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                         Dload  Upload   Total   Spent    Left  Speed
        100 10731  100 10731    0     0  20440      0 --:--:-- --:--:-- --:--:-- 20401
                <title>The install worked successfully! Congratulations!</title>
                
                
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ curl -sI localhost|grep Server
        -->Server: Apache/2.4.56 (Debian)
        
#### Debug pois:  

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ cd 
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ cd publicwsgi/nlilj/
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj$ micro nlilj/settings.py 
        
        ->
        DEBUG = False
        
        ALLOWED_HOSTS = ["localhost", "nadjaliljestrom.website"]
        
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj$ touch nlilj/wsgi.py 
        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi/nlilj$ sudo systemctl restart apache2  
        
        
Nyt etusivun pitäisi näyttää 404 - Not Found, koska sinne ei ole määritelty (vielä) mitään:  

        (env) nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~/publicwsgi$ curl -s 'localhost'|grep title
        --> <title>Not Found</title>





                                                                       



        












    


    
    


        




        





*** 
---
---  

Asensin Micron:  

        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ sudo apt-get install micro
        nadjaeta@debian-s-1vcpu-1gb-fra1-01-summerdjango:~$ export EDITOR=micro

    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)  
[Karvinen 2022: Deploy Django 4 - Production Install](https://terokarvinen.com/2022/deploy-django/)  




