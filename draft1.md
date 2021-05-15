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

 
 <p align="center"> 
  <img src="https://github.com/fabtshow/stuff/blob/5ff03253b16232db43db39ad6690634c5061af97/test2.gif" alt="quick start setup" style="max-height: 550px;">
</p>

 
#### DB Setup:

<p align="left"> 
  <img src="https://github.com/fabtshow/stuff/blob/a4bb0c0f4b25832b8744514c4ad0efa5259eaaec/img/setup2.svg" alt="setup" style="max-height: 230px;">
</p>


```js
const streamdb = require('streamdb')

streamdb.createDb({ dbName: 'myDb' })
  .then(res => console.log(res))
  .catch(e => console.log(e))
```

#### Adding Collections:

<p align="left"> 
  <img src="https://github.com/fabtshow/stuff/blob/8bc822c6f4dccae70f5cf40a3e3f151e1fdf442d/img/add-collections2.svg" alt="adding collections" style="max-height: 230px;">
</p>

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

___

### Starter Schema Model Template:

<p align="left"> 
  <img src="https://github.com/fabtshow/stuff/blob/83739c4d518c099c74127b3395a0fa451fd81f0d/img/model-template.svg" alt="model template" style="max-height: 440px;">
</p>

___

### Starter Collection Routes:

<table>
	<tr>
		<th>Request</th>
		<th>Route</th>
		<th>Description</th>
		<th>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp-&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</th>
		<th>Method</th>
	</tr>
	  <tr>
		<td align="center">GET</td>
		<td>
		   <code>/api/collection
		   </code>
		</td>
		<td colspan="2">Get all docs</td>
		<td align="center">
		  <code>
			  get()
		  </code>
		</td>
	  </tr>
	<tr>
		<td align="center">GET</td>
		<td>
		  <code>
		  	/api/collection/:id
		  </code>
		</td>
		<td colspan="2">Get by id</td>
		<td align="center">
		  <code>
			  getById()
		  </code>
		</td>
  	</tr>
	<tr>
		<td align="center">GET</td>
		<td>
		  <code>
		  	/api/collection/_q/
		  </code>
		</td>
		<td colspan="2">Run compound queries</td>
		<td align="center">
		  <code>
			helper_methods
		  </code>
		</td>
  	</tr>
	<tr>
		<td align="center">POST</td>
		<td>
		  <code>
		  	/api/collection
		  </code>
		</td>
		<td colspan="2">Insert many docs</td>
		<td align="center">
		  <code>
			  insertMany()
		  </code>
		</td>
  	</tr>
	<tr>
		<td align="center">PUT</td>
		<td>
		  <code>
		  	/api/collection
		  </code>
		</td>
		<td colspan="2">Update many docs</td>
		<td align="center">
		  <code>
			  updateMany()
		  </code>
		</td>
  	</tr>
	<tr>
		<td align="center">PUT</td>
		<td>
		  <code>
		  	/api/collection/_q/
		  </code>
		</td>
		<td colspan="2">Run update queries</td>
		<td align="center">
		  <code>
			helper_methods
		  </code>
		</td>
  	</tr>
	<tr>
		<td align="center">DELETE</td>
		<td>
		  <code>
		  	/api/collection/:id
		  </code>
		</td>
		<td colspan="2">Delete by id</td>
		<td align="center">
		  <code>
			  deleteOne()
		  </code>
		</td>
  	</tr>
</table>

<br>

**Sample Route From Router Template:**
<p align="left"> 
  <img src="https://github.com/fabtshow/stuff/blob/113f74a3790186c27e866a44919f4fadfa37ba5c/img/get-route.svg" alt="route template" style="max-height: 420px;">
</p>

