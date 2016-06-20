# Route Object & Route Matching

Vue-router 能匹配含有 dynamic segments, star segments 和 query string 的路径。

一个 route object 有如下内置的属性：

数据类型    | 名称        | 说明
------------|-------------|-----
string | `$route.path` | 当前路由，永远解析成绝对路径
object | `$route.params` | contains key/value pairs of dynamic segments and star segments
object | `$route.query` | 键值对儿，假设路径 `/foo?id=1`, 可得 `$route.query.id == 1`
object | `$route.router` | 管理当前路由的 router 实例。


