# Muotillista
## Tehtävänanto

x) Silmäile: Karvinen 2023: Tero's Classy Django Cheatsheet  
(Vain siltä osin kuin on tähän asti opeteltu. Tutut asiat ovat artikkelin alussa, suurin osa on vielä opettelematta ja opettelematonta osaa ei tarvitse lukea vielä. Tiivistelmää ei tarvita, eli tästä lukutehtävästä ei palauteta mitään.)  

a) Tee Django-ohjelma, joka listaa kaikki tietueet etusivulla (ListView). Palauta lähdekoodi ja ruutukaappaus lopputuloksesta.
(Jotain muuta kuin todo-lista, crm tai linnut. Ei tarvitse tehdä raporttia, lähdekoodi ja README.md:ssä kuvaus, mitä ohjelma tekee ja ruutukaappaus lopputuloksesta. Voit laittaa projektin omaksi kansiokseen esimerkiksi Github- tai Gitlab-varastoon.)  

b) Vapaaehtoinen: Tee käyttöliittymä uusien tietueiden lisäämiseen (CreateView)  
c) Vapaaehtoinen: Tee käyttöliittymä tietueiden muokaamiseen (UpdateView)  
d) Vapaaehtoinen: Tee käyttöliittymä tietueiden poistamiseen - ja varmista, haluatko todella poistaa (DeleteView)  

Kuten aina, palauta Laksuun klo 23 mennessä ja arvioi kaksi.  


---
<!--
### tehtäväotsikko  



#### alaotsikko
 -->
 
 
---
### Tunnilla / päivä 2  

#### Kertausta  

Kansion ja sen sisällön poistaminen:  

    rm -r  poistettavaKansio
    nadja@debian:~/django/public_sites/kesadjango$ rm -r todolist/
    
Tiedoston kopioiminen toiseen kansioon:  

    cp -n kansio/kopioitavaTiedosto kohdekansio  
    nadja@debian:~/django/public_sites/kesadjango$ cp -n kesadjango/urls.py todo    

    
#### Projektin rakenne  

Projekti(sivusto) on hyvä luoda public_sites -nimiseen kansioon, jolloin se on helposti kopioitavissa tuotantoon.  

  Projektin luominen. **Koko** projekti/sivusto:  

    ./manage.py startproject kesadjango
    
  App on osa projektin/sivuston **osa** (joita voi olla useita):  
   
    ./manage.py startapp todo
    ./manage.py startapp henkilot  
    
    
![projektinrakenne_ls](https://github.com/LiljestromNadja/Django_course/assets/118609353/a989b9fd-93e1-4a68-b064-0c7ebc5bda3c)

    
#### URLit  

Projektin(sivuston) urls.py -tiedostoon appsien urlit.  

Appsin urls.py -tiedostoon kyseisen apin urlit. 


huom! luo appsin templateille kansio:  

    nadja@debian:~/django/public_sites/kesadjango/todo/templates/todo$  
    
 eli  
   
    nadja@debian:~/djangokansio/public_sites/projektin nimi/appsin nimi/templates/ appsin nimi$


    
    

    
    
    
    
    
    
    
    
    
    
    
---
    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)
