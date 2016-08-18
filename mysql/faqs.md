# FAQs

## Errors

### ERROR 1005 (HY000): Can't create table <table-name> (errno: 150)

出现此错误的原因主要是外键约束没有满足。常见的情形有：

- 子表的外键数据类型与父表中主键**类型不完全一致**。以订单表 `order` 和货物表 `item` 为例，`order.id` 的类型为 `INT(10)`, 而 `item.order_id` 值类型为 `INT(11)`;
- 新建表格时，**子表先于父表创建**。正确的顺序是先创建父表，再创建子表。

