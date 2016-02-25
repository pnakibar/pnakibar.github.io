---
layout: post
title: Adventures in the Land of Knex.js - Migrations and Knexfile!
---

> For this tutorial you will need the sqlite3 connector so just: 'npm install sqlite3 --save' and you are good to go!

  Oooohhh the migrations, and how they saved the humanity and continues to everyday save millions of developers around the world.

  *Schema Migrations* is a term that reffers to the set of tools that lets the developer simply change the schema of his application easily, without touching the database and SQL things. *Schema Versioning* is also a thing that comes in mind, and it's extremely useful: it let's you just write downgrades and upgrades to your data base schema. In the sense of changing the possible rows, creating tables and such.

  Luckily Knex.js has a Migration Tool! Luckily you installed it in the previous tutorial, so lets just call it:

```bash
$ knex

  Usage: knex [options] [command]


  Commands:

    init [options]                         Create a fresh knexfile.
    migrate:make [options] <name>         Create a named migration file.
    migrate:latest                         Run all migrations that have not yet been run.
    migrate:rollback                       Rollback the last set of migrations performed.
    migrate:currentVersion                View the current version for the migration.
    seed:make [options] <name>            Create a named seed file.
    seed:run                              Run seed files.

  Options:

    -h, --help         output usage information
    -V, --version      output the version number
    --debug            Run with debugging.
    --knexfile [path]  Specify the knexfile path.
    --cwd [path]       Specify the working directory.
    --env [name]       environment, default: process.env.NODE_ENV || development

```

  From the beggining of your project **YOU'LL** want a *knexfile.js*. Why? It simply stores all of your databases and provides an awesome division to let you simply choose which database to use, from _development_, _staging_ and _production_, by simpling setting your NODE\_ENV.

```bash
$ knex init
Created ./knexfile.js
```

And the contents are:


```javascript
module.exports = {

  development: {
    client: 'sqlite3',
    connection: {
      filename: './dev.sqlite3'
    }
  },

  staging: {
    client: 'postgresql',
    connection: {
      database: 'my_db',
      user:     'username',
      password: 'password'
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: 'knex_migrations'
    }
  },

  production: {
    client: 'postgresql',
    connection: {
      database: 'my_db',
      user:     'username',
      password: 'password'
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: 'knex_migrations'
    }
  }

};
```

_Go ahead and just define them, look at the (wiki)[http://knexjs.org/#knexfile] if you are in trouble!_

## Now, the migrations!
Lets create a new *migration*, a new version, a update to the current database:

```bash
$ knex migrate:make first-one
Using environment: development
Knex:warning - sqlite does not support inserting default values. Set the `useNullAsDefault` flag to hide this warning. (see docs http://knexjs.org/#Builder-insert).
Created Migration: /me/git/tutorials/knex-tutorial/migrations/20160224200600_first-one.js
```

And a new file has been created at the migrations folder:

```javascript

exports.up = function(knex, Promise) {
	// You populate this function with upgrades, such as 
  //creating tables, messing with the schema, inserting data and such.
};

exports.down = function(knex, Promise) {
  // In this function you simply clean the 
  //things you have done in the *up* function.
};

```

  To understand the file youll need to know three things:

* knex migrate:latest

  This command will update your database to the last iteration of your migration.
  In other words it will just execute the up command of the migration files in order of creation, oldest first. 

* knex migrate:rollback

  This command will decrease the migration version, a simple abstraction to the _down_ function. 

* How do they work?

  When you first the execute the *knex migrate:latest*, Knex will get the actual migrations version and execute the newer files in the migrations folder. Simple as that.

### Workflow
1. _knex init_
2. _knex migrate:make <name of the migration file>_
3. modify the _migration/migration-name-1.js_
4. _knex migrate:latest_
5. continue to code
6. "oops this table needs some more columns"
7. _knex migrate:make <name of the new migration file>_
8. modify the new migration file again
9. _knex migrate:latest_
10. repeat

It's important to put the _down_ function, because if something goes wrong you can simply _knex migrate:rollback_ and try again.

### Getting Real
# TODO: real life examples
