---
layout: plain
title: Adapter Development
description: >
Steps necessary to develop a new adapter.
hide_description: true
---

Two grow and a new features to Polypheny there is often the need to develop new adapters.
This following steps should serve as a guideline for needed steps, when adding a new adapter.

## Adapter
As a polystore, Polypheny is able to support multiple different underlying databases.
To allow to interact, which them, specific adapters are needed which translate the Polypheny provided relational algebra to the database specific syntax.

## Store and Sources
Polypheny uses two different kinds of adapters, stores and sources.
Stores have their schema managed by Polypheny, which allows to query data but also to manipulate its structure and content.
Sources on the contrary are only able to query an external database through Polypheny but not to manipulate its schema and content.
Additionally, sources handle external changes to the schema, while stores expect exclusive control by Polypheny.

# Steps

## Find Database Deployment ( Store, Source )
Polypheny supports three types of deployment for adapters. 

- Embedded
- Docker
- Remote

While Remote does not need special setup besides a way to connect via IP and port, it is also less powerful.
This due to the fact that Remote needs setup and management from the administrator/user of Polypheny.
In contrast, Docker and Remote can be configured to directly manage the underlying database through Polypheny directly.

When developing a new adapter, one should try to use an embedded version of the database or if none exists, use a Docker container if available.
This helps a lot during development, as it allows to delete and spin up a new instance fast.

## Add Adapter To Polypheny ( Store, Source )

## Define Rules ( Store, Source )

## Run Integration Pipeline ( Store )
Polypheny comes with a huge testing suite, which can be used during development to assure correct implementation of stores.
To start the suite the following gradle command can be used:
```./gradlew integrationTests -Dstore.default=[New Store Name]```
More information to the testing suite can be found in [testing](Testing.md).
