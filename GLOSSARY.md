Vur-Router
===========================

## Dynamic Segments

在 path segments 前加上`:` 就是 dynamic segments.

例子：以路径 `/user/username` 来说， `user` 和 `username` 都叫做 path segment. `/user/:username` 中的 `:username` 就是一个 dynamic segment.

Vue-router 能够识别 dynamic segments. 例如 `/order/view/:id` 可以匹配到 `/order/view/3` 和 `/order/view/6`.

## Star Segments

在 path segments 前加上`*` 就是 star segments.

Dynamic segment 只能对应路径中的**一个片段**。而 dynamic segments 是 dynamic segments 的“贪婪模式”，可以匹配任意多个 path segments. 例如：`/foo/*bar` 可以匹配到下列任意一个路径：

- `foo/bar`
- `foo/har`
- `foo/goo/bar`
- `foo/goo/abc/bar`

## Route Context Object

## Route Object

Route context object 的简称。

> The route object will be injected into every component in a vue-router-enabled app as `this.$route`, and will be updated whenever a route transition is performed.

在 Template 内，可以直接使用 `$route` 获取此变量。 例如：

```vue
<p>Current route path: {{$route.path}}</p>
<p>Current route params: {{$route.params | json}}</p>
```

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
