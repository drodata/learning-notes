MySQL
===========================

## DDL

Data Definition Laguage. 操作**数据库**本身、而不是操作记录行的语句。例如：`ALTER TABLE`.

等等。

## parent table

The table in a _**foreign key**_ relationship that **holds the initial column values** pointed to from the child table.

The consequences of deleting, or updating rows in the parent table depend on the ON UPDATE and ON DELETE clauses in the foreign key definition. Rows with corresponding values in the child table could be automatically deleted or updated in turn, or those columns could be set to NULL, or the operation could be prevented.


## child table


- 子表中含有 `FOREIGN KEY ... REFERENCES` 从句；
- 子表记录创建的前提是，父表对应的记录必须存在；
- 通过设置 `ON UPDATE` 和 `ON DELETE` options, 子表可以对父表的更新、删除操作进行不同的回应，例如跟随更新或删除、拒绝等；
