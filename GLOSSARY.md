Vue.js
===========================

## directive

指令符。以 `v-` 开头的一类特殊的属性。例如 `<a v-link="/"></a>` 中的 `v-link`.

作用： _to reactively apply special behavior to the DOM when the value of its expression changes_

各种工具下的指令符举例：

- MySQL: 在 option file 内，可以使用 `!include` 指令符来包含其它 option files;
- PHP: `php.ini` 中的各种设置都叫指令符，例如 `error_reporting`;

See also: directive modifier

## directive argument

指令符参数。紧跟指令符，以 `:` 开头，比如 `v-bind:href="url"` 中的 `:href`.

你可能注意到，`v-bind:href="url"` 等价于 `href="{{url}}"`. 实际上，在内部，所有 attribute interpolation 都会转化成 `v-bind`.

## directive modifier

修饰符。以 `.` 开头的特殊后缀。表示指令符绑定的方式。例如，修饰符 `.literal` 靠素指令符以 literal string 而不是表达式来解析它的属性值：

```vue
<a v-bind:href.literal="/a/b/c"></a>
```

## Async Components

异步组件。

Ref. http://vuejs.org/guide/components.html#Async-Components

Vue-Router
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

## named route

具名路由。

相关章节：[Named Routes](/meet/vue/router/named-route.md)

## hash mode

如果 url 中含有 `#!` (hashbang), 即为 hash mode. 例如： `http://a.com/#!/user`.

See also: history mode.

## history mode

url 中不带 hashbang `#!`, 想普通的 URL 那样。

## hashbang

即 url 中的 `#!` 符号。

hash 表示井号， bang 表示感叹号。


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
