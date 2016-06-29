
## Advanced Range Syntax

版本号的三个数字代表的含义为 `[major, minor, patch]`.

### X-Ranges `1.2.x` `1.X` `1.2.*` `*`

`X`, `x` 或 `*` 都可以用来表示任意数字。例子：

- `*`: 任意版本号：`>=0.0.0`
- `1.x`: 相当于 `>=1.0.0 < 2.0.0`
- `1.2.x`: 相当于 `>=1.2.0 < 1.3.0`

A partial version range 被视为一个 X-Range, 因此，这些特殊字符（指 `x`, `X`, `*`）其实是可选的：

- `""` 空字符：等价于 `*`
- `1`: 等价于 `1.x`
- `1.2`: 等价于 `1.2.x`

### Tilde Ranges `~1.2.3` `~1.2` `~1`

> 安装 AdminLTE package 时遇到 `~` 范围符号：
> 
>     composer require "almasaeed2010/adminlte=~2.0"
> 

如果指定 minor version, 允许 patch-level 的变动，若没有，则允许 minor-level 的变动。下面是一些例子：

- `~1.2.3`: 相当于 `>=1.2.3 < 1.3.0`
- `~1.2`: 相当于 `>=1.2.0 < 1.3.0`, 和 `1.2.x` 等价

### Caret Ranges `^1.2.3` `^0.2.5` `^0.0.4`

Allows changes that do not modify the left-most non-zero digit in the `[major, minor, patch]` tuple.

含义：

- `^1.2.3`: `>=1.2.3 <2.0.0`
- `^0.2.3`: `>=0.2.3 <0.3.0`
- `^0.0.3`: `>=0.0.3 <0.0.4`

https://github.com/npm/node-semver#caret-ranges-123-025-004

参考：

- The semantic versioner for npm: https://github.com/npm/node-semver
