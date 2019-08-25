### Speckle Diff Endpoint

Diffing in speckle operates at an object collection level (speckle stream). 

**OpenAPI specification**: [link](https://speckleworks.github.io/SpeckleSpecs/#streamdiff) ([source](https://github.com/speckleworks/SpeckleSpecs/blob/master/paths/streams/%7BstreamId%7D_diff_%7BotherStreamId%7D.yaml))

Anatomy: 
```
https://hestia.speckle.works/api/streams/{streamId_A}/diff/{streamId_B})
                                             ^              ^
                                             |              |
                                         "Revision 4"   "Revision 2"
                                         eq. "inA"      eq. "inB"    
```

It produces [this response](https://hestia.speckle.works/api/streams/ZcLOC798N/diff/aIDIxSoWR):

```js
{
    "success": true,
    "objects": {
        "common": [
            "5d2e090831e15e1c74db6be0", // objectIds
            "5d2e090831e15e1c74db6be1"
        ],
        "inA": [
            "5d2e091b5586481bfb9cdbfa", // objectIds
            "5b058bcd49b3520b9a4701ba",
            // ...
        ],
        "inB": [
            "5d2e090831e15e1c74db6bdf", // objectIds
            "5d2e090831e15e1c74db6be2"
        ]
    },
    // ...
}
```

Equivalences with the current delta spec:

```js
{
   "project_id": n/a,
   "created": inB,
   "deleted": inA,
   "updated": common,
   "version": {
     from: streamId_A,
     to: streamId_B
   },
}
```

## Required End-points
- Create a project
- Create a stream
- Download stream
- List of all revisions
- Upload a new revision of a stream (provide stream ID)

