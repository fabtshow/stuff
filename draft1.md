# streamDB

> A Node streams-based JSON document-model DB, with automatically generated REST API endpoints

### Purpose/Why?

1. A sufficient DB for prototyping/simple demos, an npm install away
2. Keep data with the project...no trial expirations, no query limits, no worries
3. A minimal interface built for customizable API access

### Features

- Store data in JSON files
- Simple promise-based CRUD methods (includes geo-search)
- Scaffolded Express Routers, and Mongoose-style schema models
- Launch server with one line of code

## Table of Contents

- [Usage](#Usage)
- [DB Settings](#db-settings)
- [Collections](#collections)
- [Schemas](#schemas)
- [API Documentation](#api)
- <a target="_blank" href="CHANGELOG.md">CHANGELOG</a>
- [Tests](#tests)
- [Stability Notice](#stability-notice) 



## Usage

#### Install:

```sh
$ npm i streamdb
```

#### DB Setup:

```js
const streamdb = require('streamdb')

streamdb.createDb({ dbName: 'myDb' })
  .then(res => console.log(res))
  .catch(e => console.log(e))
```

#### Adding Collections:


```js
const streamdb = require('streamdb')
const db = new streamdb.DB('myDb')

db.addCollection('users')
  .then(res => console.log(res))
  .catch(e => console.log(e))
```

#### Adding Documents:

```js
const streamdb = require('streamdb')
const db = new streamdb.DB('myDb')

const documents = [
  {
    firstname: 'Bugs',
    lastname: 'Bunny',
    email: 'bbunny@email.com'
  },
  {
    firstname: 'Scooby',
    lastname: 'Doo',
    email: 'sdoo@email.com'
  },
  {
    firstname: 'Tom',
    lastname: 'Cat',
    email: 'tcat@email.com'
  }
]

db.collection('users').insertMany(documents)
  .then(res => console.log(res))
  .catch(e => console.log(e))
```

#### Reading Documents:


```js
const streamdb = require('streamdb')
const db = new streamdb.DB('myDb')

db.collection('users').getById(1)
  .then(res => console.log(res))
  .catch(e => console.log(e))

// using queries:
// db.collection('users')
//  .where('id = 1')
//  .and('firstname = Bugs')
//  .find()
//  .then(..)

// Response object: 
//{
//  success: true,
//  data: [
//    {
//      id: 1,
//      firstname: 'Bugs',
//      lastname: 'Bunny',
//      email: 'bbunny@email.com'
//    }
//  ]
//}
```

#### Updating Documents:


```js
const streamdb = require('streamdb')
const db = new streamdb.DB('myDb')

const docUpdate = {
  id: 1,
  email: b-bunny@email.com
}

db.collection('users').updateOne(docUpdate)
  .then(res => console.log(res))
  .catch(e => console.log(e))

// using queries:
// db.collection('users')
//  .where('id = 1')
//  .setProperty('email', 'b-bunny@email.com')
//  .then(..)

// Response object: 
//{
//  success: true,
//  message: 'Document 1 updated successfully'
//  data: [
//    {
//      id: 1,
//      firstname: 'Bugs',
//      lastname: 'Bunny',
//      email: 'b-bunny@email.com'
//    }
//  ]
//}
```

#### Deleting Documents:


```js
const streamdb = require('streamdb')
const db = new streamdb.DB('myDb')

db.collection('users').deleteMany([2,3])
  .then(res => console.log(res))
  .catch(e => console.log(e))

// Response object: 
//{
//  success: true,
//  message: '2 documents removed from "users" collection'
//  data: [2,3]
//}
```

#### Using Schema Validation:


```js
// User Model
const streamdb = require('streamdb')
const Schema = streamdb.Schema

const User = new Schema({
    id: streamDb.Types.$incr,
    firstname: String,
  	lastname: String,
    email: {
    	type: String,
			required: true,
      maxlength: 100
    }
}, 
    {
        strict: false,
        timestamps: {
            created_at: false,
            updated_at: false
    }
})

module.exports = streamdb.model('User', User)
```

#### Launching/Using Server:


```js
const streamdb = require('streamdb')

const api = streamdb.server('myDb', 'api', 3000)

// open browser (or send GET query)..
// get all --> get(): http://localhost:3000/api/users
// get by id --> getById(1): http://localhost:3000/api/users/1

// sending POST request with JSON data in body..
// add many --> insertMany(docs): http://localhost:3000/api/users

// in POST body:
// [{
//  "firstname": "john",
//  "lastname": "smith",
//  "email": "jsmith@email.com"
//	},
//	{
//  "firstname": "mary",
//  "lastname": "jane",
//  "email": "mj@email.com"
//	}]
```

