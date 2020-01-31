# Mongo Database

## Lesson Objectives

1. Describe the purpose of databases in web application development.
1. Distinguish between the database server, a database, a collection and a document in MongoDB.
1. Create, drop and interact with MongoDB databases and collections using the MongoDB shell.
1. Create, Read, Update, and Delete documents in MongoDB collections using the MongoDB shell.

# What is a Database?

A database is an organized collection of data, stored and accessed electronically.  There are many kinds of databases. A common type is a RDBMS (relational database management system).  Relational databases are frequently based on SQL (Structured Query Language)  and store data in tables and rows, much like an Excel spreadsheet/Google Sheet.

Another type of database is a NoSQL database, which don't store data in rows and columns.  In this unit, we'll be using MongoDB which is a very popular NoSQL database for building web applications, and in particular, building web applications that need to store a lot of data.

## What is Mongo?

MongoDB is a specific type of NoSQL database system called a  **document database**.  It stores data in a format called BSON, which is a JSON-like format. One of the key benefit of this format is that it is very easy to work with using JavaScript because it so closely resembles plain old JavaScript objects!

MongoDB is also much more scalable than relational databases due to this mechanism of data storage.  It means that we can store the data easily across many physical pieces of hardware since each "record", is a separate and distinct "document".

## MongoDB Databases, Collections and Documents

MongoDB is an application that runs as a **server** and can have many separate databases within it.

Each MongoDB **database** consists of collections.

Each **collection** is a set of documents.

Each **document** contains a set of data and attributes, known as fields.

This is very abstract. Let's use a simplified real world example of an address book.

Here is one document:

```js
{
 firstName: "Jennifer",
 lastName: "Juniper",
 address: "Upon the Hill",
 state: "California",
 telephone: "867-5309",
 birthday: "June 8, 1968",
 email: "jenny.juniper@juno.net"
}
```

A collection would be many documents: In our case, many contacts.

Remember: having a collection of documents sounds quite reasonable. But having a document of collections is ... kind of odd.

> If you're coming from a background where you are used to thinking of data in terms of columns and rows, then you can correlate a table (or worksheet) to the collections, a row to the documents and columns to the fields in MongoDB.

## Make sure Mongo is Running

Let's [follow the instructions](https://git.generalassemb.ly/seir-129/mongo-install-homework#verify-that-mongodb-is-installed) in last night's homework to make sure that our MongoDB server is running locally.  If it's not started (or if you're on a MacOS and started appears in yellow not green), restart your server now.

## Connect via Mongo Shell

To access our MongoDB directly, we can type `mongo` at the command prompt.  This will run a REPL for interacting with MongoDB. When you type `mongo` in the Terminal, you should see a message that tells you the MongoDB shell is running and the prompt will change to a greater than (`>`) character.  If you do not see this, inform an instructor!

## Connect/Create to a Database

Let's keep working on our Mongo Shell.

Let's see what databases we have available to us:

```
show dbs
```

We want to create and use a database called `learn`. With Mongo, if a database doesn't exist and we try to use it, Mongo will automatically create it for us, so we'll type:

```
use learn
```

It will create a database called `learn` and connect to it.

It is likely that our configuration let's us see the db name at our prompt, but in case it doesn't or we want a reminder we can type

```
db
```

To see the database we are currently connected to.

## Create a Collection

For today, we'll only be working with one collection, but most apps will have numerous collections.

Let's think about an online store. You might split up the collections like so:

```
- users
    - username
    - password
    - address
    - creditCardInfo
    - phoneNumber

- products
    - productName
    - catalogNum
    - imageLink
    - price
    - inStock
```

This helps us organize our data.

Let's go back to our address book example and create a collection of contacts.

```js
db.createCollection("contacts");
```

We should get an `ok` message, if we've done it correctly.

We can see what collections we have by typing

```js
show collections
```

## Create, Read, Update and Delete Documents

In computer programming, create, read, update, and delete (CRUD) are the four basic functions of persistent storage. Let's learn how to CRUD documents using MongoDB.

> Obviously users are not going to open a Mongo shell and type everything we're going to type. This lesson is meant to help us understand how MongoDB works.  Eventually be building apps to interact with our database.

### Insert a document into a Collection (Create)

- We'll use the `insert()` method.
- We have to tell MongoDB where to insert it. We'll do that by chaining it to `db.contacts`
- It takes two arguments.
- The first is always an object of our data.
- The second is optional and can let us choose some specific options.

Insert into contacts:

```js
db.contacts.insert();
```

Pass in an object as the first argument

```js
db.contacts.insert({});
```

Add some key value pairs, for Jennifer. We're going to split it up across multiple lines to make it easier to type and see

```js
db.contacts.insert({
  name: "Jennifer",
  phone: 8675309,
  state: "California"
});
```

We can also type our code in VS Code and when we know it's right, copy and paste it over into our Mongo shell. Go with whatever is easier.

Let's go ahead and copy paste this into our Mongo shell to populate our collection with more documents

```js
db.contacts.insert([
  {
    name: "Jennifer",
    phone: 8675309,
    state: "California"
  },
  {
    name: "Claire",
    phone: 6060842
  },
  {
    name: "Morris",
    phone: 7779311,
    state: "Minnesota"
  },
  {
    firstName: "Alicia",
    lastName: "Keys",
    phone: 4894608,
    state: "New York"
  },
  {
    name: "Etta",
    phone: "842-3089",
    state: "California"
  }
]);
```

We may notice that our data wasn't consistent.

- Jennifer has a duplicate record
- Claire, doesn't have a state
- Alicia's key's are different for her name than others, she also has an extra field for her last name, compared to others.
- Etta's phone number is a string with a hyphen instead of a number

Mongo is designed to be this flexible. Later, we'll learn how to validate our data with an npm package called `mongoose`.

### Query Documents from a Collection(READ)

We'll use the `.find()` method.

We'll do some simple queries. If we provide no argument, it will find all the documents.

Let's try it

```js
db.contacts.find();
```

We may find that to not be as human-readable as we'd like, we can chain another function on it

```js
db.contacts.find().pretty();
```

Many times, we don't want to find all.

We might want to just find the names of the people who live in California.

We can give our `.find()` method some arguments. The first argument will be a `filter` and the second argument will be a `projection` the project will be the key, it can have a value of 0 (do not show this field) or 1 (do show this field).

When we skip the second argument, we see the whole document:

```js
db.contacts.find({ state: "California" }).pretty();
```

Let's look for the names of people who are in the state of California, and let's not show the `_id` field. We'll add a second argument.

```js
db.contacts.find({ state: "California" }, { name: 1, _id: 0 }).pretty();
```

### Remove Documents from a Collection (DELETE)

Let's remove that duplicate record. We'll use a method called `.remove()`, it takes two arguments, the first is a query (what document are we looking for? - Jennifer's), the second one gives us options

```js
db.contacts.remove({
  name: "Jennifer"
});
```

Let's put Jennifer back again twice:

```js
db.contacts.insert({
  name: "Jennifer",
  phone: 8675309,
  state: "California"
});
db.contacts.insert({
  name: "Jennifer",
  phone: 8675309,
  state: "California"
});
```

We should see we did it successfully with the message that we get.

But we can also run a query:

```js
db.contacts.find({ name: "Jennifer" }).pretty();
```

And Let's try to remove again. This time we're going to pass a second argument that will be an object that has the key values of `justOne: true`

```js
db.contacts.remove(
  {
    name: "Jennifer"
  },
  { justOne: true }
);
```

We should have a success message that reads like the following:

```
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```

Let's use our `UP arrow` to scroll back to our .find() for Jennifer and check that we now have just one record.

```js
db.contacts.find({ name: "Jennifer" }).pretty();
```

### Update a document (Update)

Like `.remove()`, update takes a query for what to update. But it is also REQUIRED to use an **[update operator](https://docs.mongodb.com/manual/reference/operator/update/)** as part of the second argument in order to prevent destroying our object.

Let's update Jennifer's record to have the name Jenny instead

```js
db.contacts.update({ name: "Jennifer" }, { name: "Jenny" });
```

Success looks like this:

```js
Updated 1 existing record(s) in 35ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

- we have number matched (nMatched) equal to 1
- we have number upserted (nUpserted) equal to 0 (upsert means if it doesn't exist, create it, we did not create anything this time)
- we have number modified (nModified) equal to 1, which means we modified 1 records.

Let's push the up arrow and run our last command again:

```js
db.contacts.update({ name: "Jennifer" }, { name: "Jenny" });
```

This time we get:

```js
WriteResult({
  nMatched: 0,
  nUpserted: 0,
  nModified: 0
});
```

We have no matches, no upserts and nothing modified. This is what we expect, since we changed this record the first time we ran it.

Let's find Jenny

```js
db.contacts.find({ name: "Jenny" });
```

We lost the rest of Jenny's record!

![Jenny's Destroyed record](https://i.imgur.com/7lM5jyB.png)

This is because we didn't use an update operator. In order to keep our data intact with an update we must use an update operator.

Let's remove and reinsert Jenny's record and try again

```js
db.contacts.remove({
  name: "Jenny"
});
```

```js
db.contacts.insert({
  name: "Jennifer",
  phone: 8675309,
  state: "California"
});
```

Let's use the `$set` update operator this time

```js
db.contacts.update(
  { name: "Jennifer" },
  {
    $set: { name: "Jenny" }
  }
);
```

Since our data set is very small, let's just look at all of our records

```js
db.contacts.find().pretty();
```

We can add a field. Claire has no state, let's give her a state

```js
db.contacts.update(
  { name: "Claire" },
  {
    $set: { state: "California" }
  }
);
```

We can push the up arrow to rerun

```js
db.contacts.find().pretty();
```

And we should see that Claire now has a state, so we don't have to query for the field that we want to change, we can query for any match.

Because of this, our objects can be ever changing. The way we can reliably be sure we are always getting the right document, is to use the unique id number attributed to each document on creation. Typing these long ids are tough for a code along, but when we start making our express CRUD apps, we'll definitely be using the id numbers a lot.

By default, update will only update one record

```js
db.contacts.update({}, { $set: { bestFriend: true } });
```

Press the up arrow to run

```js
db.contacts.find().pretty();
```

As we can see, just one record was updated. Let's try to update all of our records, by adding a third argument to our `.update()` method

```js
db.contacts.update({}, { $set: { bff: true } }, { multi: true });
```

Press the up arrow to run

```js
db.contacts.find().pretty();
```

### Search for Multiple Values

We can query for multiple values. In our contacts, let's query for people who live in California and are named Etta

```js
db.contacts.find({
  name: "Etta",
  state: "California"
});
```

### Search by Quantitative Data

We can search for equal to, not equal to, greater than, less than or equal to, included in an array etc.

[query operators](https://docs.mongodb.com/manual/reference/operator/query/)

Let's just try one together. Let's query for the people who are NOT in California

```js
db.contacts.find({
  state: { $ne: "California" }
});
```

### Drop a collection

If you need to remove an entire collection

```
db.contacts.drop()
```

If you need to drop an entire database, while you are connected to the database you want to drop:

```
db.dropDatabase()
```

### Exit and Return to the Command Prompt

To quit out of the Mongo shell type `exit` or `control c`

### Bonus Configuration

Update your mongo shell to always show pretty

Anywhere in bash

```bash
echo DBQuery.prototype._prettyShell = true >> ~/.mongorc.js
```

Turn it off

```bash
echo DBQuery.prototype._prettyShell = false >> ~/.mongorc.js
```
