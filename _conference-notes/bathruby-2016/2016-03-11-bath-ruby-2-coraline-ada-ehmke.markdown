---
layout: post
title: "Bath Ruby 2 - Coraline Ada Ehmke: How Neo4j saved my relationship"
date: 2016-03-11T11:15:06+00:00
comments: true
category: liveblog
tags: [neo4j, database, bathruby]
---
* "people should not be shits to each other online"
* Relational databases are designed for backing _forms_
  * Not always the best way to store data
* Graph theory - originated with Euler
* Data suited to a graph db:
  * Where context is important - needed to understand the data


* Nodes and Edges
  * Edges are relationships between nodes
  * relationships are first class citizens (unlike join tables)

![@CoralineAda talking about neo4j](https://pbs.twimg.com/media/CdQ46yqW0AAOo8k.jpg)

* Graph dbs have Much better performance: linear, constant (RDBs performance
degrade worst)
* Good for agile because they don't lock you in to a particular data
structure.
* Relations + Metadata = facts - extract facts and model as nodes instead of
edges
* The graph db doesn't care what is stored in a node: things don't have to be
in any particular structure.
* Neo4j
  * different stores: Node, property, relation
  * Fixed-size records for Node and relationship data -
  this enables superfast lookups because
  you know where a record will exist in memory.
* Query language - Cypher
  * AScii art to represent relationships! :D `(a)-[:follows]->(b)`
  * When deleting nodes, you have to detach from any existing relationships
  pointing at that node
* Neo4j console is a single-page webapp, with shiny interactive represenations
of the graph
* Interaction with ruby: via a gem - build queries in mostly the same way that
you would if writing it directly in plain text. Get a struct back with the data
* neo4j for AI:
  * Sophia - Coraline's AI project
  * aim is to create metaphors
  * models contexts like beauty, animals etc..
  * models synonyms and antonyms
  * models facts about the terms - uses these to answer questions - e.g. "do
  cats have tails?"
* When to use:
  * when relations/metadata are as important as the data itself
  * when data is deeply nested
