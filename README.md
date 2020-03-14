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
db.getCollection("pets").count({});</br >
Count:8
<br /><br />
Retorne apenas um elemento o método prático possível<br />
db.getCollection("pets").findOne({});<br />
output: { 
    "_id" : ObjectId("5e5edf3a6e65a00df16ee905"), 
    "name" : "Mike", 
    "species" : "Hamster"
}

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
);<br/>
Output: { 
    "_id" : ObjectId("5e5edf3a6e65a00df16ee905"), 
    "name" : "Mike", 
    "species" : "Hamster"
}

<br /><br />
Use o find para trazer todos os Hamsters<br />
db.getCollection("pets").find(
    { 
        "species" : "Hamster"
    }
);<br/>
output
{ 
    "_id" : ObjectId("5e5edf3a6e65a00df16ee905"), 
    "name" : "Mike", 
    "species" : "Hamster"
}<br/>
{ 
    "_id" : ObjectId("5e5ee0317d46ac60d2a10f19"), 
    "name" : "Frodo", 
    "species" : "Hamster"
}

<br /><br />
Use o find para listar todos os pets com nome Mike<br />
db.getCollection("pets").find(
    { 
        "name" : "Mike"
    }
);<br/>
output
{ 
    "_id" : ObjectId("5e5edf3a6e65a00df16ee905"), 
    "name" : "Mike", 
    "species" : "Hamster"
}<br/>
{ 
    "_id" : ObjectId("5e5edf3a6e65a00df16ee908"), 
    "name" : "Mike", 
    "species" : "Cachorro"
}

<br /><br />

Liste apenas o documento que é um Cachorro chamado Mike<br />
db.getCollection("pets").find(
    { 
        "name" : "Mike",
        "species": "Cachorro"
    }
);<br/>
output:{ 
    "_id" : ObjectId("5e5edf3a6e65a00df16ee908"), 
    "name" : "Mike", 
    "species" : "Cachorro"
}
<br /><br />

EXERCICIO 2
*****************************************************************************
<br />
Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade.<br />

db.getCollection("italians").count(
    { 
        "age" : 99.0
    }
);<br/>
Count: 0
<br /><br />


Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos)<br />
db.getCollection("italians").count(
    { 
        "age" : {$gte :65}
    }
);<br/>
Count: 1887
<br /><br />

Identifique todos os jovens (pessoas entre 12 a 18 anos).<br />
db.getCollection("italians").find(
    { 
        "age" : {$gte :12, $lt :18}
    }
);
<br/>
Count: 706

<br /><br />

Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois<br />
db.getCollection("italians").count(
    { 
        "cat": { "$exists": true }
    }
);<br/>
Count: 5970
<br /><br />

db.getCollection("italians").count(
    { 
        "dog": { "$exists": true }
    }
);<br/>
Count: 4055
<br /><br />

db.getCollection("italians").count(
    { 
        "$and": [{"cat": {"$exists": false}},
                 {"dog": {"$exists": false}}]
    }
);<br/>
Count: 2386
<br /><br />

Liste/Conte todas as pessoas acima de 60 anos que tenham gato<br />
db.getCollection("italians").find(
    { 
        "$and": [{"age": {"$gt": 60}},
                 {"cat": {"$exists": true}}]
    }
).count();<br/>
Count: 1398
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
<br/>
Count: 231
<br /><br />

Utilizando o $where, liste todas as pessoas que tem gato e cachorro<br />
db.italians.find(
    {$where: "this.dog && this.cat"}
).count();<br/>
Count:2411
<br /><br />

Liste todas as pessoas mais novas que seus respectivos gatos.<br />
db.italians.find(
    {$where: "this.cat && this.age < this.cat.age"}
);<br/>Count: 603
<br /><br />

Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro)<br />
db.italians.find(
    {$where: "(this.dog && this.firstname == this.dog.name) || (this.cat && this.firstname == this.cat.name) "},
    {firstname:1, dog:1, cat:1}
);<br/>Cout: 90
<br /><br />

Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo<br />
db.italians.find(
        {bloodType: {$regex:/\w{1,2}-/i}} 
        ,{_id:0, firstname:1, surname:1/*, bloodType:1*/}
);<br/>Count: 4962
<br /><br />

Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)<br />
db.italians.aggregate(
    [{$match: {$or: [{dog: {$exists: true}}, 
                     {cat: {$exists: true}}]
                 }
     },
     {$project: {_id:0, 'cat.name':1, 'cat.age':1, 'dog.name':1, 'dog.age':1}}
    ]
);<br/>
Output:
 { 
    "cat" : {
        "name" : "Nicola", 
        "age" : 12.0
    }
}<br/>
{ 
    "cat" : {
        "name" : "Sonia", 
        "age" : 8.0
    }, 
    "dog" : {
        "name" : "Valentina", 
        "age" : 2.0
    }
}<br/>
{ 
    "cat" : {
        "name" : "Domenico", 
        "age" : 1.0
    }
}<br/>
Type "it" for more
<br /><br />

Quais são as 5 pessoas mais velhas com sobrenome Rossi?<br />
db.italians.find(
    {surname: 'Rossi'}
).sort({age: -1}).limit(5);
<br/>
Output:<br />
{ 
    "_id" : ObjectId("5e5ee7f9df2c3f4fbe46418b"), 
    "firstname" : "Lorenzo", 
    "surname" : "Rossi", 
    "username" : "user406", 
    "age" : 76.0, 
    "email" : "Lorenzo.Rossi@outlook.com", 
    "bloodType" : "B+", 
    "id_num" : "501312648576", 
    "registerDate" : ISODate("2015-04-06T10:14:38.835+0000"), 
    "ticketNumber" : 5258.0, 
    "jobs" : [
        "Estatística"
    ], 
    "favFruits" : [
        "Melancia", 
        "Tangerina", 
        "Melancia"
    ], 
    "movies" : [
        {
            "title" : "Intocáveis (2011)", 
            "rating" : 1.87
        }, 
        {
            "title" : "A Lista de Schindler (1993)", 
            "rating" : 2.6
        }
    ], 
    "cat" : {
        "name" : "Simona", 
        "age" : 7.0
    }
}<br />
{ 
    "_id" : ObjectId("5e5ee7fcdf2c3f4fbe464d79"), 
    "firstname" : "Mirko", 
    "surname" : "Rossi", 
    "username" : "user3460", 
    "age" : 76.0, 
    "email" : "Mirko.Rossi@yahoo.com", 
    "bloodType" : "B+", 
    "id_num" : "356060125741", 
    "registerDate" : ISODate("2014-03-31T01:40:18.034+0000"), 
    "ticketNumber" : 7782.0, 
    "jobs" : [
        "Cooperativismo", 
        "Design de Games"
    ], 
    "favFruits" : [
        "Mamão", 
        "Laranja"
    ], 
    "movies" : [
        {
            "title" : "A Lista de Schindler (1993)", 
            "rating" : 0.06
        }
    ]
}<br />
{ 
    "_id" : ObjectId("5e5ee7fcdf2c3f4fbe464eed"), 
    "firstname" : "Sergio", 
    "surname" : "Rossi", 
    "username" : "user3832", 
    "age" : 76.0, 
    "email" : "Sergio.Rossi@live.com", 
    "bloodType" : "B-", 
    "id_num" : "065144161521", 
    "registerDate" : ISODate("2009-05-13T20:05:22.765+0000"), 
    "ticketNumber" : 1143.0, 
    "jobs" : [
        "Biotecnologia", 
        "Engenharia e Produção"
    ], 
    "favFruits" : [
        "Goiaba"
    ], 
    "movies" : [
        {
            "title" : "O Silêncio dos Inocentes (1991)", 
            "rating" : 1.77
        }, 
        {
            "title" : "Star Wars, Episódio V: O Império Contra-Ataca (1980)", 
            "rating" : 0.88
        }, 
        {
            "title" : "Seven: Os Sete Crimes Capitais (1995)", 
            "rating" : 3.16
        }, 
        {
            "title" : "Os Sete Samurais (1954)", 
            "rating" : 3.31
        }, 
        {
            "title" : "O Senhor dos Anéis: A Sociedade do Anel (2001)", 
            "rating" : 3.76
        }
    ]
}<br />
{ 
    "_id" : ObjectId("5e5ee7ffdf2c3f4fbe465caa"), 
    "firstname" : "Roberto", 
    "surname" : "Rossi", 
    "username" : "user7349", 
    "age" : 76.0, 
    "email" : "Roberto.Rossi@outlook.com", 
    "bloodType" : "AB-", 
    "id_num" : "265481662567", 
    "registerDate" : ISODate("2016-03-19T21:35:52.637+0000"), 
    "ticketNumber" : 9857.0, 
    "jobs" : [
        "Produção Editorial", 
        "Fabricação Mecânica"
    ], 
    "favFruits" : [
        "Uva", 
        "Laranja", 
        "Maçã"
    ], 
    "movies" : [
        {
            "title" : "Vingadores: Ultimato (2019)", 
            "rating" : 1.77
        }
    ], 
    "mother" : {
        "firstname" : "Tiziana", 
        "surname" : "Rossi", 
        "age" : 100.0
    }
}<br />
{ 
    "_id" : ObjectId("5e5ee7ffdf2c3f4fbe465ed9"), 
    "firstname" : "Enzo ", 
    "surname" : "Rossi", 
    "username" : "user7908", 
    "age" : 76.0, 
    "email" : "Enzo .Rossi@hotmail.com", 
    "bloodType" : "B-", 
    "id_num" : "655841215010", 
    "registerDate" : ISODate("2016-10-11T09:08:24.725+0000"), 
    "ticketNumber" : 8520.0, 
    "jobs" : [
        "Teologia"
    ], 
    "favFruits" : [
        "Maçã"
    ], 
    "movies" : [
        {
            "title" : "Um Estranho no Ninho (1975)", 
            "rating" : 0.52
        }, 
        {
            "title" : "1917 (2019)", 
            "rating" : 1.19
        }, 
        {
            "title" : "Interestelar (2014)", 
            "rating" : 4.36
        }, 
        {
            "title" : "Gladiador (2000)", 
            "rating" : 4.48
        }
    ], 
    "father" : {
        "firstname" : "Roberta", 
        "surname" : "Rossi", 
        "age" : 106.0
    }, 
    "cat" : {
        "name" : "Salvatore", 
        "age" : 0.0
    }, 
    "dog" : {
        "name" : "Veronica", 
        "age" : 0.0
    }
}
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
<br />
Liste as ações com profit acima de 0.5 (limite a 10 o resultado)<br />
db.stock.find(
    {'Profit Margin' : {$gt:0.5}}
).limit(10)
<br /><br />

Liste as ações com perdas (limite a 10 novamente)<br />
db.stock.find(
    {'Profit Margin' : {$lt:0.0}}
).limit(10)
<br /><br />

Liste as 10 ações mais rentáveis<br />
db.stock.find().sort(
    {'Profit Margin':-1}
).limit(10)

<br /><br />

Qual foi o setor mais rentável?<br />
db.stock.aggregate([
	{$group: {_id: "$Sector", profit: {$sum: "$Profit Margin"}}},
	{$sort: { profit: -1 } }
])
<br /><br />

Ordene as ações pelo profit e usando um cursor, liste as ações.<br />
var i = db.stock.find({}).sort({'Profit Margin':-1});
i.next();
<br /><br />

Agora liste apenas a empresa e seu respectivo resultado<br />
db.stock.find(
    {},
    {_id:0, Company: 1, profit: 1}
)
<br /><br />

Analise as ações. É uma bola de cristal na sua mão... Quais as três ações você investiria?<br />
/* idem exercicio 3 - as mais rentaveis*/
db.stock.find().sort(
    {'Profit Margin':-1}
).limit(3)
<br /><br />


Liste as ações agrupadas por setor<br />
db.stock.aggregate(
    [{$group: {_id:'$Sector', acoes:{$push:'$$ROOT'}}},
     {$sort: {_id:1}}
    ]
)
<br /><br />

