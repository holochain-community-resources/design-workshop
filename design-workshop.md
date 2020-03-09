summary: How to design a Holochain Application
id: design-workshop
categories: holochain design
tags: holochain design architecture workshop
status: Published 
authors: Guillem Córdoba
Feedback Link: https://github.com/holochain-community-resources/design-workshop

# How to design a Holochain Application
<!-- ------------------------ -->
## Overview 
Duration: 2

> This workshop is a work in progress. [Contributions](https://github.com/holochain-community-resources/design-workshop/issues) are greatly appreciated!

### Goal

**Have an idea for a holochain app?**

Follow along this codelab to get a **first draft of a design** for your hApp.

### What You’ll Learn 

You'll learn to:

- **Use common patterns** to build hApps
- **Architect DHTs** to match your functional requirements
- **Define membranes** around DHT shared spaces
- Break down architectural components into **modular zomes**
- **Design entries and links** for every zome
- Design **complex information flows** across shared spaces

### Requirements

It is highly recommended to do this workshop after:

* Having read/understood the holochain **[Core Concepts](https://developer.holochain.org/docs/concepts/)**.
* Having **built a simple Hello-World happ** or similar, either following the [tutorial](https://developer.holochain.org/docs/tutorials/coreconcepts/hello_holo/) or by having attended one of the hAppathons around the world. 

This way you will already know the basic primitives and building blocks available to us.

<!-- ------------------------ -->
## DHT Architecture
Duration: 30

### Inputs
* User stories
* Functional requirements

### Output: 
* Architectural high-level DHTs components with dependencies and functions

### Concepts

Holochain's crucial aspect regarding DHT architecture is the fact that **every happ creates its own network or DHT**. Those networks can be public, private, permissioned, or impose certain conditions for an agent to be able to enter.

This makes architecturing your **network topology** a big first step when designing a happ.

Any holochain application system can be described as a set of high-level DHT components. For purposes of this workshop, **you can think about DHTs as a shared space**, in which all agents can see every piece of data published in it.

### Questions to ask

* Which agents and roles exist in your happ?
* What information will each of these agents publish into the system?
* Which other agents should be able to access that information?

### Rules of thumb

* Define a new DHT shared space every time you have a group of agents that should be able to read some specific information, but not everyone should be able to see it.
* Define a new DHT every time you need to propagate information from one agent to another, but they are not on any shared space.
* Define a new DHT if you need special nodes to store a subset of the information (e.g. indexing nodes, file storage, etc.).

These rules don't cover all the cases, but can guide you towards the appropriate solution.

### Example 

TBD

<!-- ------------------------ -->
## Define Membranes
Duration: 15

### Input
* DHT components of your system

### Output
* Membranes definitions, a.k.a. validation rules for the agents in each shared space
* Processes by which an agent enters the shared space

### Concepts

TBD

### Questions

TBD

### Example

TBD

<!-- ------------------------ -->
## Design Modular Zomes
Duration: 25

### Input
* DHT components of your system
* User stories
* Processes by which an agent enters each shared space

### Output
* Breakdown of each high-level component into modular and re-usable zomes

### Concepts

Holochain applications are composed of different modules or *zomes*. Each zome is an aggregation of:

* **Entry types** definitions
* **Link types** definitions between those entries
* **Validation rules** for those entries and links
* **Function calls** exposed to the exterior
* **Validation rules** for any agent trying to access the network
* **Lifecycle callbacks** like `init`, `receive_direct_message`...

Zomes don't actually specify which entries they are going to be storing, but **defining** which type of entries are supported. In a traditional database system, this would be akin to a database schema specification.

Different technical aspects apply to the relationship between multiple zomes:

* All zomes in the same DNA **share both source chain and DHT**. Entries commited with one zome can be accessed by another zome.
* Agent validation is run in all zomes: each agent is only valid if **all zomes** successfully validate the agent.
* You can have the **same zome in different DNAs**: this allows to bridge calls between DNAs which are defined in the same zome.
* Each zome **can require different properties** set in the happ's properties. For example, a "members" zome may require a `valid_members` property while a "mutual-credit" zome may require a `credit_limit` property. All properties should be provided for all zomes to work.
* **Zomes can depend on other zomes**. Zome functionality can be broken down into zomes A and B, where B only will work if A is present as well. This allows A to be used by other DNAs without assuming anything else, but all of B's code can assume that A's entries and validation rules are present. (Note: for now, this **has to be put in the documentation and warned to developers**, since there is no explicit mechanism to express this).

However, by far the most important concept in breaking down functionalities into differents zomes is the **[Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)**, which states that a single module should only be responsible for one autonomous part of the functionality. Here, knowledge and experience from other kinds of systems are very useful and needed.

### Questions

* For any subset of entries and/or links, **do they make sense without one another**? 
    * Yes: try to split them into different zomes.
    * No: put them in the same zome.

* For any subset of entries and/or links, **would they be reusable by themselves in another DNA?**
    * If so, try to split them into their own zome. If other entries depend on these ones, you can make a second zome depend on this first one.

### Example

TBD

<!-- ------------------------ -->
## Design Entries and Links
Duration: 35

### Input
* Breakdown of modular zomes.

### Output
* Entries and links design for all your zomes
* Entry relationship diagrams specifications.

### Steps

1. Watch [this intro to entries and links design](https://www.youtube.com/watch?v=JytLB6hD-GQ&t=667s). 

2. Build an entry relationship diagram for each one of your zomes with [this guide](https://hackmd.io/XNPC4NUKRd2zja9Itja-fg).

### Concepts

In holochain, **all that an agent can do to modify state is create entries**. It does so by appending the new entry and its header to their own source chain, and advancing the top of that source chain to the latest header. BUT: that entry might be of [different types](https://docs.rs/hdk/0.0.44-alpha3/hdk/prelude/enum.Entry.html): `App`, `LinkAdd`, `CapTokenGrant`... 

After that, it proceeds to calculate which **DHT Operation Transforms** should every one of those entries produce, and **where should those transforms happen in the DHT**. 

For example: creating a normal entry produces at least 3 DHT Operations: it publishes the entry in the entry's hash location, it publishes the entry's header in the header's hash location, and publishes an internal system link from at the agent's address to point to the new header's hash.

This gives us two very different sets of data storage and retrieval systems:

#### Source chain

Local state relative to every agent. Here **local order of events is automatically guaranteed** by the chain of headers and can be scanned in validation rules, when countersigning, etc. 

#### DHT

**Global shared state**, which arrives to its final state through **eventual consistency**. Only from the built-in mechanisms, **we cannot assume that we are seeing all the actions from all the agents in the network** (there are also extreme edge cases as well, like network partitions).

BUT, we **can be sure that what we already see happened and is valid**. All entries accessible from the DHT are already validated by the neighborhood they live in, so we can assume that they are valid. Also, we can be sure that the **content of an entry won't change**, although **its metadata CAN change** (whether it is updated or deleted, or which links it has attached).

Lastly, holochain's DHT works like a big [CRDT](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type). That means that global order of events cannot influence the final state of the data. This means that, for example, if Alice creates entry A, then Bob removes entry A, and then Alice creates again entry A, **entry A will stay removed forever**. Holochain internals mechanisms don't have any way of discerning in which global order those events happened (*does global time exist?*) so it will solve the dispute by giving the remove entry order priority over all the other ones. 

#### Validation rules

All these mechanisms and primitives influences our validation rule design, and we have to keep them in mind at all times not to write bad validation rules. **Validation rules must be deterministic**: for a given entry or link, all validations any agent might run at any point in time **must** result in the same execution.

This gives us some constraints when designing validation rules:

* **Can't change throughout time**: as time passes, our rules should not change result. 

> Example: when validating if an agent has some role assigned, we must check if that agent **had that role assigned when they committed the entry**, not at current point in time.

* **Can't depend on changing metadata**: this will give different results for different perspectives and fail to be consistent.

> Example: I write a validation rule that checks that the number of links from a given entry is less than 2. This will be true at some point in time and for some perspective, but as links are added to the entry, the validation rule will change its result.

* **Must validate all roles implicated in an entry at the same time**, no matter the "perspective" that we are validating.

> Example: if we are validating a countersigned transaction, in which both Alice and Bob sign and commit the transaction separately. No matter "whose commit" we are validating, the validation should give the same result from Alice's point of view as well as Bob's point on view.

* BUT, **they can depend on another entry being present in the DHT**, by an internal mechanism that will retry getting an entry from the DHT if the device is disconnected or the entry has not been published yet.

> Example: if I'm validating a transaction in a mutual credit application, I can make that transaction be valid only if both parties have the appropriate headers published on the DHT. If the validation rules fail to find the headers at some execution, they will pause and try later.

### Questions

TBD

### Example

TBD

<!-- ------------------------ -->
## Complex Information Flows
Duration: 20

### Inputs
* Architecture DHT components and their membranes.
* Necessary non-trivial information flows 

### Output
* Sequence diagrams for all components involved in the system for each complex information flow.

By *complex information flow* we mean any piece of information or protocol that goes beyond a simple `get_entry` or `get_links`, and involve multiple parties or components.

Examples include:
* Courtersigning a transaction
* Retrieving private data from another DHT shared space
* Using capabilities to execute a function of another agent

### Concepts

TBD

### Questions

TBD

### Example

* [Mutual-credit](https://github.com/holochain-open-dev/mutual-credit)

TBD