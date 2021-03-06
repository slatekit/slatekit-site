---
layout: start_page_mods_utils
title: module Orm-Setup
permalink: /kotlin-mod-orm-setup
---

# Orm-Setup

{: .table .table-striped .table-bordered}
|:--|:--|
| **desc** | A registration system for entities and their corresponding repository/service impelementations | 
| **date**| 2018-03-19 |
| **version** | 0.9.9  |
| **jar** | slate.entities.jar  |
| **namespace** | slatekit.common.entities  |
| **source core** | slatekit.common.entities.Entities.kt  |
| **source folder** | [src/lib/kotlin/slatekit/](https://github.com/code-helix/slatekit/tree/master/src/lib/kotlin/slatekit/){:.url-ch}  |
| **example** | [/src/apps/kotlin/slate-examples/src/main/kotlin/slatekit/examples/Example_Entities_Reg.kt](https://github.com/code-helix/slatekit/tree/master/src/lib/kotlin/slatekit-examples/src/main/kotlin/slatekit/examples/Example_Entities_Reg.kt){:.url-ch} |
| **depends on** |  slatekit.common.jar  |

## Import
```kotlin 
// required 
import slatekit.entities.core.Entities
import slatekit.entities.core.EntityInfo


// optional 
import slatekit.common.Result
import slatekit.common.conf.ConfFuncs
import slatekit.common.db.DbLookup
import slatekit.common.db.DbTypeMySql
import slatekit.common.results.ResultFuncs.ok
import slatekit.core.cmds.Cmd
import slatekit.examples.common.User
import slatekit.examples.common.UserRepository
import slatekit.examples.common.UserService


```

## Setup
```kotlin


        // The entities are dependent on the database connections setup.
        // See Example_Database.scala for more info

        // 1. Register the default connection
        val dbs = DbLookup.defaultDb(ConfFuncs.readDbCon("user://.slate/db_default.txt")!!)

        // 2. Register a named connection
        //val dbs = DbLookup.namedDbs(("user_db", DbUtils.loadFromUserFolder(".slate", "db_default.txt"))

        // 3: Register connection as a shard and link to a group
        //val dbs = DbLookup.groupedDbs(("group1", List[(String,DbConString)](("shard1", DbUtils.loadFromUserFolder(".slate", "db_default.txt")))))
        

```

## Usage
```kotlin


        // The entities can be registered and set up in multiple ways.
        // They can be registered to :
        //
        // - use in-memory repository or a sql ( mysql ) repository
        // - use the default EntityService[T] or a custom EntityService
        // - use a singleton instance or new instance
        // - use a certain type of database ( mysql only for now )
        // - use the default EntityRepository ( mysql ) or a custom repository
        // - use a supplied EntityMapper or a custom mapper
        val entities = Entities(dbs)

        // Case 1: In-memory
        showResults("Case 1", entities.register<User>(isSqlRepo = false, entityType = User::class))

        // Case 2: In-memory + with custom service
        showResults("Case 2", entities.register<User>(isSqlRepo = false, entityType = User::class,
                serviceType = UserService::class))

        // Case 3: Sql-repo = EntityRepository[T] - mysql, default service ( EntityService[T] )
        // Note: this uses the default database connection above
        showResults("Case 3", entities.register<User>(isSqlRepo = true, entityType = User::class,
                dbType = DbTypeMySql))

        // Case 4: Sql-repo + with custom service = default sql repo ( EntityRepository[T] - mysql )
        // Note: this uses the default database connection above
        showResults("Case 4", entities.register<User>(isSqlRepo = true, entityType = User::class,
                serviceType = UserService::class, dbType = DbTypeMySql))

        // Case 5: Custom repository
        // Note: this uses the default database connection above
        showResults("Case 5", entities.register<User>(isSqlRepo = true, entityType = User::class,
                repository = UserRepository(User::class), dbType = DbTypeMySql))

        // Case 6: Custom service type, custom repository, database type
        showResults("Case 6", entities.register<User>(isSqlRepo = true, entityType = User::class,
                serviceType = UserService::class, repository = UserRepository (User::class), dbType = DbTypeMySql))

        // Case 7: Custom service type, custom repository, database specified
        // Note: this uses the named database connection above called "user_db"
        showResults("Case 7", entities.register<User>(isSqlRepo = true, entityType = User::class,
                serviceType = UserService::class, repository = UserRepository (User::class),
                dbType = DbTypeMySql, dbKey = "user_db"))

        // Case 8: Custom service type, custom repository, database specified, mapper specified
        // Each registration will simply overwrite an existing registration for the same entity type
        showResults("Case 8", entities.register<User>(isSqlRepo = true, entityType = User::class,
                serviceType = UserService::class, repository = UserRepository (User::class),
                dbType = DbTypeMySql))

        // Case 9: Provide a database db key ( e.g. for multiple database connections )
        showResults("Case 9", entities.register<User>(isSqlRepo = true, entityType = User::class,
                serviceType = UserService::class, repository = UserRepository (User::class),
                dbType = DbTypeMySql, dbKey = "user_db"))

        // Case 9: Provide a database db key ( e.g. for multiple database connections )
        showResults("Case 10", entities.register<User>(isSqlRepo = true, entityType = User::class,
                serviceType = UserService::class, repository = UserRepository (User::class),
                dbType = DbTypeMySql, dbKey = "group1", dbShard = "shard1"))
        //</doc:setup>

        //<doc:examples>
        // Use case 1: Get repository
        val repo = entities.getRepo<User>(User::class)

        // Use case 2: Get the service
        val svc = entities.getSvc<User>(User::class)

        // Use case 3: Get the entity mapper
        val mapper = entities.getMapper(User::class)

        // Use case 4: Get the repo for a specific shard
        val repoShard = entities.getRepo<User>(User::class)
        

```

