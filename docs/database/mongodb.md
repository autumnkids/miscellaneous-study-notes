# MongoDB

_Knowledge collected from the [article](https://medium.com/@hnasr/mongodb-internal-architecture-9a32f1403d6f) and [presentation](https://youtu.be/ONzdr4SmOng) by Hussein Nasser._

## How It Works Internally

MongoDB is a document based database, where schema is not strictly defined between documents in a collection - i.e. one field(s) can exist in one document but not the others in the same collection.

### Binary JSON (BSON)

BSON is the format that stores the JSON document in the disk. [[Reference from the official doc](https://www.mongodb.com/json-and-bson)]

> BSON stands for “Binary JSON,” and that’s exactly what it was invented to be. BSON’s binary structure encodes type and length information, which allows it to be traversed much more quickly compared to JSON.

### The `_id` field

An `_id` is created when a document is created in a collection, which is a field with a size of 12 bytes. The reason why it's large is because MongoDB tries to make it globaly unique across machines or shards for scaling purposes. This field is used to locate a document eventually, but the implementation of how the `_id` field map to a document evolves in different versions.

### Secondary Index

The secondary index is used to accelerate the lookup of specific information in a document. Similarly, a secondary index locates a document eventually based on the specified field, but the implementation varies during evolution of the Storage Engine in MongoDB.

### MMAP_v1 (< 4.2)

MMAP_v1 is the first version of the MongoDB Storage Engine, which maps the `_id` field to a `Diskloc` that locates a document in the disk with a filename and an offset. The downside of this design is that the offset needs to be maintained carefully for each update, otherwise the lookup of documents will be off.

Another downside is that it only allows a single write to the DB at a time, where a global lock is required when it happens, which could lead to performance concerns as it scales.

### WiredTiger v1 (4.2 - 5.2)

WiredTiger is a Storage Engine acquired by MongoDB. It comes with better performance where two concurrent writes to different documents in the same collection becomes possible.

This implementation inroduced a hidden clustered index where the actions required to locate a document involved two B+ Tree lookups. The `_id` field is firstly used to locate a `recordId`, which then is used in the hidden clustered index to locate the corresponding document. With this design, the storage engine is able to load more documents with fewer I/Os since the `recordId` is relatively small - i.e. 64 bit.

> A clustered index is an index where a lookup gives you all what you need, all fields are stored in the leaf page resulting in what is commonly known in database systems as Index-only scans.

However, the downside is that each operation now requires two B+ Tree lookups, which may require more computer resources to operate on, which could lead to performance concerns.

### WiredTiger v2 (> 5.3)

An upgraded version of this storage engine removes the lookup between the `_id` index and the `recordId`, and instead, maintains the `_id` index and the documents in a clustered index such that a lookup with primary index only needs to traverse one B+ Tree. This architecture is called Clustered Collections Architecture.

The secondary index though now has to point to the `_id` field with this option. Since the `_id` field is a field with a size of 12 bytes, this design results in loading a lot more data in secondary indexes, comparing to the v1 design where the `recordId` is only 64 bits. This also means that the secondary index would still have to do two lookups in this design.

However, MongoDB gives users the option to cluster the collections or not, which would be a design tradeoff to think about when using MongoDB.

