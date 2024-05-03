# API

Application Program Interface i.e API is a software intermediary that allows 2 applications to communicate with one another.  
Using API's client/serve can communicate with another serve.

## GET METHOD

The get method in Express.js is used to define routes that respond to HTTP GET requests.

```js
const express = require("express");
const { open } = require("sqlite");
const sqlite3 = require("sqlite3");
const path = require("path");
const app = express();

const dbpath = path.join(__dirname, "goodreads.db");
let db = null;
// initializeDbServer this returns an promise object so we sue the async and await operations.
const initializeDbServer = async () => {
  try {
    db = await open({
      filename: dbpath,
      driver: sqlite3.Database,
    });

    app.listen(3000, () => {
      console.log("SERVER RUNNING");
    });
  } catch (e) {
    console.log(`DB ERROR : ${e.message}`);
    process.exit(1);
  }
};

initializeDbServer();

app.get("/books/", async (request, response) => {
  const getBookQuer = `
        select * from book order by book_id;
    `;

  const booksArray = await db.all(getBookQuer);
  response.send(booksArray);
});
```
