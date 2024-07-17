## CRUD - Aggregation With NodeJs

### Fecha: 17/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - En NodeJs, construimos una variable pipeline y la igualamos a un array de stages, luego usamos el método aggregate de una colección. 
- **Código:**
  ```javascript
  const pipeline = [
    { $match: { balance: { $lt: 1000 } } },
    {
      $group: {
        _id: "$account_type",
        total_balance: { $sum: "$balance" },
        avg_balance: { $avg: "$balance" },
      },
    },
  ]
  const main = async () => {
    try {
      await client.connect()
      console.log(`Connected to the database 🌍. \nFull connection string: ${safeURI}`)
      let result = await accountsCollection.aggregate(pipeline)
      for await (const doc of result) {
        console.log(doc)
      }
    } catch (err) {
      console.error(`Error connecting to the database: ${err}`)
    } finally {
      await client.close()
    }
  }
  main()
  const pipeline = [
    { $match: { account_type: "checking", balance: { $gte: 1500 } } },
    { $sort: { balance: -1 } },
    {
      $project: {
        _id: 0,
        account_id: 1,
        account_type: 1,
        balance: 1,
        gbp_balance: { $divide: ["$balance", 1.3] },
      },
    },
  ]
  const main = async () => {
    try {
      await client.connect()
      console.log(`Connected to the database 🌍\n ${uri}`)
      let accounts = client.db("bank").collection("accounts")
      let result = await accounts.aggregate(pipeline)
      for await (const doc of result) {
        console.log(doc)
      }
    } catch (err) {
      console.error(`Error connecting to the database: ${err}`)
    } finally {
      await client.close()
    }
  }
  main()
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)