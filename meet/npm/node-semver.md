
## Advanced Range Syntax

### Caret Ranges `^1.2.3` `^0.2.5` `^0.0.4`

Allows changes that do not modify the left-most non-zero digit in the `[major, minor, patch]` tuple.

含义：

- `^1.2.3`: `>=1.2.3 <2.0.0`
- `^0.2.3`: `>=0.2.3 <0.3.0`
- `^0.0.3`: `>=0.0.3 <0.0.4`

https://github.com/npm/node-semver#caret-ranges-123-025-004

参考：

- The semantic versioner for npm: https://github.com/npm/node-semver
