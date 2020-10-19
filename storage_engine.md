https://blog.yugabyte.com/a-busy-developers-guide-to-database-storage-engines-the-basics/

A database storage engine is an internal software component that a database server uses to store, read, update, and delete data in the underlying memory and storage systems.

## B-Tree based engine
![](https://cdn-images-1.medium.com/max/1600/0*5ZExOe0pf6SQRfBE.png)


### Pros
- B-trees usually grow wide and shallow, so for most queries very few nodes need to be traversed. 
- The net result is high throughput, low latency reads

### Cons
-  The need to maintain a well-ordered data structure with random writes usually leads to poor write performance
-  This is because random writes to the storage are more expensive than sequential writes.


## Log Structred Merge(LSM)
As data volumes grew in the mid 2000s, it became necessary to write larger datasets to databases. B-tree engines fell out of favor given their poor write performance. 


Database designers turned to a new data structure called log-structured merge-tree (or LSM tree)

> The LSM tree is a data structure with performance characteristics best fit for indexed access to files with high write volume over an extended period.

![](https://cdn-images-1.medium.com/max/1600/1*tZk0ilLtcqQsuyU4dncQdw.png)


### Pros
- LSM engines are the de facto standard today for handling workloads with large fast-growing data. 
- Fast sequential writes (as opposed to slow random writes in B-tree engines)

### Cons
- LSM tree based exhibit poor read throughput in comparision to B-tree based engine
- LSM engines consume more CPU resources during read operations and take more memory/disk storage.
However these issues get mitigated in practice:
- Reads are made faster with approaches such as bloom filters

### Real world usage
- Apache Cassandra, Elasticsearch, RocksDB, InfluxDB

## Summary
![](https://blog.yugabyte.com/wp-content/uploads/2018/06/database-storage-engines-b-tree-vs-lsm-tree.png)

At the core, database storage engines are usually optimized for either read performance (B-tree) or write performance (LSM).

In the last 10+ years data volumes have grown significantly and LSM engines have become the standard. LSM engines can be tuned more easily for higher read performance compare to B-tree engines.