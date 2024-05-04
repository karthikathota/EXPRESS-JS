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

## POST

```js
app.post("/books/", async (request, response) => {
  let bookDetails = request.body;

  const {
    title,
    authorId,
    rating,
    ratingCount,
    reviewCount,
    description,
    pages,
    dateOfPublication,
    editionLanguage,
    price,
    onlineStores,
  } = bookDetails;
  const addBookQuery = `
    INSERT INTO
      book (title,author_id,rating,rating_count,review_count,description,pages,date_of_publication,edition_language,price,online_stores)
    VALUES
      (
        '${title}',
         ${authorId},
         ${rating},
         ${ratingCount},
         ${reviewCount},
        '${description}',
         ${pages},
        '${dateOfPublication}',
        '${editionLanguage}',
         ${price},
        '${onlineStores}'
      );`;
  let dbResponse = await db.run(addBookQuery);
  let bookId = dbResponse.lastID;
  response.send({ bookId: bookId });
});
```
