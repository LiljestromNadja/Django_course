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


Uusi app: Notes  
Tarkoitus: Heippalaput  
One-to-many: Yksi henkilö - monta lappua  

Lomakekentät:  
Person:  name, apartment  
Note: subject, message , person(FK)   

*** 

**Listaus ja heippalapun lisääminen:**  

<br>

![listaus_ja_add](https://github.com/LiljestromNadja/Django_course/assets/118609353/7987d649-dcd4-4c76-b0e9-af2c66b0724d)


**Muokkaaminen:**  

![edit](https://github.com/LiljestromNadja/Django_course/assets/118609353/6377ddf0-d627-4179-9744-d4f57f73002e)  

**Poistaminen:**  

![delete](https://github.com/LiljestromNadja/Django_course/assets/118609353/5f088b5e-dbcc-40c8-bd22-018398d60027)  


**Admin:**  

![admin](https://github.com/LiljestromNadja/Django_course/assets/118609353/68c80736-d6d3-4e79-be98-1cb0f758fd41)































---
---
### My notes from creating notes..

**Luodaan app nimeltä notes**

        (env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py startapp notes 

        (env) nadja@debian:~/django/public_sites/kesadjango$ micro kesadjango/settings.py  

**Päivitetään notes/models.py jossa myös määritellään one-to-many -suhde:**  


        (env) nadja@debian:~/django/public_sites/kesadjango$ micro notes/models.py  

        ->

        from django.db import models


        class Person(models.Model):
            name = models.CharField(max_length=100)
            apartment = models.CharField(max_length=100, null=True)



            def __str__(self):
                return f" {self.name} {self.apartment}"

        class Note(models.Model):
            subject = models.CharField(max_length=100)
            message = models.TextField(max_length=500)
            person = models.ForeignKey(Person, on_delete=models.CASCADE, null=True, blank=True)

            def __str__(self):
                return f" {self.subject} {self.person}"

                #return self.subject  
  
 
**Laitetaan kantaan:**  
               

(env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py makemigrations ; ./manage.py migrate  

**"Register your models here:"  

        (env) nadja@debian:~/django/public_sites/kesadjango$ micro notes/admin.py  
        ->
        from django.contrib import admin
        from .models import *

        admin.site.register(Person)
        admin.site.register(Note)


**Crud:**

Projektin kesadjango/urls.py:  

        nadja@debian:~/django/public_sites/kesadjango$ micro kesadjango/urls.py  

        ->
        from django.contrib import admin
        from django.urls import path, include 

        urlpatterns = [
            path('admin/', admin.site.urls), 
            path("", include('notes.urls')),

        ]  


**Luodaan:**  

        nadja@debian:~/django/public_sites/kesadjango$ micro notes/urls.py 

        from django.urls import path
        from .views import *


        urlpatterns = [

            path('', NoteBaseView.as_view()),

        #    path('note/<int:pk>/edit', NoteUpdateView.as_view()),
        #    path('note/add', NoteCreateView.as_view()),
        #    path('note/<int:pk>/delete', NoteDeleteView.as_view()),


        ]


**notes/views.py:**  

        from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
        from .models import *

        class NoteBaseView(TemplateView):
            template_name = 'notes/base.html'  
    
    
    
**Luodaan notes/templates ja notes/templates/notes:**   
    

        (env) nadja@debian:~/django/public_sites/kesadjango/notes$ mkdir templates  
        (env) nadja@debian:~/django/public_sites/kesadjango/notes/templates$ mkdir notes  

**Sinne base.html:**

        <!doctype html>
        <html lang=en>
          <head>
            <title>Products</title>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width; initial-scale=1">
          </head>
          <body>
           {% block "content" %}
             <h1>This is base.html in Products</h1>
           {% endblock "content" %}
          </body>
        </html>  


**Listaus:**

        from django.urls import path
        from .views import *


        urlpatterns = [

**Päivitetään notes/urls.py:** 

        from django.urls import path
        from .views import *


        urlpatterns = [

            path('', NoteListView.as_view()),

        #    path('note/<int:pk>/edit', NoteUpdateView.as_view()),
        #    path('note/add', NoteCreateView.as_view()),
        #    path('note/<int:pk>/delete', NoteDeleteView.as_view()),


        ]



**Päivitetään notes/views.py:**

        from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
        from .models import *

        class NoteBaseView(TemplateView):
            template_name = 'notes/base.html'

        class NoteListView(ListView):
            model = Note


**Luodaan templates/notes/note_list.html:**  

        {% extends "notes/base.html" %}


        {% block "content" %}
          <h3>Heippalaput</h3>

          <p>
            <a href="note/add"> Add note </a>
          </p>

          {% for object in  object_list %}

            <p>	Note subject: 
              <a href='note/{{ object.pk }}/edit'>
              {{ object.subject }} 
              </a>

              Publisher: 
              {{ object.person }} 

              Message:
              {{ object.message }}      


              Apartment: 
              {{ object.person.apartment }}


            </p>

          {% endfor %}
        {% endblock "content" %}


**Update:**  

        Päivitetään notes/urls.py:  

        from django.urls import path
        from .views import *


        urlpatterns = [

            path('', NoteListView.as_view()),

            path('note/<int:pk>/edit', NoteUpdateView.as_view()),
        #    path('note/add', NoteCreateView.as_view()),
        #    path('note/<int:pk>/delete', NoteDeleteView.as_view()),


        ]


**Päivitetään notes/views.py:**  

        from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
        from .models import *

        class NoteBaseView(TemplateView):
            template_name = 'notes/base.html'

        class NoteListView(ListView):
            model = Note

        class NoteUpdateView(UpdateView):
            model = Note
            fields = ["subject","message", "person"]
            success_url = '/'  



**Luodaan templates/notes/note_form.html:**   


        {% extends "notes/base.html" %}


        {% block "content" %}
          <a href="/">Back to list</a>
          <form method="post">
            {% csrf_token %} 
            {{form.as_p}}
            <input type=submit value="Save changes">
            {% if object  %}
              <p><a href="/note/{{ object.pk }}/delete">Delete</a>
            {% endif  %}
          </form>	
        {% endblock "content" %}


**Create:**  

**Päivitetään notes/urls.py:**   

        from django.urls import path
        from .views import *


        urlpatterns = [

            path('', NoteListView.as_view()),

            path('note/<int:pk>/edit', NoteUpdateView.as_view()),
            path('note/add', NoteCreateView.as_view()),
        #    path('note/<int:pk>/delete', NoteDeleteView.as_view()),


        ]


**Päivitetään notes/views.py:** 

        from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
        from .models import *

        class NoteBaseView(TemplateView):
            template_name = 'notes/base.html'

        class NoteListView(ListView):
            model = Note

        class NoteUpdateView(UpdateView):
            model = Note
            fields = ["subject","message", "person"]
            success_url = '/'

        class NoteCreateView(CreateView):
            model = Note
            fields = ["subject","message", "person"]
            success_url = '/'

    
**Delete:**  

**notes/urls.py:**   


        from django.urls import path
        from .views import *


        urlpatterns = [

            path('', NoteListView.as_view()),

            path('note/<int:pk>/edit', NoteUpdateView.as_view()),
            path('note/add', NoteCreateView.as_view()),
            path('note/<int:pk>/delete', NoteDeleteView.as_view()),


        ]


**notes/views.py:**   

        from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
        from .models import *

        class NoteBaseView(TemplateView):
            template_name = 'notes/base.html'

        class NoteListView(ListView):
            model = Note

        class NoteUpdateView(UpdateView):
            model = Note
            fields = ["subject","message", "person"]
            success_url = '/'

        class NoteCreateView(CreateView):
            model = Note
            fields = ["subject","message", "person"]
            success_url = '/'

        class NoteDeleteView(DeleteView):
            model = Note
            success_url = "/"


**Luodaan templates/notes/note_confirm_delete.html:** 

        {% extends "notes/base.html" %}


        {% block "content" %}
            <a href="/">Back to list</a>
            <form method="post">
                {% csrf_token %} 
                This cannot be undone. Are you sure to delete <b> {{object.name}}</b>? 
                <p><a href="/note/{{ object.pk  }}/edit">Cancel<a>
                <input type=submit value="Yes, I am sure">

            </form>	
        {% endblock "content" %}





















































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

**Hyödyllistä:**  

[Django 4.2 Documentation Working with forms](https://docs.djangoproject.com/en/4.2/topics/forms/)  
[Django 4.2 Documentation Model field reference](https://docs.djangoproject.com/en/4.2/ref/models/fields/#datetimefield)



