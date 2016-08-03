插入表格代码如下：

```
<table class="table table-bordered table-striped table-condensed">
    <tr>
        <td>北京</td>
	<td>雾霾</td>
    </tr>
    <tr>
        <td>深圳</td>
	<td>暴雨</td>
    </tr>
</table>
```

发现table加了个class属性，如果只是table标签 将不起作用。

table-bordered：带圆角边框和竖线

table-striped：奇偶行颜色不同

table-condensed：压缩行距

&#160;

除了以上另外还有其他可供选择：

1、如果需要表头跟内容不一样，可以将<td>表头内容</td>换成<th>表头内容</th>。

2、如果表格内文需要换行，可以在要换行的内容后加入<br>，后面的内容就会跑到下一行。

3、如果内文中有代码，需要特别显示，可使用：<code>代码</code>。

4、如果表格中有需要设为斜体的内容，可使用：<I>要设为斜体的内容</I>。

5、如果有跨行或者跨列的单元格，可用<th colspan="跨列数">内容</th>或rowspan。

6、如果要调整某一列的宽度，可使用：<th width="宽度值或百分比">表头内容</th>。

&#160;

最后效果如下：

<table class="table table-bordered table-striped table-condensed">
	<tr>
		<td>北京</td>
		<td>雾霾</td>
	</tr>
	<tr>
		<td>深圳</td>
		<td>暴雨</td>
	</tr>
</table>