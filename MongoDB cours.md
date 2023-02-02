

MongoDb sert à la gestion de base de données.

Pour commencer, on réalise la création d'un compte mongoDB atlas

L'index va nous permettre d'optimiser le temps de recherche.

Pourquoi le format json ?

Format très répendu / aucune structure imposé / Les documents permettent la gestion dune quantité immense de données

MONGODB est une base orienté document(unité de stockage)

Le document est stocké dans un containers et n'a pas de schema prédéfini

2 document peuvent contenir les même champs

MOngoDb est crossplateform

Base de données stock des collections

Le schema des documents est dis "dynamique"

mongoDB est convertible avec toute les techno back

Gros désavantage de mongo : beaucoup d'information sont dupliqué(n'a pas dimpact en performance mais en applicatif si)

Il faut savoir se détacher du relationnel pour pouvoir bien utiliser mongoDB

AVANTAGE :

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

Vous remarquez lutilisation du mot cle 'db' il sagit dun mot cle qui renvoi vers la bdd en cours d'utilisation la combinaison 'db.unecollection' constitue une 'namespace'en mongodb

Supression d'une bdd :

```js

Db.dropdatabase()

```

Pour verifier utilisez la commande :

```js
Show dbs

```

Afin de lister les bdd

Mongodb met a disposition des commandes de tpe helper

runcommand()

adminCommand()

Colation : 
local = localisation

Pour créer une collection il suffit dutiliser :
```js
db.createcollection(
"macollection",
{"colation"; {"locale" : "fr"}}
)
```



Collection : permet de stocker des documents de manière logique / groupe de  document equivalent  d'une table système de gestion de bdd

Schema :

INDEX : Il stock les mots les plus important et les plus utilisés afin de faciliter le moyen de les retrouver. Attention si il est trop long il peut engendrer des ralentissements

Exemple d'index : 

```js
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

Les requetes geospaciales :
```js
{ type: <type d'objet GEOJSON>, coordinates : <coordonee>}

```
Le type point :
```json
{
	"type" : "Point",
	"coordinates" : [13.0, 1.0]
}
```
Le type multipoint :
```json
{
	"type" : "MultiPoint",
	"coordinates" : [
	[13.0, 1.0], [13.0, 3.0]
	]
}
```
Le type LineString : 
```json
{
	"type" : "LineString",
	"coordinates" : [
	[
	[13.0, 1.0], [13.0, 3.0]
	],
	[
		[13.0, 1.0] , [13.0, 3.0]
	]
	]
}
```
Le type polygon :
```json
{
	"type" : "Polygon",
	"coordinates" : [
	[
	[13.0, 1.0] , [13.0, 3.0]
	],
	[
	[13.0, 1.0] , [13.0, 3.0]
	]
	]
}
```
Création d'index :
```js
db.avignon.createIndex({"localisation" : "2dsphere"})

db.avignon.createIndex({"localisation" : "2d"})


```
L'opérateur $nearSphere :
```js
{
	$nearSphere : {
		$geometry : {
			type : "Point",
			coordinates : [<longitude>, <latitude>]
		},
		$minDistance : <distance en mètres>
		$maxDistance : <distance en mètre>
	}
}

var opera = { type : "Point", coordinates : [43.949749, 4.805325]}
```

Effectuer une requete sur la collection avignon
```js
var opera = { type : "Point", coordinates : [43.949749, 4.805325]}
	db.avignon.find({"localisation" : {$nearSphere : { $geometry : opera}}}, {"_id" : 0, "nom" : 1}).explain()
```

Le framework d'agregation : 

Mongodb met a disposition un puissant outil d'analyse et de traitement de l'information : le pipeline d'agregation(ou framework)

Metaphore du tapis roulant d'usine  / traitement du document telle le tapis roulant

Methode utilisée : 
```json
db.collection.aggregate(pipeline, options)
```
pipeline = designe un tableau d'etapes
options = designe un document

Parmis les options, nous retiendrons : 
-collation, permet d'affecter une collation a l'operation d'agregation
-bypassDocumentValidation : fonctionne avec un opérateur appelé $out et permet de passer au travers de la validation des docs.
-allowDiskuse : donne la possibilité de faire deborder les operations d'ecriture sur le disque.

Vous pouvez appeler aggregate sans argument :
```json
db.personnes.aggregate()
```
Au sein du shell, nous allons créer une variable pipeline : 
```js
var pipeline = {}
db.personnes.aggregate(pipeline)
db.personnes.aggregate(
pipeline,
{
	"collation" : {
		"locale" : "fr"
	}
}
)
```
Le filtrage avec $match :

Cela permet d'obtenr des pipelines performants avec des temps d'execution courts.
Normalement $match doit intervenir  le plus en amont possible dans le pipeline car $match agit comme un filtre en reduisant le nombre de document a traiter plus en aval dans le pipeline. (Dans l'ideal on devrait le trouver comme premier operateur)

La syntaxe est la suivante :
```js
	{$match : {<requete>}}
```

```js
var pipeline = [{
	$match : {
		"interets" : "jardinage"
	},
	$match : {
		"nom" : /^L/,
		"age" : {$gt : 70}
	}, {
		$project : {
			"_id" : 0,
			"nom" : 1,
			"prenom" : 1,
			"super_senior" : {$gte : ["$age",70]}
		}
	},
	{
	$match : {
		"super_senior" : true
	}
	}
}
]
db.personnes.aggregate(pipeline)
```
Cela correspond à la requete
```js
db.personnes.find({"interets" : "jardinage"})
```
EXO cours :
```js
db.personnes.updateMany({"nom": "Durand"}, {$set: {"adresse": {"cp": 84140, "ville": "Montfavet"}}})    db.personnes.updateMany({"nom": "Dupont"}, {$set: {"adresse": {"cp": 13480, "ville": "Calas"}}})
```

#### Selection/modification de champs: $project

syntaxe :
```js
{$project : { <spec> }}
```




