installation de mongo tools , ajout dans les variable du PC puis importation des données json avec la commande :

```json
mongoimport --db Evaluation --collection Pizza --file donnee.json mongodb+srv://test:bLdDVadMREeSumR1@cluster0.onvn7qm.mongodb.net/?retryWrites=true // POUR JSON


mongoimport --db Evaluation --collection Restaurants --type csv --file pizza_hut_locations.csv --headerline mongodb+srv://test:bLdDVadMREeSumR1@cluster0.onvn7qm.mongodb.net/?retryWrites=true // pour un csv
```
A / Recherchez les restaurants qui sont ouvert à partir de 18h00 :
```js

// Les requêtes qui fonctionne sont les suivantes :

db.Pizza.find({"ouverture_soir" : /^18/}) // Ici on recherche tout les restaurants ou l'horiare d'ouverture du soir commence par 18 heures

db.restaurants.find({"ouverture_soir": {$gte: 18}}) // Ici on recherche tout les restaurants qui ont un horaire d'ouverture supérieur ou egal a 18

db.Pizza.find({ "ouverture_soir": { $eq: "18:00" } }) // ici on recherche tout les restaurants qui ont le champs ouverture_soir qui est égal à 18:00

db.Pizza.find({ "ouverture_soir": { $regex: "^18.*" } })  // ici on recherche tout les restaurants qui ont le champ ouverture_soir commencant par 18.


					
```

B/ Trier les restaurants par note, par ordre décroissant :

```js
db.Pizza.find().sort({"Note": 1}) // affiche dans l'ordre décroissant toute les pizzas

db.Pizza.find({}, {name: 1, Note: 1}).sort({Note: -1}) // on utilise celle ci pour afficher le nom et la note seulement pour faciliter laffichage des données.

```

C/ Créer un Index sur le champ d'ouverture des restaurants pour améliorer la recherche :  Vérifiez que l'index a été créé en utilisant la méthode listIndexes ().

```js

db.Pizza.createIndex( {"name" : 1, "ouverture_soir": 1 } ) // Création de l'index ouverture_soir

db.runCommand({listIndexes: "Pizza"})//Pour avoir la liste des index 

```

D/  Recherchez les restaurants qui se trouvent à moins de 2 km d'une certaine localisation

```js
db.Pizza.createIndex( { location : "2dsphere" } )// création d'un index geospaciale

db.Pizza.find( { location: { $nearSphere: { $geometry: { geometry_type : "Point", coordinates : [4.8648312, 45.7569798003785] }, $maxDistance: 2000 } } } )
```

E/ Recherchez les restaurants qui se trouvent dans un certain rayon autour d'un point de localisation spécifique.

```js

```

F/ Calculez la moyenne des notes des restaurants. Utilisez le framework d'agrégation de MongoDB pour effectuer des calculs sur les données

```js
```

G/ Trouvez les restaurants les plus populaires en fonction du nombre de commentaires. Utilisez le framework d'agrégation de MongoDB pour groupes les données et effectuer des calculs.
```js

```

H/ Exportez les résultats des requêtes dans un fichier CSV pour un usage ultérieur. Utilisez la commande mongoexport pour exporter des données de MongoDB.

