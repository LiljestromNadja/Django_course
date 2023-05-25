## h4 Suhteita 

### Tehtävänanto  

x) Silmäile: [Django contributors 2023: Django 4.2 Documentation Many-to-one relationships](https://docs.djangoproject.com/en/4.2/topics/db/examples/many_to_one/)
(Tiivistelmää ei tarvita, eli tästä lukutehtävästä ei palauteta mitään.)  

a) Tee alusta lähtien uusi CRUD-ohjelma, jossa on vähintään kaksi model-luokkaa (ja siten taulua) sekä niiden välillä riippuvuus (ForeignKey). Kokeile, että riippuvuus toimi sekä automaattisessa hallintaliittymässä (Django admin) että itse muoteilla tekemilläsi sivuilla.  
(Sellainen, mitä et ole vielä tehtnyt. Jotain muuta kuin todo-lista, crm tai linnut. Ei tarvitse tehdä raporttia, lähdekoodi ja README.md:ssä kuvaus, mitä ohjelma tekee ja ruutukaappaus lopputuloksesta. Voit laittaa projektin omaksi kansiokseen esimerkiksi Github- tai Gitlab-varastoon.)  

b) Vapaaehtoinen: Tee Djangolla sivu, joka vaatii kirjautumista (LoginRequiredMixin). Voit tehdä tämän sivun mihin vain olemassaolevaan projektiin.  


---

### a) Tee alusta lähtien uusi CRUD-ohjelma, jossa on vähintään kaksi model-luokkaa (ja siten taulua) sekä niiden välillä riippuvuus (ForeignKey). Kokeile, että riippuvuus toimi sekä automaattisessa hallintaliittymässä (Django admin) että itse muoteilla tekemilläsi sivuilla.  


### [>>>Projektin koodit<<<](https://github.com/LiljestromNadja/djangoSummer)






























---

<!-- 

### Migraatio-ongelma

Kun migraatio menee sekaisin, tätä voi kokeilla -> ei tuotannossa!


    ls
    rm db.sqlite3 
    ./manage.py makemigrations ; ./manage.py migrate
    micro products/models.py 
    ls
    ls kesadjango/
    find | grep mig
    mv -n -v products/migrations/ DISABLED-migrations
    ls
    ./manage.py makemigrations ; ./manage.py migrate
    
  -->


---
---
    
#### Lähteet ja materiaalit:  

[Django contributors 2023: Django 4.2 Documentation Many-to-one relationships](https://docs.djangoproject.com/en/4.2/topics/db/examples/many_to_one/)  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)  
[Tero's Classy Django Cheatsheet](https://terokarvinen.com/2023/django-cheatsheet/)  

**Hyödyllistä: **  

[Django 4.2 Documentation Working with forms](https://docs.djangoproject.com/en/4.2/topics/forms/)  
[Django 4.2 Documentation Model field reference](https://docs.djangoproject.com/en/4.2/ref/models/fields/#datetimefield)



