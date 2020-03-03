# mongo-db-learnings
Mongo DB is a Schema Free Database it is based on the Binary JSON
Mongo DB is a opensource, uses to

Difference between relational and no sql?

tables => collections
rows => documents
field => column

collections is not strict about what should go inside.

use myDatabaseName => If the database is not present, it will create the new database and use it. else it will use it
db => this will show the database which we are using now
show dbs  => will list out all the dbs present
db.dropDatabase() => will drop or delete the database.

// create and delete collection

db.createCollection("myCollectionName") (or) // for creating new collection
db.myCollectionName.insert({"jsonKey":"jsonValue"}); // if the collection is not present create the collection else use the collection insert the values in collection 
db.myCollectionName.insert([{"jsonKey":"jsonValue"},
{"jsonKey1":"jsonValue1"},{"jsonKey2":"jsonValue2"}]); // for Bulk Insertion
show collections => to check the list of collections we have
db.myCollectionName.drop() => delete the collection

// Querying the Document
db.myCollectionName.find() => this is an equivalent to "select * from myCollectionName"
db.myCollectionName.find().pretty()
db.myCollectionName.find({"empCode":"20"})
db.myCollectionName.find({"age":{$gt:"20"}}) // $gte , $lt, $lte, $ne 
db.myCollectionName.findOne() // first record will be fetched

// And operator (, operator will work as and)
db.myCollectionName.find({"firstName":"Mark", "age":"20"})
// Or operator  $or then value should be list of objects
db.myCollectionName.find( $or:[{"firstName":"Mark"}, {"age":"15"}])

// And + Or
db.myCollectionName.find({"firstName":"Mark",
			$or:[{"age":"16"}, {"age":"15"}]
			})
			
// Update the Document 
db.myCollectionName.update({"age":"20"},
{$set:{"lastName":"Updated lastname"}});
// for multiple records to be updated
db.myCollectionName.update({"age":"20"},
{$set:{"lastName":"Updated lastname"}},
{multi:true}
);
db.myCollectionName.save({"id":"1", "firstName":"Mark", "age":"20"}) // if the record is there it will update else it creates.

// delete documents
db.myCollectionName.remove({"Age":"16"}) // for all records.

db.myCollectionName.remove({"Age":"16"},1) // to specify no of records.

//Projection
selecting only necessary data 
db.myCollectionName.find({}, {"firstName":1}) // shows all records with 2 columns firstName and _Id
db.myCollectionName.find({}, {"firstName":1, "_id":0}) // shows all records with only one column firstName
 
here 1 means true (include) 0 means false(exclude)

//We can limit the no of values or records to display
db.myCollectionName.find({}, {"firstName":1, "_id":0}).limit(4) // it will show first 4 records
db.myCollectionName.find({}, {"firstName":1, "_id":0}).skip(2) // first 2 records it will skip
db.myCollectionName.find({}, {"firstName":1, "_id":0}).skip(2).limit(3)
db.myCollectionName.find({}, {"firstName":1, "_id":0}).sort({"firstName":1}) // 1 represents ascending order
db.myCollectionName.find({}, {"firstName":1, "_id":0}).sort({"firstName":-1}) // -1 represents descending order

// Create Indexes. Indexes helps to fetch the data faster.
db.myCollectionName.ensureIndex({"studentUniq_id":1})
// delete index
db.myCollectionName.dropIndex({"studentUniq_id":1})

// Aggregation  $sum, $max, $min, 
db.myCollectionName.aggregate([{$group:{_id:"$genderFieldName", MyResult:{$sum:1}}}])
db.myCollectionName.aggregate([{$group:{_id:"$genderFieldName", MyResultWithMaxAge:{$max:"$Age"}}}])

// Restore and backup database
go to the path where the mongo server resides
use myDbName
c:/program files/ MongoDB/ Server/3.2/bin>
mongodump => will backup all the databases
mongodump --db myDbName => will backup only one databases
mongorestore => will backup all the databases
mongorestore --db myDbName => will backup all the databases
mongorestore --db myDbName dump/myDbName => will backup all the databases with the path where it is going to restore from

// Backup only one collection from database.
mongodump --db myDbName --collection myCollectionName => will backup only one collection from database
mongorestore --db myDbName --collection myCollectionName dump/myDbName/myCollectionName.bson => will restore only one collection from database  have to mention the path
