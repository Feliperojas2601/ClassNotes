## CRUD - NodeJs

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Node usa BSON, optimizado, seguro, extiende tipos de datos. En node podemos representar estos documentos como objetos de Js.
  - insertOne e insertMany, find y findOne, updateOne y updateMany, deleteOne y deleteMany
  - Transacciones, operaciones en conjunto como unidad o devuelta de todo. Atomicidad. Bajo la misma sesión de cliente, definir las opciones, la secuencia de operaciones con opción {session} y liberar los recursos. Tienen un límite de 60 secs. 
- **Código:**
  ```javascript
    const dbname = "bank"
    const collection_name = "accounts"
    
    const accountsCollection = client.db(dbname).collection(collection_name)

    const sampleAccount = {
    account_holder: "Linus Torvalds",
    account_id: "MDB829001337",
    account_type: "checking",
    balance: 50352434,
    }

    const main = async () => {
    try {
      await connectToDatabase()
      // insertOne method is used here to insert the sampleAccount document
      let result = await accountsCollection.insertOne(sampleAccount)
      console.log(`Inserted document: ${result.insertedId}`)
    } catch (err) {
      console.error(`Error inserting document: ${err}`)
    } finally {
      await client.close()
    }
    }
    
    main()

    const dbname = "bank"
    const collection_name = "accounts"
    
    const accountsCollection = client.db(dbname).collection(collection_name)

    const sampleAccounts = [
    {
      account_id: "MDB011235813",
      account_holder: "Ada Lovelace",
      account_type: "checking",
      balance: 60218,
    },
    {
      account_id: "MDB829000001",
      account_holder: "Muhammad ibn Musa al-Khwarizmi",
      account_type: "savings",
      balance: 267914296,
    },
    ]
    
    const main = async () => {
    try {
      await connectToDatabase()
      let result = await accountsCollection.insertMany(sampleAccounts)
      console.log(`Inserted ${result.insertedCount} documents`)
      console.log(result)
    } catch (err) {
      console.error(`Error inserting documents: ${err}`)
    } finally {
      await client.close()
    }
    }

    main()

    const collection_name = "accounts"
 
    const accountsCollection = client.db(dbname).collection(collection_name)

    const documentsToFind = { balance: { $gt: 4700 } }
    
    const main = async () => {
    try {
      await connectToDatabase()
      let result = accountsCollection.find(documentsToFind)
      // let result = accountsCollection.findOne(documentsToFind)
      let docCount = accountsCollection.countDocuments(documentsToFind)
      await result.forEach((doc) => console.log(doc))
      console.log(`Found ${await docCount} documents`)
    } catch (err) {
      console.error(`Error finding documents: ${err}`)
    } finally {
      await client.close()
    }
    }

    main()

    const dbname = "bank"
    const collection_name = "accounts"

    const accountsCollection = client.db(dbname).collection(collection_name)

    const documentToUpdate = { _id: ObjectId("62d6e04ecab6d8e130497482") }

    const update = { $inc: { balance: 100 } }


    const main = async () => {
      try {
        await connectToDatabase()
        let result = await accountsCollection.updateOne(documentToUpdate, update)
        result.modifiedCount === 1
          ? console.log("Updated one document")
          : console.log("No documents updated")
        /* let result = await accountsCollection.updateMany (documentsToUpdate, update)
        result.modifiedCount > 0
        ? console.log(`Updated ${result.modifiedCount} documents`)
        : console.log("No documents updated") */
      } catch (err) {
        console.error(`Error updating document: ${err}`)
      } finally {
        await client.close()
      }
    }

    main()

    const dbname = "bank"
    const collection_name = "accounts"

    const accountsCollection = client.db(dbname).collection(collection_name)

    const documentToDelete = { _id: ObjectId("62d6e04ecab6d8e13049749c") }

    const main = async () => {
      try {
        await connectToDatabase()
        let result = await accountsCollection.deleteOne(documentToDelete)
        result.deletedCount === 1
          ? console.log("Deleted one document")
          : console.log("No documents deleted")
        /* let result = await accountsCollection.deleteMany(documentsToDelete)
        result.deletedCount > 0
        ? console.log(`Deleted ${result.deletedCount} documents`)
        : console.log("No documents deleted") */
      } catch (err) {
        console.error(`Error deleting documents: ${err}`)
      } finally {
        await client.close()
      }
    }

    main()

    // Collections
    const accounts = client.db("bank").collection("accounts")
    const transfers = client.db("bank").collection("transfers")

    // Account information
    let account_id_sender = "MDB574189300"
    let account_id_receiver = "MDB343652528"
    let transaction_amount = 100

    const session = client.startSession()

    const transactionResults = await session.withTransaction(async () => {
      try {
      const senderUpdate = await accounts.updateOne(
        { account_id: account_id_sender },
        { $inc: { balance: -transaction_amount } },
        { session }
      )

      const receiverUpdate = await accounts.updateOne(
        { account_id: account_id_receiver },
        { $inc: { balance: transaction_amount } },
        { session }
      )

      const transfer = {
        transfer_id: "TR21872187",
        amount: 100,
        from_account: account_id_sender,
        to_account: account_id_receiver,
      }

      const insertTransferResults = await transfers.insertOne(transfer, { session })

      const updateSenderTransferResults = await accounts.updateOne(
        { account_id: account_id_sender },
        { $push: { transfers_complete: transfer.transfer_id } },
        { session }
      )

      const updateReceiverTransferResults = await accounts.updateOne(
        { account_id: account_id_receiver },
        { $push: { transfers_complete: transfer.transfer_id } },
        { session }
      )

      if (transactionResults) {
        console.log("Transaction completed successfully.")
      } else {
        console.log("Transaction failed.")
      }
      } catch (err) {
        console.error(`Transaction aborted: ${err}`)
        process.exit(1)
      } finally {
        await session.endSession()
        await client.close()
      }
    })
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)