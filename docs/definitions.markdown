---
layout: page
title: Definitions
permalink: /definitions/
nav_order: 4
---

# Definitions (Glossary)

The following table debunks some of the terminology used within the research documents.

|id |Concept description|Example|AECdeltas naming |Speckle naming | 3D Repo naming | BHoM naming | Other proposed namings | Notes |
|---|--------|--------|-------|--------|--------|-------|---------|--------|
|1  |Container for the objects|Part of a building (e.g. a portal frame; mep pipes)  |Stream |Stream |Model |-  |Container, Project  ||
|2  |Given state in time of the Container (1) | |Revision   |(Parent) Stream|  |   |Version, Revision, State, Snapshot, LampPost, Checkpoint||
|3  |Container for the "containers" (1) (collection of collections of objects)   |An entire building   |Project|Project|Project (/federation?)|-  |||
|4  |Upload on the online "container" (3) | |Upload |   |  |Push   |POST, PUT   ||
|5  |Download from the online "container" (3) | |Download   |   |  |Pull   |GET ||
|6  |Delta: Objects added in the UI   |E.g. an user creates a new object; that object will then be pushed online on the container   |OnlyA  |   |  |ToBeCreated|New, Created, OnlySetA, AOnly   |In BHoM, the point of view for the object status is looking from the external software. E.g. Created = it's been added in the external software |
|7  |Delta: Objects deleted from UI   | |OnlyB  |   |  |ToDelete   |Old, Deleted, OnlySetB, BOnly   ||
|8  |Delta: Objects modified in UI| |Modified   |   |  |ToModify   |Modified, Intersection-Modified ||
|9  |Delta: Objects not changed in UI | |Unmodified |   |  |-  |Unchanged, NotModified, Intersection-NotModified||
|10 |User choice in front of a conflict status emerged from his upload action |User pushed some change, but server responds with an error, e.g. because the user did not pull the lastest changes in a while|Conflict resolution|   |  |   |Conflict resolution, Pick&Choose, Modify, Merging   ||
|11 |Id of an object; this id does not change when the object changes (it's maintained between different revisions)   | |Persistent ID  |   |  |   |||
|12 |Id of an object; this id changes when the object changes (it's always unique at any point in time)   | |Unique ID  |   |  |   ||Is it an GUID or a hash? Does not matter|
|13 |Delta message, used to transfer the information. Should be as stripped down as possible, minimum amount of information needed| |   |   |  |   |DeltaMessage, DeltaPayload, ||
|14 |Delta as result of the collection level diffing  | |   |   |  |   |Delta, DeltaDiff||
|15 |Delta as result of the property level diffing| |DeltaStream|   |  |   |DeltaStream, HistoricalDelta, DeltaHistory, DeltaTracked||
