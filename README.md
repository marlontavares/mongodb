mongodb

EXERCICIO 1
***************************************************************************
<br />
Adicione outro Peixe e um Hamster com nome Frodo
<br />
use petshop
<br />
db.pets.insert({name:"Frodo", species:"Hamster"})
<br />
db.pets.insert({name:"Frodo", species:"Peixe"})
<br /><br />

Faça uma contagem dos pets na coleção<br />
db.getCollection("pets").count({});
<br /><br />
Retorne apenas um elemento o método prático possível<br />
db.getCollection("pets").findOne({});
<br /><br />

Identifique o ID para o Gato Kilha.<br />
db.getCollection("pets").find(
    { 
        "name" : "Kilha"
    }
);
<br />
find: 5e5edf3a6e65a00df16ee907
<br /><br />

Faça uma busca pelo ID e traga o Hamster Mike<br />

db.getCollection("pets").find(
    { 
        "_id" : ObjectId("5e5edf3a6e65a00df16ee905")
    }
);
<br /><br />
Use o find para trazer todos os Hamsters<br />
db.getCollection("pets").find(
    { 
        "species" : "Hamster"
    }
);
<br /><br />
Use o find para listar todos os pets com nome Mike<br />
db.getCollection("pets").find(
    { 
        "name" : "Mike"
    }
);
<br /><br />

Liste apenas o documento que é um Cachorro chamado Mike<br />
db.getCollection("pets").find(
    { 
        "name" : "Mike",
        "species": "Cachorro"
    }
);
<br /><br />

EXERCICIO 2
*****************************************************************************
<br />
Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade.<br />

db.getCollection("italians").count(
    { 
        "age" : 99.0
    }
);
<br /><br />


Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos)<br />
db.getCollection("italians").count(
    { 
        "age" : {$gte :65}
    }
);
<br /><br />

Identifique todos os jovens (pessoas entre 12 a 18 anos).<br />
db.getCollection("italians").find(
    { 
        "age" : {$gte :12, $lt :18}
    }
);
<br /><br />

Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois<br />
db.getCollection("italians").count(
    { 
        "cat": { "$exists": true }
    }
);
<br /><br />

db.getCollection("italians").count(
    { 
        "dog": { "$exists": true }
    }
);
<br /><br />

db.getCollection("italians").count(
    { 
        "$and": [{"cat": {"$exists": false}},
                 {"dog": {"$exists": false}}]
    }
);
<br /><br />

Liste/Conte todas as pessoas acima de 60 anos que tenham gato<br />
db.getCollection("italians").find(
    { 
        "$and": [{"age": {"$gt": 60}},
                 {"cat": {"$exists": true}}]
    }
);
<br /><br />

db.getCollection("italians").count(
    { 
        "$and": [{"age": {"$gt": 60}},
                 {"cat": {"$exists": true}}]
    }
);
<br /><br />

 Liste/Conte todos os jovens com cachorro<br />
 db.italians.find(
    {"$and": [{"age": {"$gt": 12, "$lt": 18}},
              {"dog": {"$exists": true}}
              ]});
<br /><br />

Utilizando o $where, liste todas as pessoas que tem gato e cachorro<br />
db.italians.find(
    {$where: "this.dog && this.cat"}
).count();
<br /><br />

Liste todas as pessoas mais novas que seus respectivos gatos.<br />
db.italians.find(
    {$where: "this.cat && this.age < this.cat.age"}
);
<br /><br />

Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro)<br />
db.italians.find(
    {$where: "(this.dog && this.firstname == this.dog.name) || (this.cat && this.firstname == this.cat.name) "},
    {firstname:1, dog:1, cat:1}
);
<br /><br />

Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo<br />
db.italians.find(
        {bloodType: {$regex:/\w{1,2}-/i}} 
        ,{_id:0, firstname:1, surname:1/*, bloodType:1*/}
);
<br /><br />

Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)<br />
db.italians.aggregate(
    [{$match: {$or: [{dog: {$exists: true}}, 
                     {cat: {$exists: true}}]
                 }
     },
     {$project: {_id:0, 'cat.name':1, 'cat.age':1, 'dog.name':1, 'dog.age':1}}
    ]
);
<br /><br />

Quais são as 5 pessoas mais velhas com sobrenome Rossi?<br />
db.italians.find(
    {surname: 'Rossi'}
).sort({age: -1}).limit(5);
<br /><br />

Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano<br />
db.italians.insert({
        "firstname" : "Ezio",
        "surname" : "Auditore",
        "username" : "user7856",
        "age" : 33,
        "email" : "ezio@ubisoft.com",
        "bloodType" : "AB+",
        "id_num" : "256485232144",
        "registerDate" : ISODate("2020-03-07T19:00:00.123Z"),
        "ticketNumber" : 9955,
        "jobs" : ["Assassino"],
        "favFruits" : ["Tomate"],
        "movies" : [
                {
                    "title" : "Assassin's Creed - 2016",
                    "rating" : 5.7
                }
        ],
        "lion" : { "name" : "altaïr", "age" : 1 }
})
<br /><br />

Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.<br />
/*encontra o _id*/<br />
db.getCollection("italians").find({ "firstname": "Ezio" });<br />
/*Apaga*/<br />
db.italians.remove({_id: ObjectId("5e641cb2ec7197c22bae48e3")})
<br /><br />


EXERCICIO 3
*****************************************************************************






