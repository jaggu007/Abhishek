1. **MongoDB Community Server** — runs MongoDB locally on your Windows machine.
2. **MongoDB Atlas** — cloud-hosted MongoDB, useful when you want students/projects to connect over the internet.
3. **MongoDB Compass** — graphical interface for both local MongoDB and Atlas.

### Recommended setup flow

**Part A — Local MongoDB**

* Install MongoDB Community Server
* Install MongoDB Compass
* Verify the MongoDB service
* Connect using `mongosh`
* Create a database and collection
* Perform basic CRUD operations

**Part B — MongoDB Atlas**

* Create an Atlas account
* Create a free cluster
* Create a database user
* Configure network access
* Connect using Compass
* Connect using `mongosh`
* Get the connection string for applications

**Part C — Student/project setup**

* Create separate databases/collections
* Connect Java/Python/Node.js applications
* Understand the difference between **local MongoDB** and **Atlas**

==============================================

MongoDB Community Server — Windows Setup

## Step 1: Check your Windows version

MongoDB Community 8.0 supports **64-bit Windows 11 and Windows Server 2022** on x86_64. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-windows-unattended/?utm_source=chatgpt.com "Install MongoDB Community on Windows using msiexec.exe - Database Manual v8.0 - MongoDB Docs"))

Press:

**Windows + R**

Type:

```text
winver
```

Press  **Enter** .

You should preferably be running Windows 11 64-bit.

---

## Step 2: Download MongoDB Community Server

Go to the official MongoDB download page:

[MongoDB Community Server Download](https://www.mongodb.com/try/download/community?utm_source=chatgpt.com)

Select:

| Option       | Select                           |
| ------------ | -------------------------------- |
| Version      | Current stable Community version |
| Platform     | Windows                          |
| Package      | MSI                              |
| Architecture | x86_64                           |

Click  **Download** .

The official installation method uses the `.msi` Windows installer. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-windows/?utm_source=chatgpt.com "Install MongoDB Community Edition on Windows - Database Manual v8.0 - MongoDB Docs"))

---

# Step 3: Run the installer

Go to your **Downloads** folder.

You should see something similar to:

```text
mongodb-windows-x86_64-8.0.x-signed.msi
```

Double-click it.

If Windows asks:

> Do you want to allow this app to make changes?

Click:

**Yes**

---

# Step 4: Start the installation wizard

You will see the MongoDB setup wizard.

Click:

**Next**

Accept the license agreement.

Click:

**Next**

---

# Step 5: Choose Setup Type

You will normally see:

* Complete
* Custom

Choose:

### Complete

Then click:

**Next**

---

# Step 6: Configure MongoDB as a Windows Service

This is the  **important step** .

You should see an option similar to:

> Install MongoD as a Service

Make sure it is  **checked** .

Use:

**Service Name:**

```text
MongoDB
```

For a normal classroom/development installation, keep the default service configuration.

The installer will create the MongoDB configuration file and configure the data and log directories. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-windows/?utm_source=chatgpt.com "Install MongoDB Community Edition on Windows - Database Manual v8.0 - MongoDB Docs"))

---

# Step 7: Install MongoDB Compass

You may see:

> Install MongoDB Compass

Keep this  **checked** .

MongoDB Compass is a graphical interface that will make it much easier to demonstrate MongoDB to students.

So your setup will be:

```text
MongoDB Server
       +
MongoDB Compass
       +
MongoDB Shell (mongosh)
```

**Important:** the MongoDB Server MSI does **not** include `mongosh`; MongoDB provides `mongosh` as a separate installation. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-windows/?utm_source=chatgpt.com "Install MongoDB Community Edition on Windows - Database Manual v8.0 - MongoDB Docs"))

Click:

**Next → Install**

Wait for the installation to finish.

Click:

**Finish**

---

# Step 8: Check whether MongoDB Service is running

Press:

```text
Windows + R
```

Type:

```text
services.msc
```

Press  **Enter** .

Look for:

```text
MongoDB Server
```

or

```text
MongoDB
```

You should see:

```text
Status: Running
Startup Type: Automatic
```

Because MongoDB was installed as a Windows service, it can start automatically with Windows. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-windows/?utm_source=chatgpt.com "Install MongoDB Community Edition on Windows - Database Manual v8.0 - MongoDB Docs"))

---

# Step 9: Verify from Command Prompt

Open  **Command Prompt** .

Run:

```cmd
sc query MongoDB
```

If the service is running, you should see something like:

```text
STATE              : 4  RUNNING
```

That's a good sign. 🎉

---

# Step 10: Start MongoDB manually if required

If MongoDB is not running, open  **Command Prompt as Administrator** .

Run:

```cmd
net start MongoDB
```

You should get:

```text
The MongoDB service was started successfully.
```

MongoDB's documentation also recommends `net start MongoDB` for starting the service manually. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-windows-unattended/?utm_source=chatgpt.com "Install MongoDB Community on Windows using msiexec.exe - Database Manual v8.0 - MongoDB Docs"))

---

# Step 11: Install MongoDB Shell (`mongosh`)

Now we need the command-line shell.

Go to:

[MongoDB Shell Download](https://www.mongodb.com/try/download/shell?utm_source=chatgpt.com)

Select:

```text
Platform: Windows x64
Package: MSI
```

Download and install it.

During installation, make sure the installer adds `mongosh` to your **PATH** if that option is offered. MongoDB recommends having `mongosh` available through PATH. ([MongoDB](https://www.mongodb.com/docs/mongodb-shell/install/upgrade/?utm_source=chatgpt.com "Upgrade mongosh - mongosh - MongoDB Docs"))

---

# Step 12: Verify mongosh

Close your existing Command Prompt.

Open a **new** Command Prompt.

Run:

```cmd
mongosh --version
```

You should get something similar to:

```text
2.x.x
```

If you see the version number, `mongosh` is installed correctly.

---

# Step 13: Connect to your local MongoDB

Now simply type:

```cmd
mongosh
```

You should see something similar to:

```text
Current Mongosh Log ID: ...
Connecting to: mongodb://127.0.0.1:27017/
Using MongoDB: 8.x.x
Using Mongosh: 2.x.x
```

You should eventually get:

```text
test>
```

🎉 **Your MongoDB server is running!**

---

# Step 14: Test MongoDB

Inside `mongosh`, type:

```javascript
show dbs
```

You may see:

```text
admin
config
local
```

Now create a database:

```javascript
use college
```

You should get:

```text
switched to db college
```

Create a collection and insert a document:

```javascript
db.students.insertOne({
    name: "Rahul",
    course: "B.Tech CSE",
    semester: 5
})
```

You should get something like:

```text
{
  acknowledged: true,
  insertedId: ObjectId('...')
}
```

Now retrieve it:

```javascript
db.students.find()
```

You should see:

```text
{
  _id: ObjectId('...'),
  name: 'Rahul',
  course: 'B.Tech CSE',
  semester: 5
}
```

Congratulations — you've just created your first MongoDB database and document. 🚀

---

# Step 15: Connect using MongoDB Compass

Open:

**MongoDB Compass**

In the connection field, enter:

```text
mongodb://localhost:27017
```

or:

```text
mongodb://127.0.0.1:27017
```

Click:

**Connect**

You should now see:

```text
Databases
 ├── admin
 ├── config
 ├── local
 └── college
      └── students
```

The default MongoDB configuration binds the server to `127.0.0.1`, meaning it accepts local connections rather than remote connections. That's exactly what you want for your initial local setup. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-windows/?utm_source=chatgpt.com "Install MongoDB Community Edition on Windows - Database Manual v8.0 - MongoDB Docs"))

---


=======================================================================================================

Open **PowerShell as Administrator** and follow these steps.

### 1. Check WinGet

```powershell
winget --version
```

If it returns a version such as:

```text
v1.x.x
```

you're ready.

### 2. Find MongoDB Community Server

```powershell
winget search MongoDB
```

You should see MongoDB Community Server in the results.

### 3. Install MongoDB Community Server

Run:

```powershell
winget install MongoDB.Server
```

If WinGet asks you to accept source agreements, type:

```text
Y
```

The installer should configure MongoDB as a Windows service.

### 4. Check the MongoDB service

```powershell
Get-Service MongoDB
```

You want to see:

```text
Status   Name      DisplayName
------   ----      -----------
Running  MongoDB   MongoDB Server
```

If it says `Stopped`, start it:

```powershell
Start-Service MongoDB
```

### 5. Set MongoDB to start automatically

```powershell
Set-Service MongoDB -StartupType Automatic
```

Now MongoDB will start automatically whenever Windows starts.

### 6. Check whether MongoDB is listening on port 27017

```powershell
netstat -ano | findstr :27017
```

You should see something similar to:

```text
TCP    127.0.0.1:27017    0.0.0.0:0    LISTENING
```

That means the MongoDB server is running. ✅

### 7. Install MongoDB Shell

Search for `mongosh`:

```powershell
winget search mongosh
```

Then install it:

```powershell
winget install MongoDB.mongosh
```

Close PowerShell and open a **new** PowerShell window.

Check:

```powershell
mongosh --version
```

### 8. Connect to MongoDB

Now simply run:

```powershell
mongosh
```

You should get:

```text
Connecting to: mongodb://127.0.0.1:27017/
```

and eventually:

```text
test>
```

🎉 You're connected.

### 9. Test the database

Inside `mongosh`:

```javascript
show dbs
```

Create a database:

```javascript
use college
```

Create a collection and insert data:

```javascript
db.students.insertOne({
    name: "Rahul",
    branch: "CSE",
    semester: 5
})
```

Check the data:

```javascript
db.students.find()
```

You should see the student document.

### 10. Useful service commands

**Start MongoDB:**

```powershell
Start-Service MongoDB
```

**Stop MongoDB:**

```powershell
Stop-Service MongoDB
```

**Restart MongoDB:**

```powershell
Restart-Service MongoDB
```

**Check status:**

```powershell
Get-Service MongoDB
```

### One-shot installation summary

For a fresh Windows machine, the basic command sequence is:

```powershell
winget search MongoDB
winget install MongoDB.Server
Get-Service MongoDB
Start-Service MongoDB
Set-Service MongoDB -StartupType Automatic
winget search mongosh
winget install MongoDB.mongosh
```

Then open a  **new terminal** :

```powershell
mongosh
```

and test:

```javascript
use college

db.students.insertOne({
    name: "Rahul",
    branch: "CSE",
    semester: 5
})

db.students.find()
```

**Note:** `MongoDB.Server` and `MongoDB.mongosh` package IDs can vary with the WinGet catalog. If `winget install MongoDB.Server` says  *No package found* , paste the output of `winget search MongoDB` here and I’ll give you the exact command for your machine.
