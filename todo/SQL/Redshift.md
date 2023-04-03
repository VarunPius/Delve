# Sort Keys:
Redshift Sort Key determines the order in which rows in a table are stored.

Query performance is improved when Sort keys are properly used as it enables the query optimizer to read fewer chunks of data filtering out the majority of it.
There can be multiple columns defined as Sort Keys.

Redshift supports two kinds of Sort Keys:
- **Compound Sort Keys**
- **Interleaved Sort Keys**

### Compound Sort Key
These are made up of all the columns that are listed in the Redshift sort keys definition during the creation of the table, in the order that they are listed. 
Therefore, it is advisable to put the most frequently used column at the first in the list. 
**COMPOUND** is the default sort type.

Compound sort keys might speed up `JOIN`s, `GROUP BY` and `ORDER BY` operations, and Window functions that use `PARTITION BY`.

### Interleaved Sort Key
Interleaved sort gives equal weight to each column in the Redshifts sort keys.
As a result, it can significantly improve query performance where the query uses restrictive predicates (equality operator in `WHERE` clause) on secondary sort columns.

## Selecting the right kind needs the knowledge of the queries.
- Use **Interleaved Sort Key** when you plan to use:
    - one column as Sort Key, or
    - when `WHERE` clauses in your query have highly selective restrictive predicates, or
    - if the tables are huge.
- Use **Compound Sort Key**, when you have more that one column as Sort Key when your query includes `JOIN`s, `GROUP BY`, `ORDER BY` and `PARTITION BY` or when your table size is small.
- Don’t use an interleaved sort key on columns with monotonically increasing attributes, like an identity column, dates or timestamps. (Explanation next)

## Vacuum:
A computer disc consists of thousands of microscopic magnets looping around the center.
Think of it as a bunch of domino tiles laying down next to each other in a bunch of concentric circles.
A tile can either be face up or face down: 1 or 0. Each tile is a magnet that can either be on or off.
When data gets stored/updated/deleted in a database, a bolt of electricity will beam its way through the magnets and flip them on or off.

When you delete a record from a database, the magnets representing that data will be "switched off".
The kicker, and why the `VACUUM` command is so vital, is that those magnets that were just switched off aren't automatically going to be used the next time you write something to the database.
This is because the database keeps a pointer of its "next magnet to flip", and that pointer is always positioned at the last, fresh magnet to get flipped over.

What `VACUUM` does, at a microscopic level, is:
- Identify all of those tiles which were "switched off" due to UPDATE or DELETE commands
- Reclaim that space by moving other data into it, thus consolidating all of the data. 
- Move the pointer back to the next available magnet to flip.

To keep fully benefit of Interleaved sorting, you need to periodically execute `VACUUM REINDEX` to the tables. 
The total time of `VACUUM REINDEX` will be longer than that of `VACUUM FULL` because a `VACUUM REINDEX` operation requires an extra analysis of the distribution of the values in interleaved columns before performing a `VACUUM FULL` operation.
If a `VACUUM REINDEX`operation can't be included in a daily batch operation, you shouldn't use an interleaved sort key on columns with monotonically increasing attributes, such as id, dates, or timestamps in order to prevent its distribution from getting larger.

For your information, you can refer to the indicator `interleaved_skew` in the view `SVV_INTERLEAVED_COLUMNS` to find which table should be run `VACUUM REINDEX` on. If the `interleaved_skew` is greater than 1.4, a `VACUUM REINDEX` will usually improve performance.

//TODO: https://chartio.com/blog/interleaved-sort-keys-part-2/
//TODO: more info on svv_table_info: https://www.youtube.com/watch?v=mfV4GMBDQVg&list=PLjxmnUoe6snT60oKKtn8t5d1R_W_iDRZh&index=23

# Distkey
Redshift Distribution Keys determine where data is stored in Redshift. 
Clusters store data fundamentally across the compute nodes. Query performance suffers when a large amount of data is stored on a single node.
The query optimizer distributes less number of rows to the compute nodes to perform joins and aggregation on query execution.
This redistribution of data can include the shuffling of the entire tables across all the nodes.

Uneven distribution of data across computing nodes leads to the skewness of the work a node has to do and you don’t want an under-utilized compute node.
So the distribution of the data should be uniform. Distribution is per table.
So you can select a different distribution style for each of the tables you are going to have in your database.

## Types of Distribution Styles
Amazon Redshift supports three kinds of table distribution styles.
- **Even distribution**: This is the default distribution style of a table. In Even Distribution the Leader node of the cluster distributes the data of a table evenly across all slices, using a round-robin approach.
- **Key distributio**n: The data is distributed across slices by the leader node matching the values of a designated column. So all the entries with the same value in the column end up in the same slice.
- **All distribution**: The leader node maintains a copy of the table on all the computing nodes resulting in more space utilization. Since all the nodes have a local copy of the data, the query does not require copying data across the network. This results in faster query operations. The negative side of using ALL is that a copy of the table is on every node in the cluster. This takes up too much space and increases the time taken by Copy command to upload data into Redshift.

## Choosing the right Distribution Styles
The motive in selecting a table distribution style is to minimize the impact of the redistribution by relocating the data where it was prior to the query execution.
Choosing the right KEY is not as straightforward as it may seem. In fact, setting wrong `DISTKEY` can even worsen the query performance.

Choose columns used in the query that leads to the least skewness as the DISTKEY.
The good choice is the column with maximum distinct values, such as the timestamp.
Avoid columns with few distinct values, such as months of the year, payment card types.

- If the table(e.g. fact table) is highly de-normalized and no JOIN is required, choose the `EVEN` style.
- Choose `ALL` styles for small tables that do not often change. For example, a table containing telephone ISD codes against the country name.
- It is beneficial to select a `KEY` distribution if a table is used in JOINS. Also, consider the other joining tables and their distribution style.

If one particular node contains the skew data, the processing on this node will be slower.
This results in much longer total query processing time.
This query under skewed configuration may take even longer than the query made against the table without a DISTKEY


//TODO: vacuum