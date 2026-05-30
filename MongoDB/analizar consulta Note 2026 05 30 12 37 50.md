# analizar consulta Note 2026 05 30 12 37 50

import { MongoClient } from 'mongodb';

/*

- Requires the MongoDB Node.js Driver
- [https://mongodb.github.io/node-mongodb-native](https://mongodb.github.io/node-mongodb-native)
 */

const filter = {
  'periodo': '2025'
};
const sort = {
  'createdAt': -1
};
const limit = 10;

const client = await MongoClient.connect(
  'mongodb://10.10.16.11:27017/actuaciondbactuaciondb?replicaSet=rs0&directConnection=true'
);
const coll = client.db('actuaciondb').collection('multas');
const cursor = coll.find(filter, { sort, limit });
const result = await cursor.toArray();
await client.close();