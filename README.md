# Skull - Swift SQLite

**Skull** is a minimal [SQLite](https://www.sqlite.org/) interface for Swift. Its current build is configured as an iOS framework. To keep it simple, **Skull** is not thread-safe and leaves access serialization to the user. A `Skull` instance caches its compiled SQL statements (prepared statements).

[![Build Status](https://secure.travis-ci.org/michaelnisi/skull.svg)](http://travis-ci.org/michaelnisi/skull)

## types

```swift
SkullError
```
The possible errors.

- `AlreadyOpen(String)`
- `FailedToFinalize(Array<ErrorType>)`
- `InvalidURL`
- `NULLFromCString`
- `NoPath`
- `NotOpen`
- `SQLiteError(Int, String)`
- `UnsupportedType`

```swift
SkullRow()
```
A database `row` with subscript access to column values. Supported types are `String`, `Int`, and `Double`. For example:

```swift
row["a"] as String == "500.0"
row["b"] as Int == 500
row["c"] as Double == 500.0
```

```swift
Skull()
```
Initializes a Skull instance. For example:

```swift
let db = Skull()
```

## exports

### Opening a database connection

```swift
Void db.open() throws
```
Opens an in-memory database.

```swift
Void open (url: NSURL) throws
```
- url The location of the database file to open

Opens the database located at the provided `NSURL`. If the file does not exist, it is created. Passing `nil` means `db.open()`.

### Accessing the database

```swift
Void db.exec(sql: String, cb: ((SkullError?, [String:String]) -> Int)?)) throws
```
- `sql` The SQL statement to execute.
- `cb` An optional callback to handle results.

Executes SQL statement and applies the callback for each result. The callback is optional. If a callback is provided, it can abort execution by returning something else than `0`.

```swift
Void db.query(sql: String, cb: (SkullError?, SkullRow?) -> Int) throws
```
- `sql` The SQL statement to apply.
- `cb` The callback to handle results.

Queries the database with the SQL statement and apply the callback for each resulting row.

```swift
Void db.update(sql: String, params: Any?...) throws
```
- `sql` The SQL statement to apply.
- `params` The parameters to bind to the statement.

Updates the database by binding the parameters to an SQL statement, for example:
```swift
let sql = "INSERT INTO t1 VALUES (?,?,?,?,?)"
let error = db!.update(sql, 500.0, 500.0, 500.0, 500.0, "500.0")
```

### Managing the database connection

```swift
Void db.flush() throws
```
Removes and finalizes all cached prepared statements.

```swift
Void db.close() throws
```
Flushes cache and closes database connection.

## Install

To configure the [module map file](http://clang.llvm.org/docs/Modules.html#module-map-file) `module/module.modulemap` do:

```bash
$ ./configure
```
And add `Skull.xcodeproj` to your workspace to link with `Skull.framework` in your targets.

## License

MIT
