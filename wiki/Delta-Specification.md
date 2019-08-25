# Introduction

In our definition, the smallest unit of change is an **AEC object**. Such an object is simply a collection of key-value pairs describing an AEC-specific entity such as a _steel beam_, _window_, _slab_ etc., with its repsective properties such as an `id`, `length`, `fire_rating`, etc. However, we do not subscribe to any one specific data representation or encoding. That implies that the definition of an object can be based on anything such as an IFC Object but also a BHoM Object, a 3D Repo Object, a Speckle Object or any other representation as required. Ultimately, an object is simply a **binary blob** that some but not necessarily all applications would be able to decode, interpret and convert into their native representation of what the respective properties relate to.

Therefore, there is a need to maintain a set of **converters** that provide the necessary linkage between various representations of objects and their properties within the native applications. This will be achieved via a **look-up table** which defines the one-to-one mapping between the properties. For simplicity, we assume that each property only maps to one counterpart property in the receiving application and that not all properties necessarily need to be mapped across. After all, it is often the case that the receiving application is only interested in a subset of the object properties to achieve its specific function. Note that the [BHoM library](https://bhom.xyz/) by BuroHappold Engineering already contains such converters known as toolkits, see [https://github.com/BHoM](https://github.com/BHoM) for details.

#### Base Principles
* **A single object is the smallest unit of change.** Even if the smallest of properties on an object has been changed, the whole object needs to be flagged accordingly. For instance, a single vertex in a mesh change would require the whole mesh object to be updated.
* **All objects are immutable.** Any change on an object means the object has been modified, and thus a new object with a new ID/hash needs to be created to replace it and advance the revision further.
* **Each object has a unique and a shared ID.** Each object by definition requires a unique ID (UID) in order to uniquely identify it. In addition, it also requires a shared ID (SID) which is shared across various instances/revisions of the same object over time.
* **Not all objects have to have a visual representation.** There are objects that simply have no visual representation such as a renderable mesh attached. For instance, a steel beam can be represented by its `load capacity` and `length` in order to perform structural analysis calculations even though it would not be renderable directly as is.

## Deltas

A **Delta (∆)**  is a set of objects that have been identified as either `created`, `deleted` or `updated` between two applications. The trivial example is an empty set `{}` whereby no change has been detected. **Differencing** or **_"diffing"_** for short is then the process of establishing the delta set of objects between two revisions or states of host applications holding the AEC information in memory.

In mathematical notation, we would write:
```
∆ = { C, D, U },
```
where `C`, `D` and `U` are sets of created/added, deleted and updated/modified AEC objects respectively. The naming convention was selected to closely follow the [CRUD (create, read, update and delete)](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) principles of persistent storage manipulation.

However, it is important to note that differencing itself can be implemented at two levels:
- **Level 1: Collection/object-level differencing.** Input are two collections and the result is a list of changed objects. This happens at the AEC Deltas middleware layer and is integral to our definition;
- **Level 2: Property-level differencing.**  Differencing of individual line-by-line properties only happens at the application layer and is left open to implementation as required by the receiving application. This, therefore, is not part of the AEC Deltas middleware but can be delivered on top.

Here, we only concentrate on the object-level differencing (Level 1). Objects are being freely exchanged but it is the application's responsibility to determine what happens with this information on the client side. For instance, the receiving application might decide to consume only some of the updates, ignore all of them or pass them on unmodified depending on the user requirements and input. Note that the source application is the one responsible for notifying what the deltas/changes are between itself and the receiving application.

### Delta Definition
> _Delta definition is currently a work in progress and will be updated over the next two quarters._

Each delta set should have the following base fields in order to properly identify the authorship and ownership of the information that is carried within.
- **Project ID** - `uuid` unique ID in [RFC 4122 v4](https://tools.ietf.org/html/rfc4122) format (unique to each project)
- **Author** - `string`
- **Timestamp** - `time_t` timestamp in seconds from the Unix epoch
- **Payload** - `string` or `binary`
- **Cryptographic signature** - signed by a private key of the author, i.e. an [RSA cryptosystem](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) of the delta itself

In a JavaScript notation, the encoding of a delta would look like this:
```
delta = {
  "stream_id": <uuid>,
  "created": [ 
                 [ uid_1, sid_1, mesh_1, material/colour_1, metadata_1, authoring_tool_1, ... ], 
                 [ uid_2, sid_2, mesh_2, material/colour_2, metadata_2, authoring_tool_2, ... ], 
                 [ uid_3, sid_3, mesh_3, material/colour_3, metadata_3, authoring_tool_3, ... ], 
                 ... 
              ],
   "deleted": [ sid_4, sid_5, sid_6, ... ],
   "updated": [ 
                 [ uid_7, sid_7, mesh_7, material/colour_7, metadata_7, authoring_tool_7, ... ], 
                 [ uid_8, sid_8, mesh_8, material/colour_8, metadata_8, authoring_tool_8, ... ], 
                 [ uid_9, sid_9, mesh_9, material/colour_9, metadata_9, authoring_tool_9, ... ], 
                 ... 
               ]
   "revision_A": from_id,
   "revision_B": to_id,
   "timestamp": <time_t>,
   "signature": <base64 AES string>,
   "sender": <string> // E.g. BHoM, 3D Repo, Speckle, etc.
}
```

# Dataflow
> _Dataflow is currently a work in progress and will be updated over the next two quarters._

Establishing the correct dataflow between various applications and servers is essential to proper design of the delta set and the subsequent communication protocol.

A naïve protocol would look like this:

1. The status of the latest revision is checked;
1. Revision ID is obtained:
     1. ID is recognised, or
     1. ID is not recognised, a list of all revision IDs is requested:
         1. Overlap is found with some earlier revision, or
         1. Overlap is not found, all object IDs of the latest revision are requested;
1. Diff is calculated;
1. Delta updated is sent across.

## Delta Sequence Diagrams

> _A sequence diagram shows, as parallel vertical lines (lifelines), different processes or objects that live simultaneously, and, as horizontal arrows, the messages exchanged between them, in the order in which they occur. This allows the specification of simple runtime scenarios in a graphical manner._ -[Wikipedia](https://en.wikipedia.org/wiki/Sequence_diagram)

Our proposed protocol can be translated into five different sequence flow scenarios as follows:
1. **Push of a new model.** The base case is to send a new model/project to the server and/or client application for the very first time.
1. **Server is behind.** When the server is behind the latest revision, the client calculates the differences and sends a delta upstream.
1. **Conflict:**
    1. **Client is behind.** Client is working on a revision which becomes out of date. It then calculates the differences and sends the delta upstream.
    1. **Two clients are pushing the same revision.** Two clients update the same revision simultaneously which leads to one of them being out of sync. The affected client then calculates the differences and sends a delta upstream.
1. **Client is pushing an existing revision.** Client pushing an existing revision to the server results in a `null` delta.

![scenario1](https://user-images.githubusercontent.com/3008807/61866584-550db080-aecd-11e9-8733-49a6bfe199f3.png)
### Scenario 1
In Scenario 1, a new project with its first revision gets established on the server. This is the base case whereby the user creates a new project and _"pushes"_ it to the server. Since the server does not recognise this project in its database (because it is new), it obtains the revision data and stores it accordingly. The return message then indicates the transaction completed successfully. In this case, the delta is the entire model.

***

![scenario2](https://user-images.githubusercontent.com/3008807/61866701-900fe400-aecd-11e9-9496-74abe2e8e67b.png)
### Scenario 2
In Scenario 2, the client performs changes locally and thus advances the revision forward meaning the server is left behind. Thus, the client retrieves the latest revision, enacts changes locally and pushes a new advanced revision back to the server. Since the server is behind, it responds with its latest revision number which is used by the client to calculate the delta in order to send it upstream to the server. The return message then indicates the transaction completed successfully.

***

![scenario3a](https://user-images.githubusercontent.com/3008807/62529096-e23df700-b835-11e9-86c7-7be5956b7f08.png)
### Scenario 3a
In Scenario 3a, two different clients, say `A` and `B`, retrieve the same revision from the server and perform their respective changes locally independently of each other. Then, client `B` pushes their changes to the server by enacting Scenario 2. Client `A` attempts to achieve the same, however is in conflict since their revision is now behind the server. Thus, client `A` calculates the differences and pushes the delta to the server or decides to postpone the action to a later date. The return message then indicates the transaction completed successfully.

***

![scenario3b](https://user-images.githubusercontent.com/3008807/62529206-1e715780-b836-11e9-91e0-078037a95edc.png)
### Scenario 3b

> _This scenario will be updated and expanded upon in the next two quarters._

In Scenario 3b, two different clients, say `A` and `B`, retrieve the same revision potentially from different sources and then attempt to push their respective changes enacting Scenario 2 and 3a. However, instead of concluding with sending a delta back to the server, client `A` performs local updates and then enacts Scenario 1 in order to advance the revision further. The return message then indicates the transaction completed successfully.

***

![scenario4](https://user-images.githubusercontent.com/3008807/62529407-7c05a400-b836-11e9-9a96-fec27e8bb752.png)
### Scenario 4
In Scenario 4, a client is passing a revision from server `01` to server `02`. However, `02` is already at the latest revision, thus the delta calculated by the client is `null` and no further data needs to be exchanged. The return message then indicates the transaction completed successfully.

# Security
Security is of paramount importance for the AEC Deltas project since the kind of information being exchanged between architects, designers, engineers and contractors is often sensitive in nature. What is more, there are instances where access to certain types of data might be restricted only to a few select companies/individuals such as the CCTV cameras package, security doors, etc.

From a security standpoint, we need to consider three main requirements of the delta collaboration:
* **Authentication** - The ability to verify the source of the delta;
* **Integrity** - The ability to verify that the delta has not been altered during transmission;
* **Non-repudiation** - The ability to prevent the author from ever denying that they originated the delta.

Meeting these base requirements will ensure that each delta can be safely processed, it can be attributed to its respective author and the users of the system can be sure it has not been tampered with.

## Encryption
A lot of inspiration has been taken from existing messaging and e-mail protocols. For our purposes, we propose the use of the [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) hashing via [OpenSSL](https://www.openssl.org/). MongoDB provides crypto utilities natively, however, those rely on symmetric encryption with a single key which would not be suitable for our use case, see https://docs.mongodb.com/stitch/functions/utilities/#utils-crypto for details. Each system user, also known as an actor, will establish a secure [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) connection with the [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protocol with the centralised server in order to exchange individual deltas but also communicate via the [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) [API](https://en.wikipedia.org/wiki/Application_programming_interface) end-points.

### Required Components
The main components of the security layer of the system are as follows:
* **Digital signature.** Each delta will be digitally signed in order to validate its authenticity and integrity.
* **Public-private key pairs.** Each actor within the system will distribute their public key in order for the other actors to validate the digital signatures of the deltas.
* **Transport Security Layer (TLS) connection.** Secure network connection for delta exchange between the actors.

Each delta will be accompanied by a digital signature which hashes the contents of the payload using an asymmetric private-key in order to validate the author of the delta and the integrity of the message. A central server will then store the respective deltas in order of receiving them so as to provide an independent audit trail of changes over time. Pushing a new delta will automatically trigger the creation of a new incremental revision within the system.

In order to obtain a public-private key certificate, there are ready-available and free to use certificate authorities such as https://letsencrypt.org Another solution to consider is a distributed cloud storage system [EdgeFS](http://edgefs.io) which provides high throughout with on-write automatic synchronisation across geographically distributed locations.