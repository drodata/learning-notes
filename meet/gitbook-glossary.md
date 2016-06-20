# GitBook Glossary

新建 `GLOSSARY.md` 文件。每个词汇对应一个 `h2` 标题，下面带上描述段落，例如：

```
## DDL

Data Definition Laguage. 操作数据库本身、而不是操作记录行的语句。例如：ALTER TABLE.

等等。
```

注意事项：

1. glossary **不区分大小写**。拿上面的 'DDL' 为例，不管正文中出现的是 'DDL' 还是 'ddl', 都会自动生成一个链接，指向该定义.
2. 不支持部分匹配。例子：假设我定义了一个名为 "dynamic segment" 的词汇，那么，在正文中出现 "dynamic segments" 是不会被替换成链接的，只有 "dynamic segment" 才会被替换
