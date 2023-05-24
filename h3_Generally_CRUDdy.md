## h3 Generally CRUDDY  

### Tehtävänanto  

x) Kertaa CRUD muistilapusta [Karvinen 2023: Tero's Classy Django Cheatsheet](https://terokarvinen.com/2023/django-cheatsheet/)
(Opettelematonta osaa ei tarvitse lukea vielä. Tiivistelmää ei tarvita, eli tästä lukutehtävästä ei palauteta mitään.)  

a) Tee alusta lähtien uusi CRUD-ohjelma käyttäen Djangon yleisiä luokkanäkymiä (Class Based Generic Views).
(Sellainen, mitä et ole vielä tehtnyt. Jotain muuta kuin todo-lista, crm tai linnut. Ei tarvitse tehdä raporttia, lähdekoodi ja README.md:ssä kuvaus, mitä ohjelma tekee ja ruutukaappaus lopputuloksesta. Voit laittaa projektin omaksi kansiokseen esimerkiksi Github- tai Gitlab-varastoon.)  

b) Vapaaehtoinen, vaikea: Tee riippuvuus kahden luokan välille.
Kuten aina, palauta Laksuun klo 23 mennessä ja arvioi kaksi.  

Vinkit:  

Lisätietoa Djangon dokumentaatiosta: [Built-in class-based generic views](https://docs.djangoproject.com/en/4.2/topics/class-based-views/generic-display/)


---

### a) Tee alusta lähtien uusi CRUD-ohjelma käyttäen Djangon yleisiä luokkanäkymiä (Class Based Generic Views).
(Sellainen, mitä et ole vielä tehtnyt. Jotain muuta kuin todo-lista, crm tai linnut. Ei tarvitse tehdä raporttia, lähdekoodi ja README.md:ssä kuvaus, mitä ohjelma tekee ja ruutukaappaus lopputuloksesta. Voit laittaa projektin omaksi kansiokseen esimerkiksi Github- tai Gitlab-varastoon.)  

### [>>>Projektin koodit<<<](https://github.com/LiljestromNadja/djangoSummer)
Lisätty app products + CRUD. 

Create

![createproduct](https://github.com/LiljestromNadja/Django_course/assets/118609353/d8f9f86b-1d3d-4c0b-acdc-3791c328052a)

Update

![editproduct](https://github.com/LiljestromNadja/Django_course/assets/118609353/6dbe2ece-a04a-4a19-aa5f-f9f65973e264)

Delete

![deleteproduct](https://github.com/LiljestromNadja/Django_course/assets/118609353/58203fae-893b-4221-b18f-93c4f00834e9)






























---
### Uusi app products + CRUD
#### Uusi app:  

Aktivoidaan kehitysympäristö: 

    nadja@debian:~/django$ ls
    env  public_sites  requirements.txt
    nadja@debian:~/django$ source env/bin/activate  
    
Kehityspalvelin:  
    
    (env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py runserver  
    
App:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py startapp products  
    
kesadjango/settings.py:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro kesadjango/settings.py 
    
    
    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',    
    'todo', #add apps here
    'people',
    'products'
    
    ]
    

products/models.py:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro products/models.py
    
    from django.db import models

    class Product(models.Model):
      name = models.CharField(max_length=100)
      description = models.TextField(max_length=500)
      price = models.FloatField(blank=True,null=True)

      
      def __str__(self):
        return self.name

Päivitetään kantaan:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ ./manage.py makemigrations ; ./manage.py migrate

products/admin.py:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro products/admin.py 
    
    from django.contrib import admin
    from .models import *

    admin.site.register(Product) 
    
    
kesadjango/urls.py:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro kesadjango/urls.py 
    
    from django.contrib import admin
    from django.urls import path, include 


    urlpatterns = [
      path('admin/', admin.site.urls), #admin-urlit
      path('todo/', include('todo.urls')), 
      path('people/', include('people.urls')),

      path("", include('products.urls'))
   
    ]  
    
    
Luodaan products/urls.py:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro products/urls.py  
    
    from django.urls import path
    from .views import *


    urlpatterns = [

        path('', BaseView.as_view())

        ]
    
products/views.py:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro products/views.py

    from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
    from .models import *

    class BaseView(TemplateView):
        template_name = 'products/base.html'
        
        
Luodaan kansiot products/templates/ ja products/templates/products. Sinne base.html:  

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
    
**Read:**    
Päivitetään product/urls.py:  

    from django.urls import path
    from .views import *


    urlpatterns = [

        path('', ProductListView.as_view())

    ]  
    
    
    
.. ja product/views.py:  

    from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
    from .models import *

    class BaseView(TemplateView): 
        template_name = 'products/base.html'

    class ProductListView(ListView):
        model = Product

  

Luodaan templates/product/product_list.html:  

    (env) nadja@debian:~/django/public_sites/kesadjango$ micro products/templates/products/product_list.html
    
    {% extends "products/base.html" %}


    {% block "content" %}
      <h3>Products</h3>

      <p>
        <a href="product/add"> Add product </a>
      </p>

      {% for object in  object_list %}

        <p>	Product: 
          <a href='product/{{ object.pk }}/edit'>
          {{ object }} 
          </a>

          Description: 
          {{ object.description }}
          Price:
          {% if object.price %} 
          {{ object.price }}
          {% endif %} 


        </p>

      {% endfor %}
    {% endblock "content" %}



**Update:**  

Päivitetään products/urls.py:  

    from django.urls import path
    from .views import *


    urlpatterns = [

        path('', ProductListView.as_view()),

        path('product/<int:pk>/edit', ProductUpdateView.as_view()),

    ]

Päivitetään products/views.py:  

    from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
    from .models import *

    class BaseView(TemplateView): 
        template_name = 'products/base.html'

    class ProductListView(ListView):
        model = Product

    class ProductUpdateView(UpdateView):
        model = Product
        fields = ["name","description", "price"]
        success_url = '/'




Luodaan templates/products/product_form.html:  

    
    
    (env) nadja@debian:~/django/public_sites/kesadjango$ micro products/templates/products/product_form.html


    {% extends "products/base.html" %}


    {% block "content" %}
      <a href="/">Back to list</a>
      <form method="post">
        {% csrf_token %} 
        {{form.as_p}}
        <input type=submit value="Save changes">
        {% if object  %}
          <p><a href="/product/{{ object.pk }}/delete">Delete</a>
        {% endif  %}
      </form>	
    {% endblock "content" %}

**Create:**  
Päivitetään products/urls.py:  

    from django.urls import path
    from .views import *


    urlpatterns = [

        path('', ProductListView.as_view()),

        path('product/<int:pk>/edit', ProductUpdateView.as_view()),
        path('product/add', ProductCreateView.as_view()),

    ]



Päivitetään products/views.py:  

    from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
    from .models import *

    class BaseView(TemplateView): 
        template_name = 'products/base.html'

    class ProductListView(ListView):
        model = Product

    class ProductUpdateView(UpdateView):
        model = Product
        fields = ["name","description", "price"]
        success_url = '/'

    class ProductCreateView(CreateView):
        model = Product
        fields = ["name","description", "price"]
        success_url = '/'
        
        

**Delete:**  

products/urls.py:  

    from django.urls import path
    from .views import *


    urlpatterns = [

        path('', ProductListView.as_view()),

        path('product/<int:pk>/edit', ProductUpdateView.as_view()),
        path('product/add', ProductCreateView.as_view()),
        path('product/<int:pk>/delete', ProductDeleteView.as_view()),


    ]
    
    
products/views.py:  

    from django.views.generic import TemplateView, ListView, UpdateView, CreateView, DeleteView
    from .models import *

    class BaseView(TemplateView): 
        template_name = 'products/base.html'

    class ProductListView(ListView):
        model = Product

    class ProductUpdateView(UpdateView):
        model = Product
        fields = ["name","description", "price"]
        success_url = '/'

    class ProductCreateView(CreateView):
        model = Product
        fields = ["name","description", "price"]
        success_url = '/'

    class ProductDeleteView(DeleteView):
        model = Product
        success_url = "/"


Luodaan templates/products/product_confirm_delete.html

    


---
---
    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)  
[Tero's Classy Django Cheatsheet](https://terokarvinen.com/2023/django-cheatsheet/)  

