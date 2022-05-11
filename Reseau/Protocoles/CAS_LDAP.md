# **CAS & LDAP**

## **Sommaire :**

-   [Présentation du CAS & LDAP](#présentation-cas--ldap)
    -   [CAS](#cas)
    -   [LDAP](#ldap)
-   [En Python](#en-python-img-src"httpsmedia2giphycommedia7uxemhqkaimbsnlwz8giphygifcidecf05e47ynx9oqehuntcccv5sdr426zi7e7vtuj24o49ddqlridgiphygifcts"-alt"fleche-pour-indiquer-le-texte"-width"10")

    -   [CAS](#cas)
        -   [Installation](#installation)
        -   [Fichier CAS](#fichier-cas)
        -   [Fichier views.py](#fichier-viewspy-1)
        -   [ID de session](#id-de-session)
    -   [LDAP](#ldap)
        -   [Installation](#installation-1)
        -   [Fichier LDAP](#fichier-ldap)
        -   [Récupération de données](#récupération-de-données)

-   [En PHP]()

<br>


## **Présentation CAS & LDAP** <img src="https://media2.giphy.com/media/7uxeMHQkAIMBSnLWz8/giphy.gif?cid=ecf05e47ynx9oqehuntcccv5sdr426zi7e7vtuj24o49ddql&rid=giphy.gif&ct=s" alt="Fleche pour indiquer le texte" width="10"/>



### CAS <img src="https://media4.giphy.com/media/Wtg8Bmgul1Qxc0otod/giphy.gif?cid=ecf05e47nf8ptvqajx37asbjh0p2uylgta885q0mmvrjc7sa&rid=giphy.gif&ct=s" alt="Fleche pour indiquer le texte" width="10"/>

Le CAS est un système d’authentification unique permettant d’être authentifié sur tous les sites utilisant le même lorsque l’on se connecte à l’un d’entre eux, permettant ainsi d’éviter une nouvelle authentification à chaque changement d’application.





### LDAP <img src="https://media4.giphy.com/media/Wtg8Bmgul1Qxc0otod/giphy.gif?cid=ecf05e47nf8ptvqajx37asbjh0p2uylgta885q0mmvrjc7sa&rid=giphy.gif&ct=s" alt="Fleche pour indiquer le texte" width="10"/>




<br>


## **En Python** <img src="https://media2.giphy.com/media/7uxeMHQkAIMBSnLWz8/giphy.gif?cid=ecf05e47ynx9oqehuntcccv5sdr426zi7e7vtuj24o49ddql&rid=giphy.gif&ct=s" alt="Fleche pour indiquer le texte" width="10"/>

## ***CAS***

### **Installation :**
Pour pouvoir utiliser Flask-CAS avec votre application, vous devez ajouter **Flask-CAS** dans votre fichier **requirements.txt** et relancer votre fichier **build.sh**, ou l’installer en faisant **pip install Flask-CAS** dans votre terminal de commande.



### **Fichier CAS :**
Les imports se font depuis la librairie flask_cas. Pour pouvoir initialiser le CAS, vous aurez besoin de l’importer depuis la librairie. De plus, si le fichier python dans lequel vous initialisez votre CAS est différent de celui où vous avez créé votre app, vous devrez également l’importer.

```python

from flask_cas import CAS
from .app import app #seulement si fichier CAS différent du fichier Flask app

```

Une fois les imports fait, il faut initialiser le CAS sur votre appli Flask et configurer l’url du serveur d’authentification.

```python

CAS(app, (‘/cas’))
app.config[‘CAS_SERVER’]="Adresse du serveur d'authentification" #ne fonctionne pas sans le https://

```
L’intégration du CAS à votre application est terminée. Il ne vous reste plus qu’à l’utiliser sur les routes pour lesquelles une authentification est nécessaire.

### **Fichier views.py :**

Pour utiliser le CAS initialisé précédemment, il faudra importer les éléments CAS et session (celui-ci servira à récupérer l’ID du compte authentifié) depuis le fichier dans lequel il est initialisé. Vous devez également importer login_required depuis flask_cas, qui nous servira à rediriger les utilisateurs sur la page d’authentification du CAS.

```python

from .nomdufichier import CAS, session
from flask_cas import login_required

```

Pour appliquer la redirection à une route, on utilise l’import login required :

```python

@app.route(“/quelquechose”, methods =(“GET”, “POST”))
@login_required #ligne à rajouter pour la redirection
def routequelconque():

```

### **ID de session :**
Vous pouvez récupérer l’identifiant de l’utilisateur authentifié sur l’application en utilisant l’import session, afin de l’utiliser comme filtre pour récupérer d’autres informations, avec LDAP par exemple.

```python

session[‘CAS_USERNAME’]:

```

## ***LDAP***

### **Installation :**
Pour pouvoir utiliser **Flask-CAS** avec votre application, vous devez ajouter **python-ldap** dans votre fichier **requirements.txt** et **relancer** votre fichier **build.sh**, ou l’installer en faisant **pip install python-ldap** dans votre terminal de commande.

### **Fichier LDAP :**
Pour pouvoir initialiser le LDAP, vous aurez besoin d’importer sa librairie. De plus, si le fichier python dans lequel vous initialisez votre LDAP est différent de celui où vous avez créé votre app, vous devrez également l’importer.

```python

import ldap
from .app import app #seulement si fichier LDAP différent du fichier Flask app

```

Une fois les imports fait, il faut initialiser le LDAP sur votre appli Flask avec l’url du serveur, et créer une liaison avec le compte que vous avez à votre disposition pour utiliser LDAP.

```python

votreldap = ldap.initialize("L'adresse de l'annuaire LDAP")
votreldap.simple_bind_s(’cn=nomdecompte,dc="Mettre le nom de domaine",dc=fr’,
‘motdepassecompte’)

```

L’intégration du LDAP à votre application est terminée. Vous pouvez maintenant l’utiliser pour récupérer des informations sur un utilisateur à partir de son identifiant (récupéré avec CAS par exemple).

### **Récupération de données :**
La récupération de données s’effectue grâce à la méthode “search_s”. Pour l’utiliser, il faut d’abord importer la librairie ldap et le ldap que vous avez initialisé si vous effectuez la recherche dans un fichier différent.

```python

from .fichierldap import ldap, votreldap

```

Une fois l’import effectué, vous pouvez utiliser la méthode comme montré ci-dessous (commandes servant à récupérer le nom/prénom et le mail de l’utilisateur en fonction de son ID de session CAS).

```python

nom = str(votreldap.search_s(‘dc="Mettre le nom de domaine",dc=fr’ #domaine du ldap,
ldap.SCOPE_SUBTREE #lieu de recherche, 
‘uid=’+session[‘CAS_USERNAME’] #filtre à partir de l’id utilisateur CAS, 
[‘displayName’] #clé de la donnée)

mail = str(votreldap.search_s(‘dc=univ-orleans,dc=fr’, ldap.SCOPE_SUBTREE, ‘uid=’+session[‘CAS_USERNAME’], [‘displayName’])

```

Si vous avez des questions, n'hésitez pas à me contacter sur le discord de ***Tech2Tech*** Ou par email : mathieujdscontact@gmail.com
