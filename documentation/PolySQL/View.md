---
layout: plain
title:  View
description: >
How to use views in Polypheny.
hide_description: true
---

This page gives an overview of views and materialized views in Polypheny. 
It shows the possibilities views offer and how they can be used.

##Views

{% highlight sql %}
createViewStatement:
    CREATE VIEW [ OR REPLACE ] name
    [ '(' tableElement [, tableElement ]* ')' ]
    [ AS query ]

dropViewStatement:
    DROP VIEW [ IF EXISTS ] name

alterStatement:
    ALTER VIEW [ schemaName . ] tableName RENAME TO newTableName
    | ALTER VIEW [ schemaName . ] tableName RENAME COLUMN columnName TO newColumnName
{% endhighlight %}

* `CREATE VIEW` creates a view of a query. views have no data placements on a store but are references to an underlying Table.
* `CREATE OR REPLACE` the only difference to the previous option is that if a view with this name already exists, it is replaced. 
  That means the old view is dropped and a new view will be created.
* `DROP VIEW` deletes an existing view.
* `DROP VIEW IF EXISTS` deletes an existing view but checks first if the view to drop is present.
* `ALTER VIEW` changes the properties of an existing view. It is possible to alter two different things on an existing view. 
  For one, it is possible to `RENAME TO` a different view name and second it is possible to `RENAME COLUMN` in order to change the column name.
  
###Functionality within Polypheny
A view is a logical table that shows data from one or more underlying tables, which can be chosen through a `SELECT` statement. 
Views do not store the data physically but only link to the underlying tables. 
In Polypheny views are read-only and if a table is linked with a view it is not possible to change that table.

##Materialized Views

{% highlight sql %}
createMaterializedViewStatement:
    CREATE MATERIALIZED VIEW [ IF NOT EXISTS ] name
    [ '(' tableElement [, tableElement ]* ')' ]
    [ AS query ]
    [ <ON> <STORE> storeName ( <COMMA> storeName )*]
    [FRESHNESS (INTERVAL interval timeUnit | UPDATE interval | MANUAL )  ]

dropMaterializedViewStatement:
    DROP MATERIALIZED VIEW [ IF EXISTS ] name

alterStatement:
    ALTER MATERIALIZED VIEW [ schemaName . ] tableName RENAME TO newTableName
    | ALTER MATERIALIZED VIEW [ schemaName . ] tableName RENAME COLUMN columnName TO newColumnName
    | ALTER MATERIALIZED VIEW [ schemaName . ] tableName FRESHNESS MANUAL
{% endhighlight %}

* `CREATE MATERIALIZED VIEW` creates a materialized view of a query.
* `ON STORE` one or many stores can be selected. The data from the selected query is placed on the chosen Store.
* `FRESHNESS` describes how often and with which method the materialized view should be refreshed.
* `FRESHNESS INTERVAL` allows specifying after what time the materialized should be updated. 
  For Example `FRESHNESS INTERVAL 10 "hours"` this refreshes the materialized view every 10 hours.
* `FRESHNESS UPDATE` allows specifying after how many updates on the underlying table the materialized view is updated. 
  For Example `FRESHNESS UPDATE 2` after two updates on the underlying Table the materialized view is automatically updated.
* `FRESHNESS MANUAL` with this type of freshness the materialized view is not automatically updated. 
  Every update of the data needs to be triggered manually. It is triggered with the following command: 
  `ALTER MATERIALIZED VIEW name FRESHNESS MANUAL`
* `DROP MATERIALIZED VIEW` deletes an existing materialized view.
* `DROP MATERIALIZED VIEW IF EXISTS` deletes an existing materialized view but checks first if the view to drop is present.
* `ALTER MATERIALIZED VIEW` changes the properties of an existing view. It is possible to alter two different things on an existing view. 
  For one it is possible to `RENAME TO` a different view name and second it is possible to `RENAME COLUMN` in order to change one column name.

###Functionality within Polypheny
Materialized view have a data placement on a selected store. This brings a speed advantage compared to a regular view.
The materialized views in Polypheny offer three different freshness options, this allows to choose a suitable option for each use case. 
Materialized views are read-only and it is not possible to change the underlying table as soon a materialized view is dependent from it.
