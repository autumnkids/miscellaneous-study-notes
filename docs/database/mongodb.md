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

### MMAP_v1

MMAP_v1 is the first version of the MongoDB Storage Engine, which maps the `_id` field to a `Diskloc` that locates a document in the disk with a filename and an offset. The downside of this design is that the offset needs to be maintained carefully for each update, otherwise the lookup of documents will be off.

Another downside is that it only allows a single write to the DB at a time, where a global lock is required when it happens, which could lead to performance concerns as it scales.

### WiredTiger v1

WiredTiger is a Storage Engine acquired by MongoDB. It comes with better performance where concurrent writes is introduced. But the tradeoff in v1 is that both the `_id` lookup and the secondary index lookup are now required two B+ Tree lookup, since a hidden clustered index is introduced between the index and the BSON document in this version of implementation. The `_id` field firstly points to a `recordId` which then is used in the hidden clustered index to locate the document from the disk.

