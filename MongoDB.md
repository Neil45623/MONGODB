MONGODB ---> Fuir le SQL

GESTION DE BASE DE DONNEE

création dun compte mOngoDB atlas

Lorsque on fait du js utilisez du typescript

Optimiser le temps d'execution avec un index

Pourquoi le format json ?

Format très répendu / aucune structure imposé / Les documents permettent la gestion dune quantité immense de données

MONGODB est une base orienté document

Unité de stockage --> document

Le document est stocké dans un containers et n'a pas de schema prédéfini

2 document peuvent contenir les même champs (objet) (stocké en json)

EN mysql on a plusieurs contrainte :

Solution pour un champs "système"

On créé un table adresse pour alléger la table utilisateur

MOngoDb est crossplateform

Base de données stock des collection

Le schema des documents est dis "dynamique"

mongoDB est convertible avec toute les techno back

Gros désavantage de mongo : beuacoup d'information sont dupliqué(n'a pas dimpact en performance mais en applicatif si)

Il faut savoir se détacher du relationnel pour pouvoir bien utiliser mongoDB

AVNATAGE :

MONGODB est fait pour la monter en charge / plus besoin d'ORM

Structure clair et simple

Indexage possible sur chaque attribut

Excellent support de développement rapide et efficace

DANS QUEL DOMAINE lutilise ton ?

APP WEB

BIG DATA

DATA HUB

Les propriétés json sont représenté par des pair cle valeur

``` json

{ "clé": "valeur","autre_cle" : ""}

{

"nom": "Nicolas",

 "prenom": "PERACHON",

"parent" : [

]

}

```

Les types de données

-booleen

-numerique

-chaine de caractere

-tableau

-objet

-null (absence de bvaleur)

MongoDB apporte ces propres types a ceux rencontrés au sein du format JSON standart :

-le type date : entier signé de 8 octets représente le nombre de seconde depuis epoch( 01/01/1970 00.00.00), le type date ne stock pas la timezone

-le type objectID : stock sur 12 octets, utile en interne afin de garantir l'unicite des identifiants auto-generes par la BDD

-Numberlog et numberint :  par defaut mongo considere que toute valeur numerique est un nombre a virgule code sur 8 octets

-number decimal : code sur 16 octets bonne précsion des deciamles

-Bindata: pour stocker des caractere quon ne peut pas representer en utf 8

On peut utiliser "use" pour se connecter a une bdd inexistante et  a linsertion du premier document cela creera la bdd

Bash

Use tp

```

Effectuer la requete

```

Resultat du type :

``` Javascript

{ }

```

Vous remarquez lutilisation du mot cle 'db' il sagit dun mot cle qui renvoi vers la bdd en corus dutilisation la combinaison 'db.unecollection' constitue une 'namespace'en mongodb

Supression d'une bdd :

```Javascript

Use math

Db.dropdatabase()

```

Pour verifier utilisez la commande :

```

Show dbs

```

Afin de lister les bdd

Mongodb met a disposition des commandes de tpe helper

runcommand()

adminCommand()

Colation : 
local = localisation

Pour créer une collection il suffit dutiliser :
```
db.createcollection(
"macollection",
{"colation"; {"locale" : "fr"}}
)
```



Collection : permet de stocker des documents de manière logique / groupe de  document equivalent  d'une table système de gestion de bdd

Schema :

INDEX : Il stock les mots les plus important et les plus utilisés afin de faciliter le moyen de les retrouver. Attention si il est trop long il peut engendrer des ralentissements

Exemple d'index : 

```javascript
db.personnes.insertMany([    {"nom": "Durand", "prenom": "René", "interets": ["jardinage", "bricolage"], "age": 77},
    {"nom": "Durand", "prenom": "Gisèle", "interets": ["bridge", "cuisine"], "age": 75},
    {"nom": "Dupont", "prenom": "Gaston", "interets": ["jardinage",   "pétanque"], "age": 79},
    {"nom": "Dupont", "prenom": "Catherine", "interets": ["cuisine"], "age": 66},
    {"nom": "Duport", "prenom": "Eric", "interets": ["cuisine", "pétanque"], "age": 57},
    {"nom": "Duport", "prenom": "Arlette","interets": ["jardinage"], "age": 80},
    {"nom": "Lejeune", "prenom": "Jean","interets": ["jardinage"], "age": 75},
    {"nom": "Lejeune", "prenom": "Mariette","interets": ["jardinage", "bridge"], "age": 66}
]);

```
Exemple d'index simple :
```js
db.collection.createIndex(<champs_et_type>), <option>)
db.personnes.createIndex({"age" : 1}); // créé dans lordre croissant
```

Comment consulter la lsite d'index d'une collection ? 

```js
db.collection.getIndex()
```

afin de supprimer un index on utilise :

```js
db.collection.dropIndex("age_1_");

db.collection.createIndex({"age" : -1}, {"name" : "unNom"});

db.collection.createIndex({"prenom" : 1}, {"background" : true});
```



Document : les document présent au sein d'une meme collection peuvent avoir des champs différent (un ensemble de données clé valeur)  
  
Malgré cela, ils ont une utilisation similaire et reste très similaire

TABLEAUX : 

```js
db.hobbies.insertMany([   {"_id": 1, "nom": "Yves"},   {"_id": 2, "nom": "Sandra", "passions": []},   {"_id": 3, "nom": "Line", "passions": ["Théâtre"]}  ])
```
Les opérateur de tableaux : 

```js
{$push {<champs>: <valeur>, ...}}
```

L'opérateur push êrmet d'ajouter  une ou plusieur valeur au sein d'un tableau

```js
db.hobbies.updateOne({"_id" : 1}, {$push : {"passions" : "le roller !"}});
```


```js
db.hobbies.updateOne({
"_id" : 2
}$push{
"passion" : {
$each: ["Minecraft", "Rise of kingdom"]
}
})

```

Pour eviter les doublons : 
```js
db.hobbies.updateOne({"_id": 2}, {$addToSet: {"passions":{$each: ["Minecreaft", "Rise of Kingdom"]}}})
```


```js
db.personnes.find({"interets" : "jardinage"})
db.personnes.find({"interets" : {$all :["jardinage","bridge"]}}) // recherche sur toute les personne qui possède ces deux interets
db.personnes.find({interet : {$size : 2}}) // toute les personne squi possède deux interets
db.personnes.find({"interets.1" : {$exist : 1}}) // cela sert a trouver les persones qui ont au moins 2 interets



```

EXO JOUR 1-2 :
```javascript



db.collection.find() //cherche dans la collection des informations
db.collection.find({}).sort({})

use "nom de bdd" //créé une bdd ou switch su r une existante

db.createcollection("macollection") //créé une collection

db.colllection.aggregate

sample_db> db.employee.updateMany( {job: "Developer"}, {$set:{Salary:"80000"}}) //def salaire

db.employee.find().sort({Salary: "-1"}) //range par ordre croissant les salaires

db.employee.find({age:{$gt: "33"}}) //Cherche tout les utilisateurs qui ont un age supérieur a 33

db.employee.updateMany( {job: "Developer"}, {$set:{Salary:"80000"}}) // Change les sallaire de tout les dev

db.employees.aggregate([{$group: {_id: "$job", count: { $sum: 1}}}]) //Affiche le nombre d'employer par job 

EX1 :

sample_db> db.salles.find({ "smac" : true}) //trouver toutes les salles smac

EX2 :

db.salles.find({ "capacite" :{$gte : 1000}}) // capacité superieur ou egale a 1000

EX3 :

db.salles.find({"adresse.numero": {$exists:false}}) //affiche toute les salles ou le champs adresse ne comporte pas de numéro

EX4 :

db.salles.find({avis: {$size:1}},{_id:1, nom:1})// affiche les identifiants des salles qui possède un avis

EX5 :

db.salles.find({styles: "blues"}, {styles:1})// affiche tous les styles musicaux des salles qui programment du blues

EX6:

db.salles.find({"styles.0": "blues"}, {styles:1})//// affiche tous les styles musicaux des salles qui ont du blues en première position. premiere position= "styles.0"

EX7:

db.salles.find({ "adresse.codePostal": { $regex: /^84/ }, "capacite": { $lt: 500 } }, { "adresse.ville": 1,}) // affiche toute les salles qui ont le code postale qui commence par 84

EX8:

db.salles.find({$or: [{_id: {$mod: [2, 0]}}, {avis: {$exists: false}}]}, {_id: 1}) // retourne les identifiants des salles qui ont un identifiant pair et/ou le champ avis est vide.

EX9 :

db.salles.find({avis: {$elemMatch: {note: {$gte: 8, $lte: 10}}}}, {nom: 1}) // affiche les salles qui ont au moins un avis et qui ont une note entre 8 et 10

EX10 :

db.salles.find({avis: {$elemMatch: {date: {$gt: new Date('2019-11-15')}}}}, {nom: 1, _id: 0}) // AFFICHE LES SALLES QUI POSSSEDE un avis avant le 15/11/2019

EX11 :

db.salles.find({$expr: {$gt: [{$multiply: [100, "$_id"]}, "$capacite"]}},{"nom": 1, "capacite": 1}) // Affiche les salles qui ont l'idx100 supérieur à la capacite (multiply sert a multiplier 100 sur l'id)($gt dis que ça affiche seulement ce qui est supérieur)($expr permet justement de detecter si IDx100 est plus grand que capacité)

EX12 :

db.salles.find({ smac: true, $where: "this.styles.length > 2"}, { nom: 1, _id: 0 }) //AFFICHE les salles smac qui ont Plus de 2 style de musique (ne fonctionne pas en version gratuite)

EX13 :

db.salles.distinct("adresse.codePostal") // afficher tout les code postaux

EX14 :

db.salles.updateMany({},{$inc:{capacite: 100}}) /// rajouter 100 de capacite a toute les salles

EX15 : 

db.salles.updateMany({styles: {$exists: false}}, {$push: {styles: "jazz"}}) // ajoute le style jazz a toute les alles

EX16 :

db.salles.updateMany({_id: {$nin: [2, 3]}}, {$pull: {"styles": "funk"}}); // rretire le style punk a toute les salles don l'id est different de 2 ET 3($nin == !=)

EX17 :

db.salles.update( { _id: 3 }, { $push: { styles: { $each: ["techno", "reggae"] } } } )
// ajoute les styles techno et reggae  a la salle identifiant 3

EX18 :

db.salles.updateMany({nom: /^P/i}, {$inc: {capacite: 150}, $set: {contact: [{telephone: "04 11 94 00 10"}]}})
// Pour toute les salle qui commence par P on augmente de 150 la capacite et rajoute le tableau contact avec un champs telephone

EX19 : 


```