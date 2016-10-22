# MySQL

## 数据库设计要点

- 外键约束。对重要的外键，应强制添加 `ON DELETE RESTRICT` 限制。同时通过程序实现软删除功能。
