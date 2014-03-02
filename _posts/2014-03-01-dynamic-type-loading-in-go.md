---
layout: post
title: How I do Dynamic Type Loading in Go
---

In Java and Python it's common to inspect your execution environment and instantiate a different 
Class or Module.  In Java you can use [`Class.forName(String className)`](http://docs.oracle.com/javase/7/docs/api/java/lang/Class.html#forName(java.lang.String\)) and if `className` does not exist
the function will throw a `ClassNotFoundException`.  It's up to the developer to deal with the exception.  Python has the magical [`__import__` builtin](http://docs.python.org/2/library/functions.html#__import__) which does a very similar thing.

Ok, so I've come to love this technique in Java and Python but how do I do something like this in Go.  Go has a [reflect](http://golang.org/pkg/reflect/) package that allows you to inspect types at runtime.  That is cool and all but how do I load a specific type at runtime.  This isn't possible in the same manner as Java or Python.  At first I was devastated then I started to look into how the `database/sql` package worked with respect to using different drivers based on a connection string.  I found some neat code in the [database/sql](https://code.google.com/p/go/source/browse/src/pkg/database/sql/sql.go#25) package.  Take a look at the snip it below:

```go
var drivers = make(map[string]driver.Driver)

// Register makes a database driver available by the provided name.
// If Register is called twice with the same name or if driver is nil,
// it panics.
func Register(name string, driver driver.Driver) {
        if driver == nil {
                panic("sql: Register driver is nil")
        }
        if _, dup := drivers[name]; dup {
                panic("sql: Register called twice for driver " + name)
        }
        drivers[name] = driver
}
```

So, first an unexported map of type string and `driver.Driver` is created.  The `Register` function is used to register a specific name (key in the map) with a struct that implements driver.Driver and panics if the user tries to register the name more than once or driver is nil.

We've looked at how we can register our stucts with a name but how do we use them?  Let's take a look at the implementation of [func Open](https://code.google.com/p/go/source/browse/src/pkg/database/sql/sql.go#411) in `database/sql`:

```go
func Open(driverName, dataSourceName string) (*DB, error) {
        driveri, ok := drivers[driverName]
        if !ok {
                return nil, fmt.Errorf("sql: unknown driver %q (forgotten import?)", driverName)
        }
        db := &DB{
                driver:   driveri,
                dsn:      dataSourceName,
                openerCh: make(chan struct{}, connectionRequestQueueSize),
                lastPut:  make(map[*driverConn]string),
        }
        db.freeConn = list.New()
        db.connRequests = list.New()
        go db.connectionOpener()
        return db, nil
}
```

Open takes a driverName (our key to the drivers map) and a dataSourceName or DSN.  The driver is pulled out of the map and verifies it was registered.  The rest of the function is out of scope for this post but you see how the driver is then passed on to other structs that will use the database driver.  

Why is this cool?  Well, now a database driver libraries can Register their structs that implement the driver.Driver interface.  Hmm...  Wait, so when I use the MySql driver in Go I don't need to call the Register funcion in the `database/sql` package I just use the Open function in the `database/sql` and off I go...  What are you talking about.

So, when you have an import like the following

```go
import (
	_ "github.com/bradfitz/mysql"
)
```

That import will register the MySql driver with `database/sql` package via an `init()` function.  Take a [peek at](https://github.com/bradfitz/mysql/blob/master/driver.go#L88).  Pretty cool right.

So, to sum it all up...  Go doesn't really allow you to do dynamic type loading in the langauge but you don't really need the language to do this for you.  If you leverage interfaces and you should you can take advantage of the technique used by `database/sql`.  This should solve the problem for you 90% of the time.


