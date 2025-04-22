### Seeding the Database

There are **three seed scripts** available to help populate the database with mock data:

- **Inside the `scripts/` folder**:
  - `generateMockData.js`
  - `seedDatabase.js`

- **Outside the `scripts/` folder**:
  - `seed.js`

> **Note for `seed.js` users:**  
If `seed.js` fails to connect to the database, it might be due to the MongoDB URL using `localhost`. In some environments, `localhost` doesn’t resolve properly. Try changing the connection string from:

```
mongodb://localhost:27017/pinterest-clone
```

to:

```
mongodb://127.0.0.1:27017/pinterest-clone
```

That usually does the trick.

