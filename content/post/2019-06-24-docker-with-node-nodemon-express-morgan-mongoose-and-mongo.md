+++
type = "post"
title = "Docker with Node.js and MongoDB (using Nodemon, Express, Morgan and Mongoose)"
summary = "This post will explain how to use docker-compose to create the environment to use Node.js and MongoDB with Docker."
date = 2019-06-24T18:25:28+02:00
categories = ["docker"]
tags = ["node", "express", "morgan", "mongoose", "mongo", "docker"]
image = "images/beverage-business-classic-2305765.jpg"
edit = ""
draft = false
+++
> **IMPORTANT**: For this tutorial you will need to have installed `docker`, `docker-compose`and `npm`. 

<br />

This tutorial will show you how to build a really flexible dockerized server running `node.js` and `mongodb`. Using `docker-compose` this way, you will be able to edit your server on the fly. You will use two images and you won't need to create a new one thanks to the use of volumes.

Ok, so lets get started.

<br />

**Folder structure**

This will be the folder structure that we will use.

{{< highlight Plaintext >}}
mongo-project/
├── package/
│   ├── node_modules/
│   ├── src/
│   │   ├── database.js
│   │   └── index.js
│   └── package.json
└── docker-compose.yml
{{< / highlight >}}

> The names of the folders and containers is up to you, except for the files `docker-compose.yml`, `package.json` and the folder `node_modules`.

<br />

**First steps to build this structure**

{{< highlight Bash >}}
$ mkdir mongo-project # Create the project folder
$ cd mongo-project    # Go inside the project folder
$ mkdir package       # Create the folder package
$ cd package          # Go inside the package folder
$ mkdir src           # Create the folder src
$ npm init --yes      # Creates the package.json file
$ npm i express mongoose morgan --save # Installs express, mongoose and morgan and adds them to the package.json
$ npm i nodemon -D    # Installs nodemon and adds it to the package.json as a development dependency
{{< / highlight >}}

<br />

**package.json**

At this point your `package.json` file will look like this:

{{< highlight JSON >}}
{
  "name": "package",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1",
    "mongoose": "^5.6.0",
    "morgan": "^1.9.1"
  },
  "devDependencies": {
    "nodemon": "^1.19.1"
  }
}
{{< / highlight >}}

<br />

Now you can edit the `name`, `description` and `author` values as you wish.

In the `scripts` section you need to add this `start` script: `"start": "nodemon src/index.js"`. And delete the `test` script.

Now you will have something like this:

{{< highlight JSON >}}
{
  "name": "node-server",
  "version": "1.0.0",
  "description": "Node.js server",
  "main": "index.js",
  "scripts": {
    "start": "nodemon src/index.js"
  },
  "keywords": [],
  "author": "Siziksu",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1",
    "mongoose": "^5.6.0",
    "morgan": "^1.9.1"
  },
  "devDependencies": {
    "nodemon": "^1.19.1"
  }
}
{{< / highlight >}}

<br />

**docker-compose.yml**

Now it's time for the `docker-compose.yml` file. Go to the `mongo-project` folder and type `$ touch docker-compose.yml` to create the file. Set the following content.

{{< highlight YAML >}}
version: '3'
services:
  server:
    image: node:12.4.0-alpine
    container_name: node
    working_dir: /server
    volumes:
    - ./package:/server
    command: npm run start
    ports:
    - "8080:80"
    depends_on:
      - mongo
      
  mongo:
    image: mongo:4.0.10
    container_name: mongo
    ports:
      - '27017:27017'
{{< / highlight >}}

> Use the versions for the images of your choice, these are just mine.

<br />

**src folder**

Go to the `src` folder and type `$ touch index.js` and `$ touch database.js` to create the file. Add the following content for both.

*database.js*
{{< highlight JavaScript >}}
const mongoose = require('mongoose')

mongoose.connect('mongodb://mongo:27017/siziksu-db', {
    useCreateIndex: true,
    useNewUrlParser: true,
    useFindAndModify: false
})
.then(db => console.log('Database connected'))
.catch(error => console.error(error))
{{< / highlight >}}

*index.js*
{{< highlight JavaScript >}}
const express = require('express')
const app = express()
const morgan = require('morgan')
const database = require('./database')

app.set('appName', 'Mongo Server')
app.set('port', 80)

app.use(morgan('dev'))

app.get('/', (req, res) => res.end(app.get('appName')))

app.get('*', (req, res) => res.end('File not found'))

app.listen(app.get('port'), () => console.log(`Running on internal port: ${app.get('port')}`))
{{< / highlight >}}

<br />

**Running the server**

Go to the `mongo-project` folder and type `$ docker-compose up` to run the server.

Now, if you go to your browser and type `http://localhost:8080/`, the page will say `Mongo Server`.

<br />

And that's it!
