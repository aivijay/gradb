# gradb

An in-memory graph database written in Clojure. Supports entities with attributes, graph traversal (BFS), multi-version concurrency via a layer-based transaction system, and datalog-style indexing (EAVT, AVET, VAET).

## Status

Experimental. Originally written circa 2018.

## Project Structure

```
src/gradb/
├── core.clj       # Core DB ops: add/update/remove entity, transact
├── graph.clj      # Graph traversal: BFS, incoming/outgoing refs
├── storage.clj    # Storage layer
├── query.clj      # Query engine
├── constructs.clj # Data constructs and utilities
└── manage.clj     # Management operations
```

## Running

Requires [Leiningen](https://leiningen.org):

```bash
lein repl
```

## Usage

```clojure
(require '[gradb.core :as db])

;; Create a database
(def db {})

;; Add entities
(def db (db/add-entity db {:db/id :db/no-id-yet
                           :person/name "Alice"
                           :person/age 30}))

;; Update an entity
(def db (db/update-entity db :1 :person/age 31))

;; Query (see query.clj for full query API)
```

## API Overview

### Core (gradb.core)

| Function | Description |
|----------|-------------|
| `add-entity` | Add an entity to the db |
| `add-entities` | Add multiple entities |
| `update-entity` | Update an entity attribute |
| `remove-entity` | Remove an entity |
| `transact` | Execute a transaction |
| `what-if` | Simulate a transaction without committing |
| `evolution-of` | History of an entity's attribute values |

### Graph (gradb.graph)

| Function | Description |
|----------|-------------|
| `traverse-db` | Traverse graph via BFS |
| `outgoing-refs` | Get outgoing references from an entity |
| `incoming-refs` | Get entities that reference a given entity |

## Architecture

- **Storage**: In-memory Clojure map
- **Indices**: EAVT, AVET, VAET for efficient lookups
- **Transactions**: Layer-based MVCC — each transaction creates a new layer
- **History**: Full attribute history with timestamps and previous-timestamps

## Dependencies

- Clojure 1.8.0

## License

Eclipse Public License
