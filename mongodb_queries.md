# ***MongoDB Queries***
🔗 ***[operators](https://www.mongodb.com/docs/manual/reference/operator/query/)***

➡️ ***Get all the data***

```bash
db.test.find()
```

➡️ ***one specific data from the database***

```bash
db.test.findOne({gender: "Male"})
```

➡️ ***Query with projection***

```bash
db.test.find({gender: "Male"},{name:1,gender:1,email:1})
```

➡️ ***Chaining***

```bash
db.test.find({ gender: "Male" })
    .sort({ age: -1 })
    .limit(10)
    .skip(5)

```



➡️ ***Comparison Query***

```bash
db.test.find({ gender: 'Female',
 interests: { $in: ["Cooking", "Travelling"] },
 age: { $in: [18, 19, 20, 22, 23, 24, 25, 26] }
},
{
    age: 1, gender: 1, name: 1, interests
        : 1
}).sort({ age: 1 })
```

➡️ ***Explicit `$and`***

```bash
db.test.find({
    $and: [
        { gender: "Female" },
        { age: { $ne: 15 } },
        { age: { $lte: 30 } }
    ]
}).project({
    age: 1,
    gender: 1,

}).sort({ age: 1 })
```

➡️ ***Explicit `$or`***

```bash
db.test.find({
    $or: [
        { interests: "Travelling" },
        { interests: 'Cooking' },
        {"skills.name": "JAVASCRIPT"}
    ]
}).project({
    interests: 1,
    skills: 1

}).sort({ age: 1 })
```

➡️ ***The $in operator in MongoDB acts like an implicit OR within the same field. This means that it checks if any value in the specified array matches the field's value, and if it finds any match, the document is returned.***
```bash
db.test.find({
    "skills.name": { $in: ["JAVASCRIPT", 'PYTHON'] }
}).project({
    interests: 1,
    skills: 1

})
```


👊*****Element Query*****

***$exists***

```mongodb
db.test.find({age: {$exists: true}})
```

***$type***

```mongodb
db.test.find({friends: {$type: "array"}})
```


👊*****Array Query*****

***$all***
```mongodb
db.test.find(
    {
        interests: { $all: ["Travelling", "Gaming", "Reading"] }
    },
    { interests: 1 })
```

***$elemMatch***
```mongodb
db.test.find(
    {
        skills: {
            $elemMatch: {
                name: "JAVASCRIPT",
                level: "Intermediate"
            }
        }
    },
    { skills: 1 })
 ```