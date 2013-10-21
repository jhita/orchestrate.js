# orchestrate.js

Node Driver for [Orchestrate.io](http://orchestrate.io).


# Installation

```
$ npm install orchestrate
```

# Running Tests
Currently, Orchestrate.js runs against the actual Orchestrate API. At the moment, there is no available local version to work with.

Ensure all dependencies are installed within the orchestrate director by running

```
$ npm install
```
To run tests:

```
$ npm test
```

# Creating a Client

```javascript
var db = require('orchestrate')(token)
```

# Running Queries

Orchestrate comes with support for GET/PUT/DEL for key-value queries, as well as search, graph, and events. Documentation can be found [here](https://docs.orchestrate.io/).

All queries are promise based. Just as a typical function would return a callback containing an error field followed by a result, orchestrate.js returns `then` and `fail` methods.

## Key-Value

To get a value:

```javascript
db.get('collection', 'key')
.then(function (result) {

})
.fail(function (err) {

})
```

To set a value:

```javascript
db.put('collection', 'key', {
  "name": "Steve Kaliski",
  "hometown": "New York, NY",
  "twitter": "@stevekaliski"
})
.then(function (result) {

})
.fail(function (err) {

})
```

## Search

```javascript
db.search('collection', 'query')
.then(function (result) {

})
.fail(function (err) {

})
```

## Graphs
An awesome feature Orchestrate includes is the ability to generate graphs between collections. For example, consider the collections `users` and `movies`. Some user Steve will `like` a variety of movies. We can generate this relationship:

```javascript
db.newGraphBuilder()
  .from('users', 'Steve')
  .related('likes')
  .to('movies', 'Superbad')
```

We can then look up all the different items Steve likes:
```javascript
db.newGraphReader()
  .from('users', 'Steve')
  .related('likes')
```

We can even take this another step further:
```javascript
db.newGraphReader()
  .from('users', 'Steve')
  .related('friends', 'likes')
```
This will return all of the things that friends of Steve have liked. This assumes a friend relation has previously been defined between Steve and another user.

## Events
Events are time-ordered objects that exist with the context of a Key-Value object. Consider comments on a post or messages in a thread.
