Yes. If you want to install  **MongoDB Community Server on macOS using Zsh + Homebrew** , the cleanest approach is to use MongoDB's official Homebrew tap. The current MongoDB documentation provides the **8.0 Community Edition** formula. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-os-x/?utm_source=chatgpt.com "Install MongoDB Community Edition on macOS - Database Manual v8.0 - MongoDB Docs"))

## 1. Open Terminal

macOS uses **Zsh by default** on modern versions.

Check:

```zsh
echo $SHELL
```

You should see:

```text
/bin/zsh
```

---

## 2. Install Xcode Command Line Tools

MongoDB/Homebrew requires the Apple command-line tools.

```zsh
xcode-select --install
```

If they are already installed, macOS will tell you so. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-os-x/?utm_source=chatgpt.com "Install MongoDB Community Edition on macOS - Database Manual v8.0 - MongoDB Docs"))

---

## 3. Check whether Homebrew is installed

```zsh
brew --version
```

If you get something like:

```text
Homebrew 4.x.x
```

you are ready.

If `brew` is not found, install Homebrew using the official installer, then configure it for Zsh. Homebrew recommends using `brew shellenv` to put Homebrew into your shell environment. ([Homebrew Documentation](https://docs.brew.sh/Installation?utm_source=chatgpt.com "Homebrew Documentation: Installation"))

For an Apple Silicon Mac:

```zsh
eval "$(/opt/homebrew/bin/brew shellenv)"
```

To make this permanent:

```zsh
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

For an Intel Mac, Homebrew is normally under `/usr/local`:

```zsh
eval "$(/usr/local/bin/brew shellenv)"
```

---

# 4. Update Homebrew

```zsh
brew update
```

---

# 5. Add MongoDB's Homebrew repository

```zsh
brew tap mongodb/brew
```

MongoDB's documentation uses this official MongoDB Homebrew tap. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-os-x/?utm_source=chatgpt.com "Install MongoDB Community Edition on macOS - Database Manual v8.0 - MongoDB Docs"))

You can verify it:

```zsh
brew tap
```

You should see:

```text
mongodb/brew
```

---

# 6. Install MongoDB Community Server

For the current  **MongoDB 8.0 Community Edition** :

```zsh
brew install mongodb-community@8.0
```

This installation includes:

* `mongod` → MongoDB database server
* `mongosh` → MongoDB Shell
* `mongos` → sharded-cluster query router
* MongoDB Database Tools ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-os-x/?utm_source=chatgpt.com "Install MongoDB Community Edition on macOS - Database Manual v8.0 - MongoDB Docs"))

---

# 7. Check the installation

Run:

```zsh
mongod --version
```

Then:

```zsh
mongosh --version
```

You can also check where Homebrew installed MongoDB:

```zsh
brew --prefix mongodb-community@8.0
```

And:

```zsh
brew --prefix
```

---

# 8. Start MongoDB

This is the important step: **installing MongoDB does not mean the database server is currently running.**

Start it as a macOS service:

```zsh
brew services start mongodb-community@8.0
```

MongoDB recommends running it as a Homebrew/macOS service because this also handles the appropriate `ulimit` settings. ([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-os-x/?utm_source=chatgpt.com "Install MongoDB Community Edition on macOS - Database Manual v8.0 - MongoDB Docs"))

---

# 9. Check whether MongoDB is running

```zsh
brew services list
```

You should see something similar to:

```text
mongodb-community@8.0    started
```

You can also check the running process:

```zsh
ps aux | grep mongod
```

---

# 10. Connect using `mongosh`

Now open the MongoDB shell:

```zsh
mongosh
```

You should get something similar to:

```text
Current Mongosh Log ID: ...
Connecting to: mongodb://127.0.0.1:27017/
Using MongoDB: 8.0.x

test>
```

🎉 **MongoDB is now running locally.**

---

# 11. Test your first database

Inside `mongosh`:

```javascript
show dbs
```

Create/use a database:

```javascript
use KIET
```

You may see:

```text
switched to db KIET
```

Now create a collection and document:

```javascript
db.students.insertOne({
    name: "Rahul",
    course: "CSE",
    semester: 3
})
```

Check:

```javascript
show collections
```

Then:

```javascript
db.students.find()
```

You should get:

```text
[
  {
    _id: ObjectId('...'),
    name: 'Rahul',
    course: 'CSE',
    semester: 3
  }
]
```

---

## 12. Stop MongoDB

When you want to stop the server:

```zsh
brew services stop mongodb-community@8.0
```

Start it again:

```zsh
brew services start mongodb-community@8.0
```

Restart it:

```zsh
brew services restart mongodb-community@8.0
```

([MongoDB](https://www.mongodb.com/docs/v8.0/tutorial/install-mongodb-on-os-x/?msockid=0ee1596cebaa6b7e3b104f87ea0a6a85&utm_source=chatgpt.com "Install MongoDB Community Edition on macOS - Database Manual v8.0 - MongoDB Docs"))

---
