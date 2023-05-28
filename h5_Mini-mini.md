## h5_Mini-mini  

### Tehtävänanto  


a) Kuka tekee mitä & miniversio. Mitä projektillasi tehdään? Kuka tekee? Tee tosi pieni ja simppeli versio miniprojektistasi.  
b) Vapaaehtoinen: Tee miniprojektisi pidemmälle, kohti valmista.  




---

### a) Kuka tekee mitä & miniversio. Mitä projektillasi tehdään? Kuka tekee? Tee tosi pieni ja simppeli versio miniprojektistasi.  


### [>>>Projektin koodit<<<](https://github.com/LiljestromNadja/djangoSummer)  

Digitaaliset heippalaput taloyhtiön sisäisen kommunikoinnin ja tiedottamisen edistämiseen.  
Ideana on että sovellukseen jätetyt ilmoitukset ja kysymykset ovat kaikkien saatavilla (kyseisessä taloyhtiössä).  
Sovelluksen käyttö edellyttää käyttäjätunnusta, käyttäjätasoina user ja admin. Esimerkkikäyttäjiä ovat osakkaat/asukkaat, talonmies ja isännöitsijä.  



**Sovelluksen tämänhetkinen tilanne:**  

App: Notes  
Tarkoitus: Heippalaput  
One-to-many: Yksi henkilö - monta lappua  

Lomakekentät:  
Person:  name, apartment  
Note: subject, message , person(FK)  

Käyttöoikeudet:  
Admin:  
Laput: katselu, lisääminen, muokkaaminen poistaminen   
Käyttäjätilit: katselu, lisääminen, muokkaaminen, poistaminen  

User:   
Laput: katselu, lisääminen, muokkaaminen poistaminen   

Käyttäjätilit: -  
Rekisteröitymätön käyttäjä: voi luoda user-tasoisen tilin  

**Sovellukseen lisättävää:**  


Tavoitteena lisätä sovellukseen:  
- Ilmoituksiin vastaaminen: mahdollisuus vastata toisten käyttäjien jättämiin ilmoituksiin.  
- Lappujen muokkaaminen/poistaminen -> vain omia lappuja on mahdollista muokata/poistaa.  
- User == person


App: Notes  
Tarkoitus: Heippalaput  
One-to-many: Yksi henkilö - monta lappua   
One to many: Yksi lappu - monta kommenttia  

Lomakekentät:  
Person:  name, apartment  
Note: subject, message , person(FK)  
Comment: message, person(FK)  

Käyttöoikeudet:  
Admin:  
Laput: katselu, lisääminen, muokkaaminen poistaminen   
Käyttäjätilit: katselu, lisääminen, muokkaaminen, poistaminen   

User:   
Laput: katselu, lisääminen, muokkaaminen poistaminen   

Käyttäjätilit: oman käyttäjätilin muokkaaminen: salasana     
Rekisteröitymätön käyttäjä: voi luoda user-tasoisen tilin  




*** 
---
---
    
#### Lähteet ja materiaalit:  

[Tero Karvinen - Python Web - Idea to Production - 2023](https://terokarvinen.com/2023/python-web-idea-to-production/)  
[Tero's Classy Django Cheatsheet](https://terokarvinen.com/2023/django-cheatsheet/)  




