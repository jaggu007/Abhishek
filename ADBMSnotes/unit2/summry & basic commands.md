
### MongoDB Local Installation Checklist

| #  | Component                             | Required?      | Purpose                                                                   |
| -- | ------------------------------------- | -------------- | ------------------------------------------------------------------------- |
| 1  | **MongoDB Community Server**    | ✅ Yes         | The actual MongoDB database server                                        |
| 2  | **MongoDB Shell (`mongosh`)** | ✅ Yes         | Command-line interface to interact with MongoDB                           |
| 3  | **MongoDB Compass**             | ✅ Recommended | GUI for creating databases, collections, queries, and viewing documents   |
| 4  | **MongoDB Database Tools**      | ⭐ Recommended | Utilities such as `mongodump`,`mongorestore`,`mongoexport`, etc.    |
| 5  | **Command Line / Terminal**     | ✅ Yes         | Start and manage MongoDB services and run `mongosh`                     |
| 6  | **PATH configuration**          | ✅ Yes         | Allows commands such as `mongosh`and `mongod`to run from any terminal |
| 7  | **Database storage directory**  | ✅ Yes         | Location where MongoDB stores local database files                        |
| 8  | **MongoDB service**             | ✅ Yes         | Background process that runs the MongoDB server                           |
| 9  | **MongoDB connection string**   | ✅ Yes         | Used by applications/tools to connect to the local server                 |
| 10 | **Programming driver**          | Optional       | Required if connecting MongoDB to Java, Python, Node.js, PHP, etc.        |

### The minimum setup

For a basic MongoDB practical/lab, you really need:

**MongoDB Community Server**
↓
**MongoDB Shell (`mongosh`)**
↓
**MongoDB Compass**
↓
**Terminal/Command Prompt**
↓
**MongoDB service running**

Then you should be able to test:

I would install these  **four MongoDB components** :

1. **MongoDB Community Server** — database engine
2. **MongoDB Shell (`mongosh`)** — command-line operations
3. **MongoDB Compass** — graphical interface
4. **MongoDB Database Tools** — backup/import/export tools

=============================
