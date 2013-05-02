## Purpose

Web app that lets you quickly see specific data in your database, and what
other data it connects to, without having to type any SQL queries.

## Setup
* Fill in config file with list of relationships, one per line, like this:
address customer 1
customer order
order line_item

* Foreign key points from right item to left item (left item is the parent)
* When there is a 1:1 relationship, enter a "1" at the end; otherwise a 1:many relationship is implied.

* Specify a "newest in given table" query:

```
select * from t_{{current_name}} order by creation_date desc limit 30
```

* Specify a template upstream relationship query, like this (provided in default file):

```
select {{parent_name}}_id, count(*) as num from t_{{parent}} join t_{{child}} using ({{parent}}_id) group by {{parent}}_id order by num desc limit 30
select * from t_{{parent_name}} where {{parent_name}}_id = {{parent_id}}
```

* Specify a template "current table" query, like this (provided in default file):

```
select * from t_{{current_name}} where {{current_name}}_id = {{current_id}}
```

* Specify a template downstream relationship query, like this (provided in default file):

```
select * from t_{{child_name}} where {{current_name}}_id = {{current_id}} order by creation_date desc limit 15;
```

* Specify a "max downstream cardinality examples" query:

```
# select {{current_name}}_id, count(*) as num from t_{{current_name}} join t_{{child_name}} using ({{current_name}}_id) group by {{current_name}}_id order by num desc limit 30
```

Usage:
* Choose a table.  Table newest rows appears.
* Optionally enter a value, or bounding pair of values, on a given column to filter the results.  The table updates.
* Click one of the rows.  
** The row clicked gets a special "current" highlight.
** Other rows disappear (?)
** All related upstream data (infinite levels) appears above the row
** Some downstream data (finite rows per table), only tables directly related (one level deep).
* Click any row in the result to update the "current" node accordingly, and everything updates.
* Filter any table in the result as desired.  Unrelated data de-highlights.
