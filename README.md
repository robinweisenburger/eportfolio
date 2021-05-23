# E-Portfolio MongoDB

Welcome to my E-Portfolio. I will give you an introduction to MongoDB which is the leading NoSQL Database. 

![MongoDB Logo](./mongodb.png)

The project requires the installation of a MongoDB Server. The MongoDB Compass and the Mongo Shell are also required. 

## Install MongoDB Server

This instruction guide only covers the basic Server installation! More details on how to setup and configure the server will be given during the presentation. I recommend installing the server yourself on your pc or on a linux server.

### Windows

On Windows the community server installer which can be downloaded here: https://www.mongodb.com/try/download/community will install the server itself and the shell! So you don't need to install the shell on top of the community server.

### 1. Use MongoDB Atlas

If you don't want to install a MongoDB Server yourself on your PC you can use a free MongoDB Server which is hosted in the cloud. To get a server you will need to create an account and order a server. MongoDB offers a small server for free!

Sign up and create the server here: https://www.mongodb.com/cloud/atlas

To access the server you will need the connection details which look like the following strings. Before you can access the server you will need to add your ip to a whitelist. This can be done in the Network Access settings at the MongoDB cloud interface.

For MongoDB Compass:

```
mongodb+srv://<username>:<password>@cluster0.5dj9j.mongodb.net/<databasename>
```

For MongoDB Shell:

```
mongo "mongodb+srv://cluster0.5dj9j.mongodb.net/<databasename>" --username <username>
```



### 2. MongoDB Server on Linux, Mac or Windows

The server installation files can be downloaded at: https://www.mongodb.com/try/download/community. On Windows the Server runs as a service. So you don't need to start it manually. During the presentation I will show how to install a MongoSB Server on Ubuntu Linux. Therefore there is no more information here at this point.

## Install MongoDB Compass

MongoDB Compass is a graphical user interface for interacting with a mongo database. It also can be used to analyze stored data and execute commands on the server. It is available for Linux, Mac and Windows. It can be downloaded here: https://www.mongodb.com/try/download/compass

## Install MongoDB Shell

In the presentation I will focus on the MongoDB shell. It is available for Linux, Mac and Windows. On Windows the shell will be installed automatically with the community server. So in this case you don't need to do anything. If you install the server on Linux the shell is also part of the server installation. 

Access the Shell on Windows:

```
C:\Program Files\MongoDB\Server\4.4\bin\mongo.exe
```

Install MongoDB Shell with Brew (if not already installed with the server)

```sh
1. brew tap mongodb/brew
2. brew install mongodb-community-shell
```

Access the Shell on Linux:

```sh
mongo
```

## Cheat Sheet

The basic MongoDB shell commands are listed in the cheat sheet which can be found here: [Link](./cheat_sheet.md)

## Test Data

During the presentation I will use some Mock Data to show how MongoDB deals with it. This Mock Data can also be used by you. Therefore the test files (airports.json, flights.json, passengers.json) can be downloaded in this git repository. 

## Want to use MongoDB in your own project?

For almost every popular programming language MongoDB provides "drivers". These are Frameworks which adds MongoDB compatibility to the application or project. With these drivers it is possible to execute commands directly on the MongoDB server and for example insert information into the databse. Feel free to check out the drivers of the programming language you are using in your projects: https://docs.mongodb.com/drivers/

## More Information

More detailed information and examples can be found in the MongoDB documentation: https://docs.mongodb.com/
