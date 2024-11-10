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

➡️ ***The `$in` operator in MongoDB acts like an implicit OR within the same field. This means that it checks if any value in the specified array matches the field's value, and if it finds any match, the document is returned.***
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
                level: "Intermidiate"
            }
        }
    },
    { skills: 1 })
 ```

 👊*****Update*****

 
  ***`$set` ➡️ Should be used if absolutely replaced, otherwise not.For primitive data type it can be used to update data but do no use for updating a non primitive data.***
```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $set: {
            name: {
                "firstName": "Niaz",
                "lastName": "Ahmed"
            }
        }
    }
)
 ```

 🤔 **But `$set` can be used only as a non-primitive datatype when it is Object**

```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $set: {
            "address.city": "Sirajgonj",
            "address.country": "Bangladesh"
        }

    }
) 
```

***Instead use ➡️ `$addToSet` for updating non primitive data type.***
```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $addToSet: {
            interests: "Programming"
        }
    }
)
```
🤔 ***if updating data more than one then use `$each` method to update array or non primitive data.***

```monogdb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $addToSet: {
            interests: {
                $each: ['Wacthing Movies', 'Sleeping']
            }
        }
    }
)
```

😒 ***Update duplicate data then use `$push` method.***

```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $push: {
            interests: {
                $each: ['Wacthing Movies']
            }
        }
    }
)
```

👊 ***The `$unset` operator deletes a particular field.***
```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $unset: {
            birthday: ""
        }
    }
)
```
👊 ***The `$pop` operator removes the first or last element of an array.***
```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $pop: {
            friends: 1
        }
    }
)
```
😒***The `$pull` operator removes from an existing array all instances of a value or values that match a specified condition.***
```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $pull: {
            friends: "MH Emon"
        }
    }
)
```
➡️***The `$pullAll` operator removes all instances of the specified values from an existing array.***
```mongodb
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $pullAll: {
            interests: ["Gaming",
                "Cooking",
                "Writing",
                "Programming",
                "Wacthing Movies", "Gaming",
                "Cooking",
                "Writing",
                "Programming",
                "Wacthing Movies",]
        }
    }
)
```

👊 ***Projection***


➡️ ***The `$` operator projects the first matching array`[]` element from each document in a collection based on some condition from the query statement.***

```bash
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000069"), "education.degree": "Master of Science" },
    {
        $set: {
            "education.$.degree": "CSE"
        }

    }
) 
```

