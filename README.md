# Learning MongoDB

## Docker: Install Docker Toolbox
  * VirtualBox is automatically installed if not installed.
  * Kitematic (UI) and Docker Quickstart Terminal (CLI) are included in the Docker Toolbox

## Docker: Create docker VM
```
% set "PATH=C:\Program Files\Docker Toolbox;C:\Program Files\Docker Toolbox\kitematic;C:\Program Files\Docker Toolbox\kitematic\resources\resources;%PATH%"
% docker-machine rm -f default
% docker-machine create -d virtualbox --virtualbox-cpu-count 8 --virtualbox-memory 64000 --virtualbox-disk-size 64000 default
```

## Docker: Set up connection to docker VM
```
% FOR /f "tokens=*" %i IN ('docker-machine env default') DO @%i // for Command Prompt
REM FOR /f "tokens=*" %%i IN ('docker-machine env default') DO @%%i // for .bat file
% ping default
% docker-machine ip
% docker-machine status
```

## Docker: Create containers
  * Run hello-world container

	```
	docker run -it --rm hello-world
	```

  * Run mongodb server container in background mode
  	```
  	docker run -d --name mongodb-server -e ALLOW_EMPTY_PASSWORD=yes -e MONGODB_ROOT_USER=root -e MONGODB_ROOT_PASSWORD=password -e MONGODB_DATABASE=test -e MONGODB_USERNAME=test -e MONGODB_PASSWORD=test -e AUTH=no -p 27017:27017 -p 27018:27018 bitnami/mongodb	
  	```
  
  * Run mongosh container and connect to above container
  	```
	docker run -it --rm --name mongosh --link mongodb-server:mongodb-server bitnami/mongodb mongosh mongodb://mongodb-server/test -u test -p test
	```

## MongoDB: DDL
  * Use test database
	```
	show dbs
	use test
	```

  * Creating a collection
	```
	db.createCollection("catalog")
	show collections
	```

  * Dropping a collection
	```
    db.catalog.drop()
	```

## MongoDB: DML

  * Creating a document
	```
    doc1 = { "catalogId": "catalog1", "journal": 'Oracle Magazine', "publisher": 'Oracle Publishing', "edition": 'November 2013',"title": 'Engineering as a Service',"author": 'John Doe'}
    db.catalog.insert(doc1)
    doc2 = { "catalogId": "catalog2", "journal": 'PC Magaznie', "publisher": 'Computserve', "edition": 'December 2013',"title": 'Big deals!', "author": 'Jane Doe'}
    db.catalog.insert(doc2)
    db.catalog.countDocuments()
	```

  * Updating a document
	```
    db.catalog.updateOne({ "catalogId": "catalog2" }, {$set: { "author": "Jane Doe - One" }})
    db.catalog.updateMany({ "catalogId": "catalog2" }, {$set: { "author": "Jane Doe - Many" }})
	```

  * Removing documents
	```
	db.catalog.remove({_id: ObjectId("5a0d1600f11aa8f0e0eac21a")})
	```

  * Removing all documents
	```
    db.catalog.remove({})
	```

## MongoDB: Query
  * Finding a matching first doc
	```
    db.catalog.findOne()
	db.catalog.findOne({ "catalogId": "catalog2" })	
	```
  
  * Filtering all matching docs
	```
	db.catalog.find()
	db.catalog.find({ "catalogId": "catalog1" })
	```

  * Select columns
	```
    db.catalog.find({}, { "catalogId": 1, "author": 1, "_id": 0 })
    db.catalog.find({ "catalogId": "catalog2" }, { "catalogId": 1, "author": 1, "_id": 0 })
	```
