# GridView

- [DataColumn](gridview-actioncolumn.md)

## Options

### `tableOptions`

默认值：`table table-striped table-bordered`.

### `filterModel`: `null` 表示禁用

如果仅需要显示一个表格信息，不需要搜索功能（禁用 filter），只需将 `filterModel` 值设置为 `null` 即可。

如果需要**禁用单个列的 filter 功能**，仅需在 DataColumn 配置内将 `filter` 值设置为 `false` 即可。注意这两处禁用的值不同，前者是 null, 后者是 false. `filter` 的默认值是 `null`, 表示filter 区域显示的是文本框。

## yii\grid\DataColumn

- `value` (string | closure). 其中 closure signature 中参数的名称及顺序最不容易记住：**`function ($model, $key, $index, $column)`**

## yii\grid\ActionColumn

### Options

- `contentOptions`: `<td>` 样式;
- `headerOptions`: 表头样式；
- `footerOptions`: footer 样式；

#### `filter`: `false` 表示禁用

该属性如果：

- 没有设置（或值是`null`），显示的是文本框；
- 数组，下拉菜单；
- false, 禁用
