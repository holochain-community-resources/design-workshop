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
* Functional requirements.

### Output: 
* Architectural high-level DHTs components with dependencies and functions.

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
* DHT components of your system.

### Output
* Membranes definitions, a.k.a. validation rules for the agents in each shared space
* Processes by which an agent enters the shared space.

### Concepts

TBD

### Questions

TBD

### Example

TBD

<!-- ------------------------ -->
## Design Modular Zomes
Duration: 15

### Input
* DHT components of your system.
* Processes by which an agent enters each shared space.

### Output
* Breakdown of each high-level component into modular and re-usable zomes.

### Concepts

TBD

### Questions

TBD

### Example

TBD

<!-- ------------------------ -->
## Design entries and links
Duration: 25

### Input
* Breakdown of modular zomes.

### Output
* Entries and links design for all your zomes
* Entry relationship diagrams specifications.

Build an entry relationship diagram for each one of your zomes: https://hackmd.io/XNPC4NUKRd2zja9Itja-fg

### Concepts

TBD

### Questions

TBD

### Example

TBD

<!-- ------------------------ -->
## Complex information flows
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

TBD