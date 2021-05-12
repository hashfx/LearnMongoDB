# Table of Content
[Starting with MongoDB](#Starting-with-MongoDB)<br>
[CREATE](#CREATE) <br>
[READ](#READ) <br>
[UPDATE](#UPDATE) <br>
[DELETE](#DELETE) <br>
[All Commands](#All-Commands) <br>

# Starting with MongoDB

[Download MongoDB for Windows or other OS](https://www.mongodb.com/try/download/community) <br>
&nbsp;

## Check Version of MongoDB after Installation [mongo --version]

```powershell
[In]    mongo --version
[op]    MongoDB shell version v4.4.4
        Build Info: {
            "version": "4.4.4"
        }
```
&nbsp;
<h2 align="center"> <u>SQL to MongoDB Comprehension</u> <br><br>

|SQL | &nbsp;MongoDB|
|-----|-----|
|RDBMS |  MongoDB|
|Database | Database|
|Table	    |  Collection|
|Tuple/Row	|  Document|
|column	    |  Field|                                          
|Table Join	|  Embedded Documents|         
|Primary Key	|  Primary Key |
-------
### (Default key _id is provided by MongoDB itself as Primary Key)
-------
&nbsp;&nbsp;

## Reference
+ **Editor:** Windows Command Prompt **[** official CLI **]**
+ [In] _means_ **Input**
+ [op] _means_ **Output**

## To start MongoDB

```powershell
[In]    > mongo
[op]      Server Starts...
```

## To exit MongoDB
```powershell
[In]    > ^c^c  [{ctrl+c} + {ctrl+c}]
[In]    > quit()
[op]      bye
```

# CRUD (CREATE, READ, UPDATE, DELETE)

## check databases list: [Command: show dbs]
```powershell
[In]    >show dbs
[op]     admin      0.000GB
[op]     config     0.000GB
[op]     local      0.000GB 
# Only those dbs will be shown which have a collection in them
```

## check current active db:
```powershell
[In]    >db
[op]     db_name
```

# CREATE

## SYNTAX  

```powershell
db.collection_name.insertOne({field:value}) 
db.collection_name.insertMany({field:value})
```

## To Create OR Switch to a particular db

```powershell
[In]    >use mydb # mydb is db name to be switched or created
[op]    Switched to db mydb
```

## To Create/add a single collection in current database

```powershell
[In]    >db.collection1.insertOne({ name:"Name1",num:20,bool:true }) 
[op]
	{
		"acknowledged" : true,
		"insertedId" : ObjectId("605473fe4f8600f829bbe070")
	}
```

## To check if db is created or not

```powershell
[In]    >show dbs
[op]     admin     0.000GB  ---
[op]     config    0.000GB    |---> Default DBs
[op]     local     0.000GB  ---  
[op]     mydb      0.000GB  ---> DB created by user which has (collection in it) & (is not empty)
```

## To add multiple collections in current db in one go

```powershell
[In]    >db.collection1.insertMany( [ {name:"Name2",num:50,bool:true}, {name:"Name3",num:40,bool:false}, {name:"Name4",num:80,bool:true} ] )    
[op]
	{
			"acknowledged" : true,
			"insertedIds" : [
					ObjectId("60547ad70e1b8949ba245f81"),
					ObjectId("60547ad70e1b8949ba245f82"),
					ObjectId("60547ad70e1b8949ba245f83")
			]
	}
```

## To see collections of mydb

```
[In]    >show collections
[op]     collection1
```

# READ

```powershell
SYNTAX : db.collection_name.find(query, projection)
```

## To see documents inside collection1

```powershell
[In]    > db.collection1.find()
[op]    { "_id" : ObjectId("605473fe4f8600f829bbe070"), "name" : "Name", "num" : 10, "bool" : true }
{ "_id" : ObjectId("60547ad70e1b8949ba245f81"), "name" : "Name2", "num" : 50, "bool" : true }
{ "_id" : ObjectId("60547ad70e1b8949ba245f82"), "name" : "Name3", "num" : 40, "bool" : false }
{ "_id" : ObjectId("60547ad70e1b8949ba245f83"), "name" : "Name4", "num" : 80, "bool" : true }
```

## To see documents inside collection1 in formatted way

```powershell
[In]    > db.collection1.find().pretty()
[op]
{
        "_id" : ObjectId("605473fe4f8600f829bbe070"),
        "name" : "Name",
        "num" : 10,
        "bool" : true
}
{
        "_id" : ObjectId("60547ad70e1b8949ba245f81"),
        "name" : "Name2",
        "num" : 50,
        "bool" : true
}
{
        "_id" : ObjectId("60547ad70e1b8949ba245f82"),
        "name" : "Name3",
        "num" : 40,
        "bool" : false
}
{
        "_id" : ObjectId("60547ad70e1b8949ba245f83"),
        "name" : "Name4",
        "num" : 80,
        "bool" : true
}
```

## Read all documents

```powershell
[In]    >db.collection1.find().pretty()
[op]     <list of all documents>
```

## Get document with query &lt;name: "Name"&gt; as output

```powershell
[In]    >db.collection1.find({name:"Name"})    
[op]     
	{ "_id" : ObjectId("605473fe4f8600f829bbe070"), "name" : "Name", "num" : 10, "bool" : true }
```

## Get &lt;num:10&gt; field only from document as output

```powershell
[In]    >db.collection1.find({num:10}, {name:1})
[op]     { "_id" : ObjectId("605473fe4f8600f829bbe070"), "name" : "Name" }
```

## Get &lt;num:10&gt; field only from document as output without _id field

```powershell
[In]    >db.collection1.find({num:10}, {_id:0})
[op]    { "name" : "Name", "num" : 10, "bool" : true }
```

## set query to <mark style="background-color: pink;">"bool:true"</mark> and get only first field with "active:true" value

```powershell
# to get first output only after query is performed
[In]    >db.collection1.find({bool:true}).pretty().limit(1) 
[op]     
	{ "_id" : ObjectId("605473fe4f8600f829bbe070"), "name" : "Name", "num" : 10, "bool" : true }
```

### ----------Alternate Method----------

```powershell
# to get first output only after query is performed w/o limit() function
[In]    >db.collection1.findOne({bool:true}) 
[op]
     { "_id" : ObjectId("605473fe4f8600f829bbe070"), "name" : "Name", "num" : 10, "bool" : true }
```

## set query to "bool:true" and get second field with "active:true" value by skipping 1st field

```powershell
[In]    >db.collection1.find({bool:true}).limit(1).skip(1)
[op]     
	{ "_id" : ObjectId("60547ad70e1b8949ba245f81"), "name" : "Name2", "num" : 50, "bool" : true }
```

# UPDATE

## SYNTAX

```powershell
db.collection_name.updateOne/Many(filter, update)
```

## Update &lt;name:"Name"&gt; to &lt;name:"Name1"&gt;

```powershell
[In]    >db.collection1.updateOne({name:"Name"},{$set:{name:"Name1"}})
[op]    { "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
```

##  Change queries with &lt;bool:true&gt; to &lt;bool:false&gt;

```powershell
[In]    >db.collection1.updateMany({bool:true},{$set:{bool:"false"}})
[op]     { "acknowledged" : true, "matchedCount" : 3, "modifiedCount" : 3 }
```

# DELETE

## SYNTAX 
```powershell
db.collection_name.deleteMany(DeletionCriteria)
```

## Delete field with &lt;num:40&gt;

```powershell
[In]    >db.collection1.deleteMany({num:40})
[op]     { "acknowledged" : true, "deletedCount" : 1 } 
```

##  To Delete all Documents in Collection

```powershell
[In]    >db.collection1.deleteMany({})
[op]     { "acknowledged" : true, "deletedCount" : 3 } 
```

# All Commands

# CREATE

```powershell
# to add one document to collection
db.collection_name.insertOne({field:value}) 
# to add many documents to Collection
db.collection_name.insertMany({fields:values},{fields:values},{fields:values}.....) 
```

# READ


```powershell
# SYNTAX
db.collection_name.find(query, projection)
# to display all documents of collection
db.collection_name.find() 
# 1 to display; 0 to hide
db.collection_name.find({query}, {query_to_display}) 
# to display all documents of collection
db.collection_name.find().pretty() 
# to get first n fields which satisfies the query
db.collection_name.find({query}).pretty().limit(n)
# number of initial fields to be skipped 
db.collection_name(query).limit(number_of_results_to_be_shown).skip(fields_to_be_skipped)
```

# UPDATE
```powershell
#SYNTAX 
db.collection_name.updateOne/Many(filter, update) 
# to Update One Field; it will change the first field with query match
db.collection_name.updateOne({field:value},{$set:{new_field:new_value}})
# To Update fields with same query with new data
db.collection_name.updateMany({field:value},{$set:{new_field:new_value}}) 
```

# DELETE

```powershell
# SYNTAX 
# To delete documents which matches the query specified inside parentheses.
db.collection_name.deleteMany(DeletionCriteria)
# To delete all documents of collection
db.collection_name.deleteMany({}) 
```


https://www.tutorialspoint.com/mongodb/mongodb_create_database.htm
