installation de mongo tools , ajout dans les variable du PC puis importation des données json avec la commande :

```json
mongoimport --db Evaluation --collection Pays --file Infos.json mongodb+srv://test:bLdDVadMREeSumR1@cluster0.onvn7qm.mongodb.net/?retryWrites=true // POUR JSON


mongoimport --db Evaluation --collection Restaurants --type csv --file pizza_hut_locations.csv --headerline mongodb+srv://test:bLdDVadMREeSumR1@cluster0.onvn7qm.mongodb.net/?retryWrites=true // pour un csv
```
Recherchez les restaurants qui sont ouvert à partir de 18h00 :

Trier les restaurants par note, par ordre décroissant :