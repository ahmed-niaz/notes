# **_What is Aggregation_**

🤔 _Aggregation is a way of processing a large number of documents in a collection by means of passing them through different stages.This stages make up what is known as pipeline_

_The stages in pipeline can `filter` `sort` `group` `reshape` and `modify` documents that pass through the pipeline_

➡️ **_`$match` Stage Placement_**

```mongodb
db.test.aggregate([
    // stage 1
    { $match: { gender: 'Male', age: { $lt: 30 } } },
    // stage 2
    { $project: { gender: 1, age: 1, name: 1 } }
])
```

➡️ **_`$addFields` Stage Placement_**

_it does not modify the original document_

```mongodb
db.test.aggregate([
    // stage 1
    { $match: { gender: 'Male' } },
    // stage 2
    { $addFields: { course: "level_2", eduTech: 'Programming Hero' } },
    // stage 3
    { $project: { course: 1, eduTech: 1 } }
])
```

➡️ **_`$out` Stage Placement_**

_create a new document_

```mongodb
db.test.aggregate([
    // stage 1
    { $match: { gender: 'Male' } },
    // stage 2
    { $addFields: { course: "level_2", eduTech: 'Programming Hero' } },
    // stage 3
    { $project: { course: 1, eduTech: 1 } },
    // stage 4
    { $out: "pHero" }
])
```

➡️ **_`$merge` Stage Placement_**

_merge with the existing document_

```mongodb
db.test.aggregate([
    // stage 1
    { $match: { gender: 'Male' } },
    // stage 2
    { $addFields: { course: "level_2", eduTech: 'Programming Hero' } },
    // stage 3
    { $merge: "test" }
])
```

➡️ **_`$group` Stage Placement_**

```mongodb
db.test.aggregate([
    //stage_1
    { $group: { _id: "$gender" } },

])
```

⤵️

```mongodb
db.test.aggregate([
    //stage_1
    { $group: { _id: "$address.country", count: { $sum: 1 } } },

])
```

⤵️

```mongodb
db.test.aggregate([
    //stage_1
    { $group: { _id: "$address.country", count: { $sum: 1 } ,name: {$push: "$name"}} },

])
```

_need all the document_

```mongodb
db.test.aggregate([
    //stage_1
    { $group: { _id: "$address.country", count: { $sum: 1 } ,allDocuments:{$push: "$$ROOT"}} },

])
```

**_get specific data from the document_**

```mongodb
db.test.aggregate([
    //stage_1
    { $group: { _id: "$address.country", count: { $sum: 1 } ,allDocuments:{$push: "$$ROOT"}} },
    // stage_2
    {$project: {
        "allDocuments.name":1,
        "allDocuments.email":1,
    }},
])
```

**_Group aggregation operator_**

```mongodb
db.test.aggregate([
    //stage_1
    {
        $group: {
            _id: null,
            totalSalary: { $sum: "$salary" },
            maxSalary: { $max: "$salary" },
            minSalary: { $min: "$salary" },
            avgSalary: { $avg: "$salary" }
        }
    },
    // stage_2
    {
        $project: {
            totalSalary: 1,
            maxSalary: 1,
            minSalary: 1,
            averageSalary: "$avgSalary",
            rangeBetweenMaxMin: {
                $subtract: ["$maxSalary", "$minSalary"]
            }
        }
    }
])
```

👊 **_`$group` with `$unwind` aggregation stage_**

_`$unwind` helps to break the array and create a single document or object of an array_

```bash
db.test.aggregate([
    // stage_1
    {
        $unwind: "$friends"
    },
    //  stage_2
    {
        $group: {
            _id: "$friends",
            count: {
                $sum: 1
            }
        }
    }
])
```

⤵️

```mongodb
db.test.aggregate([
    // stage_1
    {
        $unwind: "$interests"
    },
    // stage_2
    {
        $group: {
            _id: "$age",
            interestPerPage: {
                $push: "$interests"
            }
        }
    }
])
```

👊 **_`$bucket` (aggregation)_**

```mongodb
db.test.aggregate([
    //stage_1
    {
        $bucket: {
            groupBy: "$age",
            boundaries: [20, 40, 60, 80],
            default: "greater than 80",
            output: {
                count: {
                    $sum: 1
                },
                remain: {
                    $push: "$$ROOT"
                }
            }

        }
    },
    //stage_2
    {
        $sort: {
            count: -1
        }
    },
    //stage_3
    {
        $limit: 2
    },
    //stage_4
    {
        $project: {
            count: 1
        }
    }
])
```

💪 **_`$facet` Multiple pipeline aggregation stage_**

_Make aggregate pipeline more powerful_

```mongodb
db.test.aggregate([
    {
        $facet: {
            // pipeline1
            "friendsCount": [
                //stage_1
                {
                    $unwind: "$friends"
                },
                //stage_2
                {
                    $group: {
                        _id: "$friends", count: {
                            $sum: 1
                        }
                    }
                }
            ],
            //pipeline 2
            'educationCount': [
                //stage_1
                {
                    $unwind: "$education"
                },
                //stage_2
                {
                    $group: {
                        _id: "$education", count: {
                            $sum: 1
                        }
                    }
                }
            ],
            //pipeline_3
            'skillsCount': [
                //stage_1
                {
                    $unwind: "$skills"
                },
                //stage_2
                {
                    $group: {
                        _id: "$skills", count: { $sum: 1 }
                    }
                }
            ]
        }
    }
])
```

| **Embedded Data**                 | **_Reference Data_**              |
| --------------------------------- | --------------------------------- |
| **_One to one Relationships_**    | **_*One to Many Relationships*_** |
| **_*Frequent Reading Data*_**     | **_*Many To Many*_**              |
| **_*Atomic Updates*_**            | **_*Frequent Writing*_**          |
| **_*Reducing Network Overhead*_** | **_*Big Data Size*_**             |
| **_*Small Size*_**                | **_*Scalability & Flexibility*_** |

👊 **_`lookup` stage when we use referencing then our collection will be two or more.So need to join the use `lookup` and return to the user._**

```mongodb
db.orders.aggregate([
    {
        $lookup: {
            from: "test",
            localField: "userId",
            foreignField: "_id",
            as: "userOrders"
        }
    }
])
```

👊 **_IXSCN_**

**_Create index_**

```mongodb
db.getCollection("massive-data").createIndex({email:1})
```

**_Delete index_**

```mongodb
db.getCollection("massive-data").dropIndex({email:1})
```
