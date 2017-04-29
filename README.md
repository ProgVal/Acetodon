# Acetodon
Future-proof, distributed, and minimalist object storage system

## Project goals

### Easy to use

* Easy to set up
* No complicated vocabulary
* Simple HTTP API both for managing objects and retrieving them, using standard HTTP verbs (GET, PUT, DELETE). Compatible with AWS S3 clients.
* Buckets can be initialized from a directory in the filesystem without writing to it.

### Future-proof

* Can be replaced by a regular filesystem (and a regular HTTP server) with a single command (eg. `scp`).
* Simple storage format. Objects should be stored as simple files whose name is their identifier, so that losing metadata does not overly complicate data retrieval.
* Minimalist resource server. It should be simple enough to be quickly reimplemented from scratch by any developper in case it breaks. Any data should be recoverable using only `ls`, `cat`, and `cp`.

### Other features

* Load-balancing between hosts.
* Movement of objects between hosts depending on traffic.
* Periodic checks of object content to compensate for data rot.

## Limitations

|Limitation                                     |Reason                                                        |Workaround                                                     |
|-----------------------------------------------|--------------------------------------------------------------|---------------------------------------------------------------|
|No fancy access restriction                    |Avoid security issues caused by bugs or misconfiguration.     |None                                                           |
|No built-in deduplication                      |Goes against minimalism.                                      |Application-level deduplication                                |
|No built-in compression                        |Goes against minimalism.                                      |Application-level compression, FS-level compression            |
|Limited character support in object names      |Object names are mapped to filesystem names                   |Application-level handling of file names                       |
|No object/file striping                        |Prevents recovery of object using only `ls` and `cp`.         |None                                                           |
|Not infinitely scalable                        |Recursive Read nodes store a list of all objects.             |None                                                           |

## Architecture

### Buckets

A set of Acetodon nodes communicating with each other is called a Bucket.

### Write Nodes

Storage Nodes answer `PUT` and `DELETE` requests for adding or deleting objects.

### Read nodes

Read Nodes answer `GET` requests for objects.

Some Read Nodes may transfer objects from other Read Nodes if it is not available locally, they are called Recursive Read Nodes.

## Format used

Objects themselves are stored as plain files.
Files are stored in subdirectories of the host's storage root; the name of the subdirectory is 

## Usage

### Create a new bucket

TODO

### Stop using Acetodon

If for some reason you want to stop using Acetodon, you can easily revert back to a regular HTTP server:

```
mkdir asserts
scp -r "host1:~/bucket/*" "host2:~/bucket/*" assets/
cd assets/
python3 -m http.server --bind 0.0.0.0 --port 8000
```

### Run a Write Node

TODO

### Run a Read Node

TODO

## Configuration

### Connect to other nodes

TODO

### Limits

TODO: disk limit, traffic limit

### Hints

TODO: indication of network bandwidth, disk bandwidth
