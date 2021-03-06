---
layout: start_page_mods_utils
title: module Request
permalink: /kotlin-mod-request
---

# Request

{: .table .table-striped .table-bordered}
|:--|:--|
| **desc** | Models and abstracts a request that can represent either an HTTP request or a request from the CLI ( Command line ). | 
| **date**| 2018-03-19 |
| **version** | 0.9.9  |
| **jar** | slatekit.common.jar  |
| **namespace** | slatekit.common  |
| **source core** | slatekit.common.Request.kt  |
| **source folder** | [src/lib/kotlin/slatekit/](https://github.com/code-helix/slatekit/tree/master/src/lib/kotlin/slatekit/){:.url-ch}  |
| **example** | [/src/apps/kotlin/slate-examples/src/main/kotlin/slatekit/examples/Example_Request.kt](https://github.com/code-helix/slatekit/tree/master/src/lib/kotlin/slatekit-examples/src/main/kotlin/slatekit/examples/Example_Request.kt){:.url-ch} |
| **depends on** |   |

## Import
```kotlin 
// required 
import slatekit.common.Request


// optional 
import slatekit.core.cmds.Cmd
import slatekit.common.Result
import slatekit.common.results.ResultFuncs.ok
import slatekit.common.InputArgs
import slatekit.common.Random


```

## Setup
```kotlin

n/a

```

## Usage
```kotlin


        // The Request class models and abstracts both an
        // HTTP Request and a CLI ( command line interface ) request.
        // This Request class along with its complementary Result class
        // provide the basis for much of the API support in Slate Kit.
        // APIs in Slate are protocol independent which basically means
        // that the APIs can be accessed as HTTP Endpoints or on the CLI
        //
        // ROUTING:
        // Requests in Slate Kit follow a 3 part route scheme
        // e.g. {area}.{api}.{action}
        // where :
        // 1. area   = top level category containing 1 or more apis ( e.g. "app"      )
        // 2. api    = name of a specific api                       ( e.g. "users"    )
        // 3. action = name of the endpoint or api action           ( e.g. "activate" )
        //
        // The api & action are just simple annotated classes and methods.
        //
        // NOTES:
        // This routing scheme is specifically designed to provide the following benefits:
        //
        // 1. Support a simplified routing scheme
        // 2. Support a unified routing scheme between HTTP and CLI
        // 3. Support easy discoverability by simply typing a "?" after each route part
        //    e.g. area? | area.api? | area.api.action?
        //
        // For more information on APIs and CLI refer to the Slate API and CLI documentation.

        // Here is a simple example of representing a route/request.
        // NOTE: This example below is just to show the concept, in the Slate Kit CLI and Server,
        // the request is appropriately built from either a CLI Command or the
        // SparkJava request ( with a thin abstraction layer ). The requests are
        // kept very simple/light for maximum performance and data is NEVER copied.
        val request = Request(
                path     = "app.users.activate",
                parts    = listOf("app", "users", "activate"),
                source   = "cli",
                verb     = "post",
                meta     = InputArgs(mapOf("api-key" to "ABC-123")),
                data     = InputArgs(mapOf("userId" to 5001)),
                raw      = "the raw HTTP SparkJava request or CLI ShellCommand",
                tag      = Random.stringGuid()
        )

        // NOTES: ResultSupport trait builds results that simulate Http Status codes
        // This allows easy construction of results/status from a server layer
        // instead of a controller/api layer

        // CASE 1: Get the path of the request
        println( request.path )

        // CASE 2: Get the parts of the path
        println( request.parts )

        // CASE 3: Get the area/api/action
        println( request.area   )
        println( request.name   )
        println( request.action )

        // CASE 4a: Get a header named "api-key"
        println( request.meta?.getString("api-key") )

        // CASE 4b: Get a header named "sample-id" as integer
        // IF its not there, this will return a 0 as getInt
        // returns a non-nullable value
        println( request.meta?.getInt("sample-id") )

        // CASE 4c: Get a header named "sample-id" as nullable integer
        println( request.meta?.getIntOpt("sample-id") )

        // CASE 4d: Get a header named "sample-id" as integer with default
        // value if its not there
        println( request.meta?.getIntOrElse("sample-id", -1) )

        // CASE 5a: Get a parameter named "userId" as integer
        // IF its not there, this will return a 0 as getInt
        // returns a non-nullable value
        // The value is obtained from query string if get,
        // otherwise, if the request is a post, the value is
        // first checked in the body ( json data ) before checking
        // the query params
        println( request.meta?.getInt("userId") )

        // CASE 5b: Get a parameter named "userId" as nullable integer
        println( request.meta?.getIntOpt("userId") )

        // CASE 5c: Get a parameter named "userId" as integer with default
        // value if its not there
        println( request.meta?.getIntOrElse("userId", -1) )

        // CASE 6: Get the verb ( only applicable to HTTP requests )
        // For the CLI - the verb will be "cli"
        println( request.verb )

        // CASE 7: Get the tag
        // The request is immutable and the tag field is populated with
        // a random guid, so if an error occurs this tag links the request to the error.
        println( request.tag )
        

```

