---
layout: post
title: Adventures in the Land of Knex.js - Part 1
---

The latest addition to my Javascript toolbet is *Knex.js*. The query builder that powers other libraries, such as [bookshelf.js](http://bookshelfjs.org/). As they say it's a _batteries included_ SQL query builder, amazingly fast and even more easy and *fun* to use, it's the definitive tool to learn. If you are not still convinced, it supports the traditional (aka callback-hell) and *promise* interfaces.

Enough with talking, this library supports *Postgres, MySQL, MariaDB, SQLite3, and Oracle*.

_This tutorial is going to cover only the postgres part, but knex can handle the others with the same interface, besides SQLite3, which requires setting the path, but it's extremely simple. I'm not going to cover all of the tool, just the basics (querying and inserting data, handling schemas) and the_ *Migrations*

# Getting to work!
Installing it is simple, inside your npm project just run these commands:

```bash
$ npm install knex --save #for the project
$ npm install knex -g #for the tools I'm going to explain later
$ npm install pg --save #because we need the driver!
```

Once the dependencies are installed, you can just mess around, with the queries:

```javascript
var pg = require('knex')({
  client: 'pg',
  connection: {
    host     : '127.0.0.1',
    user     : 'your_database_user',
    password : 'your_database_password',
    database : 'myapp_test'
  }
});
```

The above snippet is a simple connection to a _postgres_ database. If it evaluates accordingly, you are connected, and you can make queries using the variable _pg_.

Using the _pg variable_ we can do some nice things:

```javascript
pg('table_name').insert({id: 1, colName: 'asd', col2Name: 'asdasd'})
pg.select('*').from('table_name').then((x) => console.log(x))
Selecting and inserting data couldn't be easier!
```

With promises your life sure can be easier, no more callback hells! improving on the snipper above, for the selection (the same can also apply to the insertion), we can make the _promise_, oops, select does something (in this case, _console.error_) if any of the operations preceding it goes wrong:

```javascript
pg.select('*')
	.from('table_name1')
    .then((x) => console.log(x))
    .error(console.error) //like the try/catch but in a monadic way.
```
