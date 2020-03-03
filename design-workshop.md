summary: How to design a Holochain Application
id: design-workshop
categories: holochain design
tags: holochain design architecture workshop
status: Published 
authors: Guillem Córdoba
Feedback Link: https://github.com/holochain-community-resources/design-workshop

# How to design a Holochain Applicaion
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
- Design **complex information flows** across shared spaces
- Break down architectural components into **modular zomes**
- **Design entries and links** for every zome

### Requirements

It is highly recommended to do this workshop after:

* Having read/understood the holochain [Core Concepts](https://developer.holochain.org/docs/concepts/).
* Having built a simple Hello-World happ, either following the [tutorial](https://developer.holochain.org/docs/tutorials/coreconcepts/hello_holo/) or by having attended one of the hAppathons around the world. 

This way you will already know the basic primitives and building blocks available to us.

<!-- ------------------------ -->
## Architect DHTs
Duration: 30

* **Input**: user stories and functional requirements.

* **Output**: architectural high-level DHTs components with dependencies and functions. Together they will form your system.

TBD

<!-- ------------------------ -->
## Define membranes
Duration: 15

* **Input**: DHT components of your system.

* **Output**: membranes definitions, a.k.a. validation rules for the agents in each shared space, or process by which an agent enters the shared space.

TBD

<!-- ------------------------ -->
## Design modular zomes
Duration: 15

* **Input**: DHT components of your system.

* **Output**: breakdown of each high-level component into modular and re-usable zomes.

TBD

<!-- ------------------------ -->
## Design entries and links
Duration: 25

* **Input**: breakdown of modular zomes.

* **Output**: entries and links design for all your zomes, with entry relationship diagrams specifications.

Build an entry relationship diagram for each one of your zomes: https://hackmd.io/XNPC4NUKRd2zja9Itja-fg

TBD

<!-- ------------------------ -->
## Complex information flows
Duration: 20

* **Inputs**: 
  * Architecture DHT components and their membranes.
  * Necessary non-trivial information flows 

* **Output**: sequence diagrams for all components involved in the system for each complex information flow.

By "complex information flow" we mean any piece of information or protocol that goes beyond a simple "get_entry" or "get_links", and involve multiple parties or components.

Examples include:
* Courtersigning a transaction
* Retrieving private data from another DHT shared space
* Using capabilities to execute a function of another agent

TBD