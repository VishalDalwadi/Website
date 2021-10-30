---
layout: plain
title: Data Partitioning
---

Data partitioning generally enables a system to process data concurrently and to some extent even in parallel. Considering that access to data can be 
efficiently load balanced and therefore enhances the throughput per query.
Data partitioning is a widely used concept among DBMS, to split data in chunks or fragments of relevance
and therefore achieve efficient access and parallel processing. 

In general data partitioning in Polypheny can be distinguished between two variations:

* [Vertical Partitioning](#vertical-partitioning)
* [Horizontal Partitioning](#horizontal-partitioning)

## Vertical Partitioning
In Polypheny-DB the vertical partitioning is inherently driven by the polystore design.
This enables users not only to replicate entire tables (*Full Placement*) but to place only a subset of columns onto the different stores harvesting the engines specific benefits to provide the best possible performance.
These are called *Column Placements*.

To correctly identify any object across the system it is necessary that the primary key of a table is always present on every placement.



## Horizontal Partitioning
Refers to the partitioning of objects like tables into a disjoint set of rows that can be stored and accessed separetely.
To support this explicit form of partitioning there exist several partition [algorithms](#existing-horizontal-partition-algorithms).
These algorithms can be applied to any table based on an arbitary column which results in a fragmentation of the table 
based on the data values of the selected column. 
These are called *Partition Placements* and can also be utilized to place specific partitions onto stores.

### Existing Horizontal Partition Algorithms

Currently Polypheny supports the following partition functions:
* **List** - the table is partitioned by explicitly assigning values to each partition. These placements are the result of the extended vertical partitoning of a table.
* **Hash** - the table is partitioned by specifying a modulus and a remainder for each partition. Each partition will hold the rows for which the hash value of the partition key divided by the specified modulus will produce the specified remainder.
* **Range** - the table is partitioned into numerical "ranges" defined by a key column, with no overlap between  the ranges of values assigned to different partitions. 
* **Temperature-aware Partitioning** - Is somewhat special since it serves as an extension to the classical partition functions.
It places data based on their "temperature" in either a *HOT*- or *COLD*-area. This classification depends on certain cost metrics like the access frequency to decide
the current.


## Vertical + Horizontal Partitioning

With this kind of distribution and combination of the data partitioning concepts, Polypheny-DB is able to selectiviely replicate data for specific use cases
Providing support for a wide variety of application requirements. Such as read-performance enhancements, availability and even security related zoning of data fragments. 

For example lets consider a sales table which is partitioned (horizontal) by every sales region: DE,US,...,UK,CH. Adminstrators could now choose to place the 
two columns (vertical) *net_sales* and *region* with all data that is associated with the partitioned identified by US.
Allowing to construct a store which only really contains the data on net sales in the US region. Which can be used to optimize use-case specific scenarios.

Finally users can not only choose to replicate the data to speedup individual queries but to distribute the data fragements entirely. For either case Polypheny-DB is able to retrieve all relevant data no matter its distribution.


### Current Limitations

* Currently every Data Placement (Table on a Store) does not allow an arbitrary number of partitions per vertically placed partitions columns on that store.
This means that all columns placed on one specific store will all hold data belonging to the the assigned partitions, reducing the overall complexity of the system.

* At the moment only HASH-Partitioning is supported as the internal partiton function for *Temperature-aware Partitioning*.

### Roadmap
* *Temperature-aware Partitioning*:
  * Add Range- and List-Partitioning to the suported internal partition functions.
  * Add a *WARM*-area to fulfil more specialized use cases.




