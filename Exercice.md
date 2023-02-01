```js



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

db.salles.update({nom: {$regex: '[^aeiou]+$'}}, {$push: {avis: {date: new Date(), note: NumberInt(10)}}}) // Pour les salles commencant par une voyelle on ajoute dans le tableau avis un document composé du champ date v= date courante et note = 10

EX20 :

db.salles.updateMany({nom: {$regex: /^[zZ]/ } }, {$set: {nom: "Pub Z", capacite: 50, smac: false}}, {upsert: true}) //mettre a jour le nom des document commencant par z ou Z en pub Z avec capacite 50 de plus on place smac en false

EXO INDEX :

db.salles.createIndex({"adresse.codePostal" : 1, "capacite" : 1})// codepostal et capacite contienne des données
db.salles.dropIndex({"adresse.codePostal" : 1, "capacite" : 1}); //destruction de l'index créé

EXO VALIDATION :

EX1 :

db.runCommand({collMod: "salles",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nom", "capacite", "adresse.ville", "adresse.codePostal"],
      properties: {
        nom: {
          bsonType: "string",
          description: "Type char et obligatoire"
        },
        capacite: {
          bsonType: "int",
          description: "type int et obligatoire"
        },
        adresse: {
          bsonType: "object",
          required: ["ville", "codePostal"],
          properties: {
            ville: {
              bsonType: "string",
              description: "type char et obligatoire"
            },
            codePostal: {
              bsonType: "string",
              description: "type char et obligatoire"
            }
          }
        }
      }
    }
  }
}) 

// De plus, l'insertion ne fonctionne pas car il manque le code postal

db.salles.insertOne({"nom" : "super salle", "capacite" : 1500, "adresse" : {"ville" : "Musiqueville", "codePostal" : "01600"}}) // cette insert fonctionnerait

EX2 : 

db.salles.updatesMany({}, {$set : {"verifie : true"}})

// lorsque on essaye de mettre a jour avec cette commande une erreur apparait : car le champs verifie si il existe ou non dans le schema

EX3: 

db.runCommand({ collMod: "salles", validationLevel: "strict", validator: { $or: [ {smac: {$exists: true}}, {stylesMusicaux: {$in: ["jazz", "soul", "funk", "blues"]}} ] } })// permet de rajouter des critère de validation

```