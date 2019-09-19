---
layout: page
title: Requirements
permalink: /requirements/
nav_order: 1
---
# Requirements
{: .no_toc }

> _Requirements are descriptions of the services that a software system must provide and the constraints under which it must operate. Requirements can range from high-level abstract statements of services or system constraints to detailed mathematical functional specifications. [Source](http://www.inf.ed.ac.uk/teaching/courses/cs2/LectureNotes/CS2Ah/SoftEng/se02.pdf)_

The AEC Delta Mobility project aims to define a new middle-ware layer to support exchange of small incremental changes between AEC-specific software packages either locally or within the cloud. This document lists the end-user requirements as well as individual system requirements to drive the software specification going forward.

# Table Of Contents
{: .no_toc }

1. TOC
{:toc}
# User Requirements

> _User requirements are defined using natural language, tables and diagrams._

## Actors
1. Admin
1. User (any user): Planner, Design Coordinator/Project Manager, Consultant/Engineer, Contractor, Architect, Field Operator
1. Software Developer
1. Observer (can see data but cannot commit)


## User Stories


> _As a **\<role\>**, I want **\<feature\>** so that **\<reason\>**._



### US1.1: Planner

_As a **Planner**, I want to **associate planning activities to design objects** so that I can **plan activities in planning software** (e.g. Tilos, Primavera P6, Bentley Synchro, Microsoft Project, etc.)._

![planner](https://user-images.githubusercontent.com/3008807/56722740-96ac8500-673f-11e9-8542-9a5d734adf48.png)

This includes quantities take off, object definitions, schedules and plans in software such as Tilos, Primavera P6, Synchro, etc. Ultimately, the user wants to see how model changes affect the activities.

1. Planner opens all the BIM models and federates them (most likely in Solibri or Navisworks or 3D Repo);
1. Create a planning view from the given federated models;
1. Create relevant planning activities;
1. Export into a sub-plan and save the configuration in order to be able to re-apply.


![planner-detailed](https://user-images.githubusercontent.com/3008807/56723264-86e17080-6740-11e9-8953-8472e6edaa7c.png)

### US1.2: Estimator 
_As an **Estimator**, I want to **create a model view on the design scope** so that I can **bring it into an estimation software and create a cost estimate**._

![estimator](https://user-images.githubusercontent.com/3008807/56723456-e63f8080-6740-11e9-9925-204496aaaa16.png)

Software such as CostX and Ribi2.

![costing](https://user-images.githubusercontent.com/3008807/56737642-c456f580-6762-11e9-82fe-bda37c8d04d0.png)

---

### US2.1: Design Coordinator/Project Manager

_As a **Design Coordinator/Project Manager**, I want to **collect and federate the latest BIM models** so that I can **assign design tasks/issues back to the individual Consultants/Contractors to resolve**._

![information-manager](https://user-images.githubusercontent.com/3008807/56736686-b30ce980-6760-11e9-96a7-30e9ca159a7e.png)

### US2.2: Consultant/Contractor 

_As a **Consultant/Contractor**, I want to **be told what issues to resolve** so that **I can improve my design and deliver a new iteration**._

This user requirement effectively defines a design feedback loop whereby the models are regularly updated and federated in systems like 3D Repo. There, the issues are collected and passed back onto the Consultants/Contractors to resolve and then the whole process is reiterated.

![design-coordinator](https://user-images.githubusercontent.com/3008807/56735843-8061f180-675e-11e9-8691-594ab21bb74f.png)

---

### US3: Design Coordinator/Project Manager

_As a **Design Coordinator/Project Manager**, I want to **define data compliance checks** so that **individual Consultants/Contractors can self-validate Deltas before sharing**._

![validation](https://user-images.githubusercontent.com/3008807/56736511-3c6fec00-6760-11e9-8821-a3b421637022.png)

Standard approach especially in the contractor world is to define Employers Information Requirements (EIRs) and BIM Execution Plan which specify the BIM data requirements on a project. Currently, there is no automated way to validate these have been met when exchanging data.

---

### US4: Consultant

_As a **Consultant**, I want to **define the accuracy/certainty/LOD of data/options** so that **other actors know to what degree to rely on the information**._ 

This reflects the Levels of Detail and Levels of Information in BIM projects. For instance, many objects could be labelled as placeholders at the early design stages. However, if this gets implemented, then we also need the ability to automatically validate that all placeholders have been replaced by the end of the project.

---

### US5.1: Consultant

_As a **Consultant**, I want to **have multiple representations of the same object** so that **I can run different analysis for different purposes**._

![conception-stages](https://user-images.githubusercontent.com/3008807/56736839-172fad80-6761-11e9-9a3e-6799a38edf01.png)

It is important that the system records the relationships between the objects, be it siblings or parent-child rels.
Often there would be different representations of the same object for different purposes. A steel beam in architecture would be represented differently to a structural analysis calculation for lateral stress vs top load, etc.

### US5.2: User 

_As a **User**, I want to **have different view of the same data** so that I can **concentrate only what is important to me at any given time**._

![multi-modelling](https://user-images.githubusercontent.com/3008807/56737169-b8b6ff00-6761-11e9-8c62-0cab20c6c921.png)

---

### US6: User

_As a **User**, I want to **set notifications trigger that is specific to me** so that **I get only notified of the changes that I care of**._

![modelling](https://user-images.githubusercontent.com/3008807/56737418-4b579e00-6762-11e9-8826-1b187315c17b.png)

---

### US7: Field Operative 

_As a **Field Operative**, I want to **update progress on site within the model** so that **the project can be controlled and monitored transparently by all other stakeholders/Users**._

![field-operative](https://user-images.githubusercontent.com/3008807/56737711-eb152c00-6762-11e9-818c-429aeb941395.png)

This should help with invoicing (cash turns) and also having _As Installed Model_ ready made.

---

### US8.1: User 

_As a **User**, I want to **decide when I receive updates and when I synchronise with the master data** so that **I am in control of my workflow**._

We need to ensure that the system is not synchronising everything all the time. For instance, if a user actively works on a design moving and creating objects, most other users would not want to get real-time updates whenever a the smallest change occurred. Thus, the user should have the ability to decide if to synchronise manually or receive maybe daily or weekly digest of the latest changes / playback.

### US8.2: User

_As a **User** I want to **have the capacity to get a preview of the changes before synchronising** so that I **do not damage my working copy, should the proposed update not be applicable**._

---

### US9.1: Admin

As an **Admin**, I want to **ensure Developers adhere to the standard** so that **all new software is being interoperable**.

### US9.2: Software Developer

_As a **Software Developer**, I want to **have good documentation** so that **it is easy to adhere to the standard and develop**._ 

---

### US10: User 

_As a **User**, I want to **have different branches/streams/options** so that **I can be in control of the architecture of data sharing** (e.g. in order to mirror the scope of works on a project)._

# System Requirements
> _A structured document setting out detailed descriptions of the system services written as a contract between client and contractor._

## Functional Requirements

> _Statements of services that the system should provide, how the system should react to particular inputs and how the system should behave in particular situation._

**3D Repo**

* Ability to share updates to the server without sharing with other actors until a permission to share project-wide is given.
* Files to be decomposed into object level components that define individual deltas across applications. Each change should be recorded and streamed to the server. There is the possibility that for a peer-to-peer implementation the local application would store all deltas in a sequence locally as a shadow copy (to be discussed) that can be synchronised to;
* Individual changes (deltas) to be streamed one at a time in a sequence. This is to ensure synchronised delivery since the order matters, unlike the time when a change has occurred.
* End-to-end data encryption including at rest. This is the ensure that data is available only to those users that have the appropriate decryption key;
* Ability to identify the author of each change via cryptographic signature. Every delta should be attributable to an individual working on the project as an undeniable proof;
* Permissions-based access control. Only users with the right access privileges are allowed to access data. The question is whether we should account for access to some but not all data such as federations of multiple disciplines, exclusion of sensitive information (eg CCTV models), etc;
* Centralised mapping of fields/elements across different applications with in-built versioning. Each mapping API should be properly versioned and distributed to all parties form a centralised location so that new inputs can be tracked and audited.

---

**HOK**

* The format to support versioning or not?
* Random access, read parts of the dataset (jump to a specific location or just the latest updates);
* A file (local) representation and a server-side representation;
* USD file with a random access, is it useful? Works well for huge datasets. See https://graphics.pixar.com/usd/docs/Usdz-File-Format-Specification.html

See also: https://www.nic.org.uk/wp-content/uploads/Data-for-the-Public-Good-NIC-Report.pdf

---

**BuroHappold**

* Any new adapter interface/protocol will need to provide both server/client and also server-less behaviour as required by the end user. Requires replicated architecture of the server for when off-line (queuing of CRUD/IO operations to then be pushed when back online);
* Object History is recorded;
* Infrastructure to enable (and not preclude) full version control must also be possible;
* Object level diffing (reduction of payload) combined with Object property level comparisons the combination of which allows the potential for efficient comparisons/diffing with inclusion of tolerances also; 
    * data format for protocol should be flexible and allow completely schema-less data - minimal requirements on header/diff for each data packet to be a technical implementation requirement and thus no such formatting requirements should be imposed as a bare minimum for the end user;
* However in addition - possibility that conforming to a defined schema will enable further capability of system (i.e. extraction of a geometry property(s) or predefined metadata schema?!);
* The format for diffing/comparison maybe dependent on the data type? See above;
* objective to minimise payload on each push;
    * store snapshots vs diffs vs actions of user;
* Following the latter of the above - storing actions of user, combined with data source etc. to enable "Data flow tracking/query/viz";

## Non-functional Requirements

> _Constraints on the services or functions offered by the system such as timing constraints, constraints on the development process, standards, etc._

* The system shall process a large BIM federation (i.e. a skyscraper with full architecture, structure and MEP) within 30 minutes. For instance, uploading such a model to 3D Repo will take 5 minutes when loading from Navisworks where the geometry is pre-defined versus 2 hours from direct file uploads where the geometry has to be generated on the fly.
* Two end-points to be able to synchronise over time to the latest state. The most difficult part will be the order of actions and the need to either implement locking or conflict resolution, see here: [3D Diff](https://3drepo.com/publications/3d-diff-an-interactive-approach-to-mesh-differencing-and-conflict-resolution-asia/)

## Domain Requirements

> _Requirements that come from the application domain of the system that reflect the characteristics of that domain_

**3D Repo**

- The System shall have the ability to pull only partial information from the source. I.e. quantities only, not the whole geometric BIM model;
- Structure the model and subdivide according to contractual obligation;
- Provide ability to split the model objects in new ways (i.e. a side of a building slab into individual floors);
- File-format independent delta definition that can support IFC but also any other data representation such as [BHoM](https://bhom.xyz/);
- Scalability to large infrastructure projects. The proposed solution needs to be applicable equally to buildings as well as linear assets. After all, on the data model level, a highway is like a skyscraper, just sideways;
- Ability to exchange either object definitions or triangulated meshes where required. 3D Repo provides some functionality to generate geometry on the fly from object definitions thanks to the [IFCOpenShell library](http://www.ifcopenshell.org/) but assuming authoring tools provide access to generated meshes, those should be a preferred way of retrieving data (due to computational overhead and time constraints);

---

**Rhomberg**

Defining scope of project:
- Horizontal asset delivery (design & construction phase) as per RSRG work in Rail: traditional or Slabtrack - design information
* Focus on new build, with lesser priorities on the "Operate phase"; De- prioritise Re-furbish; these will be dealt in the Project evaluation
* Must include pre-construction and construction workflows, including the simulation of construction
processes.

Process and data needs will be detailed as part of the WP2 in M2 and M3:
* Designing to tender level & planning approval (Grip stage 3 to 4)
* Designing to construction level  & construction approval (Grip Stage 4 to 5)
* Enabling grip 5 information into Grip 3 stage - design for fabrication / early contractor involvement
* Quantifying of material quantities, temporary works, and labour content during GRIP stages 2 to 6 by enabling data exchange between design tools and estimation tools (Bentley and Autodesk to CostX, Tylos and Rib)
* Planning of timescales, site access, material flows during GRIP stage 4, 5, 6 by enabling information exchange between desing tools and planning tools (Bentley and Autodesk to Synchro and Tylos)
* Linking of tool outputs to dashboarding and visualisation tools to present / optioneering outcomes at global KPI level

See https://www.transwilts.org/images/pdf/Guide_to_Rail_Investment_Process-1.pdf for GRIP stages definition

---

**HOK**

* Revit, Archicad, SketchUp, Rhino, CityEngine. Low priority: Tekla, Catia, 3ds Max, Blender

---

**BuroHappold**

* User is in control to work locally or push to server for remote access;

### Side Effects
Results that are not necessarily functional requirements but would be the expected side effects of the project implementation.

- The software connected to AEC Deltas should eventually have the ability to recalculate whatever analysis, design, planning, etc. it is doing automatically/under the control of users, whenever a single change occurs and a delta event is triggered to keep up to date as appropriate.


# Software Specification

> _A detailed software description which can serve as a basis for a design or implementation._

As a knowledge provider partner, UCL puts forward the following that can serve as a base/prior work for the future development of the user requirements above & those mentioned in the grant work packages. The below are not "user requirements" per se, but more "technical requirements/background" based on the [speckle platform](http://github.com/speckleworks) which was previously developed as part of UCL within an H2020 grant. 

## Delta Container Specification (WP3)
- Authorship: the speckle server implements a [discretionary access control](https://en.wikipedia.org/wiki/Discretionary_access_control) layer that assigns an owner to each created resource (object, object collection, etc.). This is implemented via the speckle's api authentication mechanism. 
- Encryption (SSL) is usually handled by the proxy server using standard `letsencrypt` certificates (and is not part of the implementation) 
- Encryption at rest is usually handled by the database (mongodb) deployment (and is not part of the implementation). See the [following documentation](https://docs.mongodb.com/manual/core/security-encryption-at-rest/).
- Regarding container specification: see below.

## REST API Specification (WP4)

Speckle already has an api contract written in OpenApi v3, coupled with generated documentation of the endpoints. 
- CRUD definitions: 
- Objects can be found [here](https://speckleworks.github.io/SpeckleSpecs/#speckle-objects)
- Streams (object collections) can be found [here](https://speckleworks.github.io/SpeckleSpecs/#speckle-streams)
  
Delta updates and diffing is currently managed at the client level and operate on the object collection rather than individual objects. These can be enhanced to contain more granular update notifications about single/multiple delta updates.

Nevertheless, it is important to note that various client (CAD software) implementations will require different types of diffing to occur based, hence we recommend not implementing these behaviours server-side beyond a common-sense approach. 


## Server Architecture (WP5)

Publisher-subscriber design:
- The speckle server exposes both the REST API mentioned above, as well as a WS server
- All clients connect as either publishers or subscribers to the WS server on a resource-based classification, given permission access levels.
- The server propagates messages to all subscribers, but is also able to handle individual client-to-client messages. 

Distributed architecture: The speckle server is stateless - all api calls are authenticated and WS messages are coordinated across all server instances.

### Other notes: 

#### Data Schemas/Standards

- Speckle is [schema agnostic](https://speckle.works/log/schemasandstandards/), supporting out of the box several object models developed internally by various companies.
- Furthermore, there is a planned integration with the [BHoM](http://bhom.xyz) object model.

### Links: 
- [Speckle Server](https://github.com/speckleworks/SpeckleServer)
- [Speckle Api Specifications](https://speckleworks.github.io/SpeckleSpecs/#speckle)
- Speckle Client Integrations:
  - [Rhino & Grasshopper](https://github.com/speckleworks/SpeckleRhino)
  - [Dynamo](https://github.com/speckleworks/SpeckleDynamo)
  - Revit, Unity, GSA, and others are WIP 




# Test Cases Specification 

As identified in the User Story definitions above, the AECDelta specifications will need to be designed to be compatible, support or facilitate a wide range of data sets, model formats and types of information. 

As a high level specification - data types ranging from schema-less raw data, through well defined proprietary formats, to API exposed object models - are required.

Depending on the data type. The scale at which a meaningful Delta/Change can, or is appropriate to, be detected will vary. For instance 
- Unstructured - file or entire stream level deltas may be possible
- Structured - object level deltas may be possible


List of potential data sets and test cases:

1. Raw data, e.g. csv, .txt , unstructured primative data
1. Custom schemas, JSON
1. Geometry formats 
1. IFC
1. Analysis model representations (e.g. Structural FEM, CFD model, Thermal Environmental models)
1. Multidisciplinary BIM models - building scale
1. Multidisciplinary GIS models - infrastructure scale
1. Fabrication model representation


A deliberate (and possible separate) consideration will need to be made for __"large"__ data sets. Test sets to consider:


I. Point Cloud from laser scan  
II. Bulk simulation results
