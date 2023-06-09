## h2 Muotillista
### Tehtävänanto

x) Silmäile: Karvinen 2023: Tero's Classy Django Cheatsheet  
(Vain siltä osin kuin on tähän asti opeteltu. Tutut asiat ovat artikkelin alussa, suurin osa on vielä opettelematta ja opettelematonta osaa ei tarvitse lukea vielä. Tiivistelmää ei tarvita, eli tästä lukutehtävästä ei palauteta mitään.)  

a) Tee Django-ohjelma, joka listaa kaikki tietueet etusivulla (ListView). Palauta lähdekoodi ja ruutukaappaus lopputuloksesta.
(Jotain muuta kuin todo-lista, crm tai linnut. Ei tarvitse tehdä raporttia, lähdekoodi ja README.md:ssä kuvaus, mitä ohjelma tekee ja ruutukaappaus lopputuloksesta. Voit laittaa projektin omaksi kansiokseen esimerkiksi Github- tai Gitlab-varastoon.)  

b) Vapaaehtoinen: Tee käyttöliittymä uusien tietueiden lisäämiseen (CreateView)  
c) Vapaaehtoinen: Tee käyttöliittymä tietueiden muokaamiseen (UpdateView)  
d) Vapaaehtoinen: Tee käyttöliittymä tietueiden poistamiseen - ja varmista, haluatko todella poistaa (DeleteView)  

Kuten aina, palauta Laksuun klo 23 mennessä ja arvioi kaksi.  


---

### a) Tee Django-ohjelma, joka listaa kaikki tietueet etusivulla (ListView). Palauta lähdekoodi ja ruutukaappaus lopputuloksesta.

### [Projektin koodit](https://github.com/LiljestromNadja/djangoSummer)


Luodaan app people:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py startapp people

 
![people](https://github.com/LiljestromNadja/Django_course/assets/118609353/5e447981-88c6-4f99-8f42-530a690ff6a8)
 
Lisätään projektin kesadjango settings.py -tiedostoon äsken luotu app:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro kesadjango/settings.py 

    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',    
    'todo', #add apps here
    'people'    
    ]

Luodaan people-appiin luokka Contact:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro people/models.py  
    
    from django.db import models

    class Contact(models.Model):
        fullname = models.CharField(max_length=150)
        nickname = models.CharField(unique = True, max_length=150)
        email = models.CharField(max_length=150)
        phone = models.CharField(max_length=100)
        city = models.CharField(max_length=100)


        def __str__(self):
            return self.fullname  
        
        
Lisätään luokka Contact people/admin.py -tiedostoon:  

    (env) nadja@debian:~/django/public_sites/kesadjango/people$ micro admin.py 
    
    from django.contrib import admin
    from .models import * 

    admin.site.register(Contact)  
  
Testataan selaimessa lisätä ja saadaan error:  

        Exception Type: 	OperationalError
        Exception Value:    no such table: people_contact
    
Ilmoitus sanoo että kyseistä taulua ei ole. Päivitetään siis muutokset tietokantaan:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py makemigrations ; ./manage.py migrate
            
Testataan:  

![lisaacontact](https://github.com/LiljestromNadja/Django_course/assets/118609353/7f4078f8-1ad5-4f62-9e11-0e85ac931815)  



Päivitetään kesädjango/urls.py -listausta:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro kesadjango/urls.py  
    
    from django.contrib import admin
    from django.urls import path, include 


    urlpatterns = [
    path('admin/', admin.site.urls), 
    path('todo/', include('todo.urls')),
    path('people/',  include('people.urls')), 
   
    ]
    
    
Jolloin saadaan viesti:  

    ModuleNotFoundError: No module named 'people.urls'  
    
    
Lisätään people-appiin tiedosto urls.py:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro people/urls.py  
    
    from django.urls import path
    from .views import *

    urlpatterns = [
    path('', ContactListView.as_view()),
   
    ]  
    
    
Jolloin saadaan viesti:  
    
    NameError: name 'ContactListView' is not defined  
    

Päivitetään tiedosto people/views.py
    
    from django.views.generic import TemplateView

    class ContactListView(TemplateView):
        template_name = 'people/contacts.html'


Kokeillaan ja saadaan viesti:  

    Exception Type: 	TemplateDoesNotExist
    Exception Value: 	people/contacts.html
    
    
Täytynee siis luoda kaivattu tiedosto. Tehdään ensin templates-kansio, johon saadaan kaikki appsin templatet:  

        
        (env) nadja@debian:~/django/public_sites/kesadjango/people$ mkdir templates
        (env) nadja@debian:~/django/public_sites/kesadjango/people/templates$ mkdir people
        
        (env) nadja@debian:~/django/public_sites/kesadjango/people/templates/people$ micro contacts.html
        
        
        ->
        kontaktit  
        
        

![kontaktit](https://github.com/LiljestromNadja/Django_course/assets/118609353/8e6bcefe-57b0-428b-8495-5d7cf49dc536)

Luodaan templates/people/ base.html:  

    (env) nadja@debian:~/django/public_sites/kesadjango/people/templates/people$ micro base.html
    
    <!doctype html>
    <html lang=en>
        <head>
            <title>People</title>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width; initial-scale=1">
        </head>
        <body>
            {% block "content" %}
            <h1>This is base.html in People</h1>
            {% endblock "content" %}
        </body>
    </html>
    
    
Muokataan templates/people contacts.html -tiedostoa:  

    (env) nadja@debian:~/django/public_sites/kesadjango/people/templates/people$ micro contacts.html 
    
    {% extends "people/base.html" %}  
    
    
Testataan:  



![extendsbase](https://github.com/LiljestromNadja/Django_course/assets/118609353/5efc8acb-d66b-4c2f-8fca-19672bdc42c2)  


Tehdään listausta varten muutoksia tiedostoon people/views.py: 

    (env) nadja@debian:~/django/public_sites/kesadjango/people$ micro views.py

    from django.views.generic import TemplateView, ListView
    
    class ContactListView(ListView):
    template_name = 'people/contacts.html'  
    
    
...sekä templates/people/contacts.html -tiedostoon:  

    {% extends "people/base.html" %}


    {% block "content" %}
        {% for object in  object_list %}
            <p>	{{ object }} </p>
        {% endfor %}
    {% endblock "content" %}  
    
    
Testataan localhost:8000/people/ ja saadaan virhe:   

        
    Exception Type: 	ImproperlyConfigured
    Exception Value: 	ContactListView is missing a QuerySet. Define ContactListView.model, ContactListView.queryset, or override ContactListView.get_queryset().  
    
    
Lisätään people/views.py -tiedostoon:  

    (env) nadja@debian:~/django/public_sites/kesadjango/people$ micro views.py 
    
    from django.views.generic import TemplateView, ListView
    from .models import *
    
    class ContactListView(ListView):
    model = Contact
    
..jolloin virheilmoitus vaihtuu:  

    Exception Type: 	TemplateDoesNotExist
    Exception Value: 	people/contact_list.html
    
    
Täydennetään siis vielä äskeiseen people/views.py -tiedostoon tieto, mistä kyseinen template löytyy:  

    from django.views.generic import TemplateView, ListView
    from .models import *

    class ContactListView(ListView):
        template_name = 'people/contacts.html'  <------
        model = Contact
  
  
 
Ja testataan:  

![listaus](https://github.com/LiljestromNadja/Django_course/assets/118609353/67aa5ee7-7959-49c6-bc96-6635cbab3475)

  
---
---
    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)  
[Tero's Classy Django Cheatsheet](https://terokarvinen.com/2023/django-cheatsheet/)  


---
---
--- 

#### Kertausta  

Kansion ja sen sisällön poistaminen:  

    rm -r  poistettavaKansio
    nadja@debian:~/django/public_sites/kesadjango$ rm -r todolist/
    
Tiedoston kopioiminen toiseen kansioon:  

    cp -n kansio/kopioitavaTiedosto kohdekansio  
    nadja@debian:~/django/public_sites/kesadjango$ cp -n kesadjango/urls.py todo    

---

#### Projektin rakenne  

Projekti(sivusto) on hyvä luoda public_sites -nimiseen kansioon, jolloin se on helposti kopioitavissa tuotantoon.  

  Projektin luominen. **Koko** projekti/sivusto:  

    ./manage.py startproject kesadjango
    
  App on osa projektin/sivuston **osa** (joita voi olla useita):  
   
    ./manage.py startapp todo
    ./manage.py startapp henkilot  
    
    
![projektinrakenne_ls](https://github.com/LiljestromNadja/Django_course/assets/118609353/a989b9fd-93e1-4a68-b064-0c7ebc5bda3c)

---

#### URLit  

Projektin(sivuston) urls.py -tiedostoon appsien urlit.  

Appsin urls.py -tiedostoon kyseisen apin urlit. 


huom! luo appsin templateille kansio:  

    nadja@debian:~/django/public_sites/kesadjango/todo/templates/todo$  
    
 eli  
   
    nadja@debian:~/djangokansio/public_sites/projektin nimi/appsin nimi/templates/ appsin nimi$
---    
    
#### Git

[Installing git to Debian 11](https://github.com/LiljestromNadja/DebianLinux/blob/main/GIT/installingGit.md)


    
    

    
    
    
