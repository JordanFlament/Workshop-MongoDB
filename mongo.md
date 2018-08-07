#Mongo DB

##Qu'est-ce que c'est ?

MongoDB est une base de données NoSQL (Not Only SQL) de type Documents qui manipule les fichiers en mélangeant la méthode ACID et CRUD.

*La méthode ACID qui consiste à ce que une transaction soit composée de plusieurs, et qu'un résultat ne sera donné que, et seulement si, la transaction peut être faites completement, ou pas du tout;*

	*Atomicity exige que chaque transaction soit exécutée dans son intégralité ou échoue sans qu'aucune modification ne soit appliquée.
	*La cohérence exige que la base de données ne passe que d'un état valide au suivant, sans points intermédiaires.
	*L'isolement exige que si les transactions sont exécutées simultanément, le résultat est équivalent à leur exécution en série. Une transaction ne peut pas voir le résultat partiel de l'application d'un autre.
	*La durabilité signifie que le résultat d'une transaction validée est permanent, méme si la base de données tombe en panne immédiatement ou en cas de panne de courant. 

*Et la méthode CRUD, qui consiste à créer, lire, modifié et supprimer une donnée (create, read, update, delete).*
 
La différence avec une base de données relationnelle (comme MySQL) C'est qu'il n'y a pas de shéma, ce qui rend la manipulation de documents, plus aisé.
Ce type de base de données est utilisé pour des listes, des imbrications, des fichiés,...
Elle est le plus utilisé par les web-developper utilisant NoSQL car les requétes sont assez ressemblante au requêtes des SGBDR.

**Les bdd NoSQL se compose en 4 grande famille, utilisé selon la manipulation de fichier la plus pertinante;**

*Clé / valeur (système utilisé pour les application en temps réel).
	Ces technologie utilise de système ; Redis, Memcached, Azure Cosmos DB,	SimpleDB.

*Documentation (utilisé pour y déposé des listes, des imbrications, des types, des fichiers ...).
	Ces technologie utilise de système ; MongoDB, CouchBase, DynamoDB, Cassandra.

*Colonnes (ce système fonctionne en colonne et non en ligne comme les sgbdr traditionnelle ce qui permet de ciblé les informations recherchées dans une bdd en évitant d'avoir des informations inutiles. 
Ce système est utilisé pour les calcules analytique).
	Ces technologie utilise de système ; BigTable, HBase, Spark SQL, Elasticsearch.

*Graphique (Ce système est utilisé pour le bigdata, et pensé de manière à ce que les éléments ( les neouds, les liens et des propriétés sur ces noeuds et ces liens) n'entre pas en corélations.).
	Ces technologie utilise de système ; Neo4j, OrientDB, FlockDB.

Dans ce WorkShop je vais vous apprendre les manipulation de bases du système type Documents, Si vous êtes interressé d'en apprendre plus, je vous mettrais des liens pour en apprendre d'avantage.
Mais avant de commencé, il est important de souligné que comme en sgbdr, il est préférable de mettre tout les type de documents dans les mêmes collections(tables). 

-----------------

###Installer MongoDB;

**Windows**
Télécharger le fichier à cette adresse et exécutez le.

https://www.mongodb.com/download-center#community

Ensuite créer un fichier *data* dans le disque *C* et créer un dossier *db* dans le dossier *data*.
Votre ordinateur s'occupera de faire le reste.

Lorsque l'installation sera faite, lancé dans l'ordre ;
* mongod.
* mongo.

**Ubuntu**
Je vous invite à suivre les instructions de cette page ;
https://computingforgeeks.com/how-to-install-latest-mongodb-on-ubuntu-18-04-ubuntu-16-04/

Lorsque l'installation sera faite, lancé dans l'ordre ;
* mongo.

**Mac**
https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/

Lorsque l'installation sera faite, lancé dans l'ordre ;
* mongod.
* mongo.

-----------------

Lorsque vous arriverez dans le terminal de mongo, vous devrez vous sentir un peu perdu si tout ceci est nouveau pour vous.

Si vous faite "show dbs" (commande servant à afficher les bases de données disponnible).
Vous verez que vous avez les bdd admin, config et local.

Afin de ne pas créer de problème, nous allons créer une nouvelle bdd comme ceci;

*use new_base*
La commande **use** permet de créer une nouvelle dbb, ainsi que de se déplacer dans une bdd.
Cette nouvelle bdd ne sera prise en compte par **show dbs** que lorsqu'une collection sera ajouté ainsi qu'un document.

Pour ce faire, nous entrerons ceci, ce qui créera une nouvelle collection avec un documents ;

*db.collection_name.insert({clé:"valeur"})*

Vous pouvez vérifié que votre document a été enregistré avec la methode **find**;

*db.collection_name.find()*

NoSQL n'ayant pas de shema, vous n'êtes dans aucunes, et en même temps dans toutes les tables.
Ce qui signifie que si vous avez entré ;

*db.collection_a.insert({name:"objet1"})*

Vous pouvez entré ceci, sans avoir à vous demandez comment aller dans cette table ;

*db.collection_b.insert({name:"objet1"})*

Celà aura créer deux collections différentes avec chacune disposant d'un document.

**MongoDB acccepte le langage JS, ce qui permet de s'y retrouver un peu dans les requêtes.**

Ce qui est assez pratique pour ajouter des documents en masse.
Pour ceci nous créeront une **boucle** et nous lui précisont qu'il faut placé les documents dans une _nouvelle collection_.

*for(i=1; i<50; i++){let objet = "quelque_chose"; db.obj.insert({compteur:i, quoi:objet})}*

En lançant la commande ...

*db.obj.find()*

... une liste de document apparaitra devant vous, avec l'id, un compteur incrémenté (qui nous aidera à retrouver un document plus facilement lors d'une recherche précise) et l'objet.

>{ "_id" : ObjectId("5b6731ab7713680df162c69b"), "compteur" : 1, "quoi" : "quelque_chose" }
>{ "_id" : ObjectId("5b6731ab7713680df162c69c"), "compteur" : 2, "quoi" : "quelque_chose" }
>{ "_id" : ObjectId("5b6731ab7713680df162c69d"), "compteur" : 3, "quoi" : "quelque_chose" }
>{ "_id" : ObjectId("5b6731ab7713680df162c69e"), "compteur" : 4, "quoi" : "quelque_chose" }
>{ "_id" : ObjectId("5b6731ab7713680df162c69f"), "compteur" : 5, "quoi" : "quelque_chose" }
>...

La méthode find n'étant pas prévue pour les grosses recherches, elle n'affichera que 20 documents (maximum) à la fois. 
Pour en afficher plus il faudra faire "it" pour afficher les 20 suivant.

Pour une recherche précise, il suffit d'écrire en paramètre de la methode find, une caractéristique du document rechercher.

Ex : *db.obj.find({compteur:18})*

Ce qui vous renvérra ; 

>{ "_id" : ObjectId("5b6731ab7713680df162c6ac"), "compteur" : 18, "quoi" : "quelques_chose" } 

Vous pouvez modifier les valeurs de vos document en utilisant la méthode **update** et la méthode __$set__.

*db.obj.update({"quoi":"quelque_chose"}, {$set : {"quoi":"objet1"}})*

Cependant cela ne modifiera que le premier document dont la clé "quoi" avait pour valeur "quelque_chose".
Il vous est possible de modifié toute les valeurs aillant une clé/valeur identique avec la fonction **multi**.

*db.obj.update({"quoi":"quelque_chose"}, {$set : {"quoi":"objet1"}}, {multi:true})*

Aussi, vous pouvez ciblé en masse vos modification.
Ici par exemple, vous ciblé tout les documents dont le compteur est plus grand ou égal à 5, et plus petit ou égal à 35.

*db.obj.update({compteur: {$gte:5, $lte:35}}, {$set: {"quoi":"objet1"}}, {multi:true})*

A savoir que $set, ne sert pas uniquement à modifier la valeur d'une clé, mais qu'elle peut également ajouter une nouvelle clé/valeur.

*db.obj.update({compteur: {$gte:5, $lte:35}}, {$set: {"quand":"qui sait ?"}}, {multi:true})*

Vous pouvez également supprimer certaines clé/valeur avec __$unset__ ;

*db.obj.update({compteur: {$gte:5, $lte:35}}, {$unset: {"quand":"qui sait ?"}}, {multi:true})*

Comme, aussi, supprimer plusieurs ligne d'un coup ;

*db.obj.remove({compteur: {$gte:5, $lte:35}})*

Vous voulez savoir combien de documents votre contient votre collection ?

La méthode **count** est prévue pour ça.

*db.obj.count()*

Ce qui vous renverra directement le nombre 49, le nombre d'objet entré avec la boucle.
Pourquoi 49 alors que nous avions précisé 50 dans la boucle ? Et bien parce que dans notre boucle nous précisons que i = 1, la boucle commence donc en supposant que 1 existe déja et le déduis de l'incrémentation.

Il est possible de filtré les requetes avec des opérateurs.

Trouver les objets dont le compteur est plus grand que 15 et plus petit que 20 ?

*db.obj.count({compteur: {$gt:15, $lt:20}})*

Vous voulez trier l'ordre des données en ordre croissant ? 
Il vous faut rajouter la méthode sort, pour le tri.

*db.obj.find().sort({compteur:1})*

Vous sortira tout les objet trouver, dans l'ordre croissant.
Si vous les voulez dans l'ordre décroissant, il vous faut remplacer le *sort({compteur:1})* par *sort({compteur:-1})*.

Si vous désirez supprimé un document il faut utiliser la methode **remove**.
Pour ce faire nous reprendrons une autre de nos collection, celle de collection_a.

*db.collection_a.remove({"name":"objet1"})*

Supprimera uniquement le document de l'objet1, nous laissant ainsi une collection vide, si vous n'en avez pas créer d'autres.
Il ne vous est pas possible de savoir "où vous êtes" puisqu'étant donné qu'il n'y à pas de shéma dans ces SGBD, Vous n'avez pas de chemin relatif comme dans vos ordinateurs.
Cependant, comme pour les bases de données avec "show dbs", vous pouvez voir les collections qui sont présente avec **show collections**.

De même qu'il vous es possible de les supprimer avec la méthode **drop**.

Nous en profiterons pour supprimer la collection_a et la collection_b avec ;

*db.collection_a.drop()*
*db.collection_b.drop()*

Qui nous renvérra la réponse "true", ce qui nous confirme la suppression de la collection concerné. 

De même que pour supprimer tous les documents d'une collections il faut utilisé la methode **remove**, sans rien précisé en paramètre, sauf les accolades;

Par exemple nous allons supprimer les documents de la collection *obj* contenant 49 "quelque_chose", comme ceci ;

*db.obj.remove({})*

Maintenant, nous allons créer un tableau.

*db.tableau.insert({compteur:1, tab:['Name : Nom','Student : Web-Dev','Preference : PHP']})*

Y ajouté une valeur avec la méthode *update* et la fonction __$push__;

*db.tableau.update({compteur:1}, {$push : {tab : 'Music : Rock'}})*

Y supprimer la dernière valeur du tableau (Music) avec la fonction __$pop__;

*db.tableau.update({compteur:1}, {$pop : {tab:1}})*
	
Ou la première (Name);
		
*db.tableau.update({compteur:1}, {$pop : {tab:-1}})*

Pour ajouter une nouvelle valeur, et évité les doublons, il y a la fonction __$addToSet__ ;

*db.tableau.update({compteur:1}, {$addToSet : {tab : 'Music : Rock'}})*

Si la valeur *Music : Rock* existe déja, alors il n'y aura pas de nouvelle entré.

Et pour finir je vais vous montré comment supprimer votre base de donnée.

Pour ce faire, placez vous dans la bdd que vous voulez supprimé avec **use**;

*use name_bdd*

Et ensuite faite ;

*db.dropDatabase()*

Vous pouvez vérifié que vous avez bien supprimer la bdd avec **show db**.

Voilà, j'ai fini de vous présenté les commandes de bases pour la manipulation des documents.
Si vous êtes interressé par cette technologie, je vous invite à allez sur ces liens pour en comprendre les introduction aux SGBD NoSQL.

-----------------

###Vidéos ;

https://www.youtube.com/watch?v=eRBEK8HpHSQ&list=PLYdDK3N4CIhrxekXduvcXRu_5JbSSi3gc
https://www.youtube.com/watch?v=vpiDl4NEyQQ
https://www.youtube.com/watch?v=oIpjcqHyx2M
https://www.youtube.com/watch?v=LWJelYu0FJQ
https://www.youtube.com/watch?v=Z6WMfbGCsUM
https://www.youtube.com/watch?v=z8bd0XTtJLg

###Site Web ;

https://fr.wikipedia.org/wiki/NoSQL
https://www.grafikart.fr/blog/sql-nosql
https://www.piloter.org/business-intelligence/base-nosql.htm
http://www.lsis.org/espinasseb/Supports/BD/BD_NOSQL-4p.pdf
https://www.digora.com/fr/blog/definition-base-nosql-datastax-mongodb
https://www.mongodb.com/nosql-explained
http://www.w3big.com/fr/mongodb/default.html
https://buzut.developpez.com/tutoriels/apprendre-commandes-de-base-de-mongodb/
https://nosql.developpez.com/cours/
https://openclassrooms.com/fr/courses/4462426-maitrisez-les-bases-de-donnees-nosql
https://docs.mongodb.com/