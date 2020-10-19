# Why NoSQL?
- Flexible, schemaless data modeling.
- Help dealing with data in **Aggregates**
- Support large volumes of data by running on clusters. Relational databases are not designed to run effciently on clusters

## Aggregate data model [1]
- Dealing in aggregates make it much easier for these databases to handle operating on a cluster, sinces the aggregate makes a natural unit for replication and sharding.
  
- Aggregates are often easier for application programmers to work with, since they often manipulate data through aggregate structures.

### Relations and Aggregates
#### Relation
![](images/2020-10-15-10-33-23.png)

#### Aggregate
```js
// in customers
{
    "id":1,
    "name":"Martin",
    "billingAddress":[{"city":"Chicago"}]
}
// in orders
{
    "id":99,
    "customerId":1,
    "orderItems":[
        {
        "productId":27,
        "price": 32.45,
        "productName": "NoSQL Distilled"
        }
    ],
    "shippingAddress":[{"city":"Chicago"}]
    "orderPayment":[
        {
        "ccinfo":"1000-1000-1000-1000",
        "txnId":"abelif879rft",
        "billingAddress": {"city": "Chicago"}
        }
    ],
}
```

## Types of data models
### Key-value and document data model
Key-value and document databases were strongly aggregate-oriented.
- We can access an aggregate in a key value store based on it keys. Redis allows you to break down the aggregate into lists or sets
- We can access an aggregate in a document database by submitting queries
- With key-value databases, we expect to
mostly look up aggregates using a `key`. With document databases, we mostly expect to submit some
form of `query` based on the internal structure of the document.

### Column-family stores
There are many scenarios when you often read a few columns of many rows at once.
- It's better to to store store groups of columns for all rows as the basic storage unit in the disk. 
![](images/2020-10-15-10-41-34.png)

In an RBDMS, the tuples would be stored row-wise, so the data on the disk would be stored as
follows:

|John,Smith,42|Bill,Cox,23|Jeff,Dean,35|

In online-transaction-processing (OLTP) applications, the I/O pattern is mostly reading and writing
all of the values for entire records. As a result, row-wise storage is optimal for OLTP databases.

In a columnar database, however, all of the columns are stored together as follows:
|John,Bill,Jeff|Smith,Cox,Dean|42,23,35|

The advantage here is that if we want to read values such as Firstname, reading one disk block reads a lot more information in the row-oriented case
since each block holds the similar
type of data, is that we can use efficient compression for the block, further reducing disk space and
I/O.

Examples include  `Cassandra`

## Consistency

## Reference
[1] https://www.amazon.com/NoSQL-Distilled-Emerging-Polyglot-Persistence/dp/0321826620

