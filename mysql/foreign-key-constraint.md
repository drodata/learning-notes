# Foreign Key Constraints

## Parent table and child table

![parent table and child table](http://share.drodata.com/wp-content/uploads/2016/06/parent-child-table.png)

上图中，`class` 表就是父表，`student` 就是子表。

## Syntax

`CREATE TABLE` 和 `ALTER TABLE` 语句均可使用下面的语法定义：


_`index_name`_ 表示一个 foreign key ID. 若子表中已定义外键索引，其值会被忽略。否则，MySQL 将根据如下规则隐形创建外键索引。

- 如果 `CONSTRAINT symbol` 定义过，则使用 `symbol` 值；否则，使用 `FOREIGN KEY index_name` 的值；
- 若两者都未定义，则根据 the name of the referencing foreign key column 自动生成。

使用 MySQL Workbench 创建的 SQL 内容大致如下：

```sql
CREATE TABLE IF NOT EXISTS `mydb`.`class` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;

DROP TABLE IF EXISTS `mydb`.`student` ;

CREATE TABLE IF NOT EXISTS `mydb`.`student` (
  `id` INT NOT NULL,
  `first_name` VARCHAR(45) NULL,
  `last_name` VARCHAR(45) NULL,
  `class_id` INT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_student_1_idx` (`class_id` ASC),
  CONSTRAINT `fk_student_1`
    FOREIGN KEY (`class_id`)
    REFERENCES `mydb`.`class` (`id`)
    ON DELETE CASCADE
    ON UPDATE NO ACTION)
ENGINE = InnoDB;
```
