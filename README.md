# Intro to MongoDB and Mongoose

## [Intro to MongoDB](intro-to-mongo.md)

In this unit, NodeJS will be the server environment that will be responsible for managing all of the request from clients, but we're going to need another server to store data.  This is what we'll be using MongoDB to do.

## MongoDB Lesson Objectives

1. Describe the purpose of databases in web application development.
1. Distinguish between the database server, a database, a collection and a document in MongoDB.
1. Create, drop and interact with MongoDB databases and collections using the MongoDB shell.
1. Create, Read, Update, and Delete documents in MongoDB collections using the MongoDB shell.

---

## [Intro to Mongoose](intro-to-mongoose.md)

MongoDB is a popular database choice for web applications, particularly _at-scale_.  Its flexibility can be a major benefit, and we saw that it allows us to store our data any way we want.  This can also be problematic though because it can introduce inconsistencies in our data and make it harder for us to avoid and track down bugs. To  help in this regard, we'll be using Mongoose. One of its  the key jobs will be to provide us with a bit more control over the data that goes into our database.

## Mongoose Lesson Objectives

1. Describe the purpose of Mongoose in terms of an ODM.
1. Access and manipulate a MongoDB database from a Javascript program by using Mongoose.
1. Validate data for storage in MongoDB via a Mongoose Schema.
