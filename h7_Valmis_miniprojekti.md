## h7_Valmis_miniprojekti  

### Tehtävänanto  

a) Palauta linkki miniprojektisi etusivuun  

Vinkit  

Voit palauttaa linkin esimerkiksi miniprojektin Github-varaston / kansion README-tiedostoon wepissä  
Etusivulla olisi hyvä olla taitoksen yläpuolella  
Tarkoitus - mitä ohjelma tekee  
Ruutukaappaus (ohjelman pääasiallisesta käyttötarkoituksesta - tämä on kuvallisesti "tarkoitus - mitä ohjelmalla tekee")  
Lisenssi (suosittelen GNU General Public License, version 3; lisenssin saa vapaasti valita)  
Demon linkki  
Linkki lähdekoodiin (Github/Gitlab näyttää tämän ihan automaattisestikin)  




---

### a) Miniprojekti 


#### [Projektin koodit >>>](https://github.com/LiljestromNadja/djangoSummer)  
#### [nadjaliljestrom.website >>>](http://www.nadjaliljestrom.website)  

**Sovelluksen tarkoitus:**   
Digitaaliset heippalaput taloyhtiön sisäisen kommunikoinnin ja tiedottamisen edistämiseen.  
Ideana on että sovellukseen jätetyt ilmoitukset ja kysymykset ovat kaikkien saatavilla (kyseisessä taloyhtiössä).  
Sovelluksen käyttö edellyttää käyttäjätunnusta, käyttäjätasoina user ja admin. Esimerkkikäyttäjiä ovat osakkaat/asukkaat, talonmies ja isännöitsijä.  



**Sovelluksen tämänhetkinen tilanne:**  

App: Notes  
Tarkoitus: Heippalaput  
One-to-many: Yksi henkilö - monta lappua (User&Note) 

Lomakekentät:  
Category:  name  
Note: subject, message , category(FK), owner(FK User)  

Käyttöoikeudet Admin:    
Laput: kaikkien julkaisujen katselu, oman julkaisun lisääminen, muokkaaminen ja poistaminen, Admin-käyttöliittymässä kaikkien julkaisujen muokkaaminen ja poistaminen   
Käyttäjätilit: katselu, lisääminen, muokkaaminen, poistaminen  

Käyttöoikeudet User:   
Laput: kaikkien julkaisujen katselu, oman julkaisun lisääminen, muokkaaminen ja poistaminen  
Käyttäjätilit: -  

Rekisteröitymätön käyttäjä: Toistaiseksi admin luo käyttäjätilin pääsääntöisesti user-oikeuksilla rekisteröitymättömille käyttäjille.  






**Sovelluksen jatkokehitys:**  


Tavoitteena lisätä sovellukseen:  
- Ilmoituksiin vastaaminen: mahdollisuus vastata toisten käyttäjien jättämiin ilmoituksiin.  

App: Notes  
Tarkoitus: Heippalaput  
One-to-many: Yksi henkilö - monta lappua   
One to many: Yksi lappu - monta kommenttia  

Lomakekentät:  
Category:  name  
Note: subject, message , category(FK), owner(FK User)  
Comment: message, owner(FK)  

Käyttöoikeudet Admin  
Laput: katselu, lisääminen, muokkaaminen, poistaminen 
Kommentit: katselu, lisääminen, muokkaaminen, poistaminen
Käyttäjätilit: katselu, lisääminen, muokkaaminen, poistaminen   

Käyttöoikeudet User:   
Laput: katselu, lisääminen, muokkaaminen poistaminen
Kommentit: kaikkien katselu, omien lisääminen, muokkaaminen, poistaminen
Käyttäjätilit: oman käyttäjätilin muokkaaminen: salasana     

Rekisteröitymätön käyttäjä: voi itse luoda user-tasoisen tilin  

---

**Login:**  

![login](https://github.com/LiljestromNadja/Django_course/assets/118609353/9e5a4a4c-e181-473e-8f86-2b065050cd98)

![welcome](https://github.com/LiljestromNadja/Django_course/assets/118609353/d9645150-4eb9-469c-aa80-17a8308f755a)

---  

**View:**  

![view](https://github.com/LiljestromNadja/Django_course/assets/118609353/06a08fe8-9e0f-48d1-a8b2-051a105c99ca)
![view categories](https://github.com/LiljestromNadja/Django_course/assets/118609353/b1067291-a378-443b-a3c0-6325724ebfd3)

---  

**Create:**  

![create](https://github.com/LiljestromNadja/Django_course/assets/118609353/a9a1b7e2-8688-49f3-bb17-c590226a18ae)  

---  

**Delete:**  


![delete](https://github.com/LiljestromNadja/Django_course/assets/118609353/9f456216-faec-4bea-aa89-bd163f1f94f0)


---

**Admin:**  

![admin](https://github.com/LiljestromNadja/Django_course/assets/118609353/7598ce60-fe61-4822-8c45-64474a89c7d4)


*** 
---
---
    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)  
[Tero's Classy Django Cheatsheet](https://terokarvinen.com/2023/django-cheatsheet/)  




