mongodb

EXERCICIO 1
***************************************************************************

Adicione outro Peixe e um Hamster com nome Frodo
use petshop
db.pets.insert({name:"Frodo", species:"Hamster"})
db.pets.insert({name:"Frodo", species:"Peixe"})


Faça uma contagem dos pets na coleção
db.getCollection("pets").count({});

Retorne apenas um elemento o método prático possível
db.getCollection("pets").findOne({});


Identifique o ID para o Gato Kilha.
db.getCollection("pets").find(
    { 
        "name" : "Kilha"
    }
);
find: 5e5edf3a6e65a00df16ee907


Faça uma busca pelo ID e traga o Hamster Mike

db.getCollection("pets").find(
    { 
        "_id" : ObjectId("5e5edf3a6e65a00df16ee905")
    }
);

Use o find para trazer todos os Hamsters
db.getCollection("pets").find(
    { 
        "species" : "Hamster"
    }
);

Use o find para listar todos os pets com nome Mike
db.getCollection("pets").find(
    { 
        "name" : "Mike"
    }
);


Liste apenas o documento que é um Cachorro chamado Mike
db.getCollection("pets").find(
    { 
        "name" : "Mike",
        "species": "Cachorro"
    }
);
