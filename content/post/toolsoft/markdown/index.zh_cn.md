---
title: 'Markdown语法'
date: 2023-11-17T01:35:19+08:00
lastmod: '2023-11-17'
---

# Markdown语法
## 一、标题
Markdown支持两种标题语法。

第一种：
```
=== 最高阶标题

--- 第二阶标题

```
任何数量的 = 和 - 都有效果，需要注意由于分割线也是“---”，使用分割线时需要空一行，以避免被识别为第二阶标题。

第二种：
```
# 第一阶
## 第二阶
### 第三阶
#### 第四阶
##### 第五阶
###### 第六阶
```
最多支持6阶标题
## 二、字体
Markdown使用“*”和“_”作为强调符，二者作用相同，但以什么符合开始，就要用什么符合结束。
```
*这是斜体*
_这是斜体_
**这是加粗**
__这是斜体__
***这是加粗斜体***
___这是加粗斜体___
~~这是加删除线~~
```
如果 * 和 _ 两边都有空格时，就是普通的 * 和 _ 。
### 三、分割线
在一行中使用三个以上的 * - _ 建立分割线，行中不能有其他字符。
```
***
- - - 
__________
``` 
## 四、引用
引用文字前加 > 。Markdown支持每行前加 > 和仅段前加 > 。支持嵌套引用，只需要根据嵌套层级加不同数量的 > 。
## 五、列表
无序列表使用 * - + 都可，符号右边加空格。

有序列表使用数字加英文句号 . ,列表的数字代表序号，从第一行的数字开始自增，后续列表的数字不影响显示。

列表支持嵌套，上一级和下一级之间用三个空格的缩进来控制。

## 六、表格

格式如下：
```
| 默认居左 | 居中表头 | 居左表头 | 居右表头 |
| ------- | :-----: | :-- | -------: |
| 第一列  | 第二列 | 第三列 | 第四列 |
```
显示效果如下：
| 默认居中 | 居中表头 | 居左表头 | 居右表头 |
| ------- | :-----: | :-- | -------: |
| 第一列  | 第二列 | 第三列 | 第四列 |

第一行声明表头

第二行分割表头和内容，并用来声明该列表格中文字是否居中，有一个 - 即可有效，文字默认居左，在 - 两边加 : 表示居中，在 - 右侧加 : 表示居右

后面的行填写内容。
## 七、代码
Markdown有两种加入代码块的方式

第一种只需要缩进4个空格或1个制表符即可

更推荐第二种：单行代码用反引号 ` 包裹，多行代码用 ``` 包裹。

`这是单行代码`
```
这是多行代码
```
## 八、换行和段落
当某行为空，或仅有空格或仅有制表符即被认为是空行。

当要表示两部分文字为不同的段落时，必须在之间加入空行，不然会被合并到一段。

支持使用html标签来强制换行和空格
## 九、html高阶文本
支持html标签来控制文本更复杂的格式。
## 十、超链接
Markdown有两种超链接方式，两种的链接文字都以方括号 [ ] 标记。

第一种行内超链接，方括号后紧跟圆括号，在圆括号加入url，若是想在链接上添加标题，可在圆括号后用双引号 ""将标题内容包裹起来，当鼠标到超链接时会显示url标题。

```
这是一个[百度](https://www.baidu.com/)不含标题的超链接
这是一个[百度](https://www.baidu.com/ "百度")的超链接
```
效果如下：

这是一个[百度](https://www.baidu.com/)不含标题的超链接

这是一个[百度](https://www.baidu.com/ "百度")的超链接

第二种参考式超链接，在链接文字方括号后再加一个方括号，插入唯一的标识符以获取标识符对应的url，链接定义可以放在文本的任意地方
```
这是一个参考式[百度][1]不含标题的超链接
这是一个参考式[百度][2]超链接
[1]: https://www.baidu.com/
[2]: https://www.baidu.com/ "百度"
```
效果如下：

这是一个参考式[百度][1]不含标题的超链接

这是一个参考式[百度][a]超链接

[1]: https://www.baidu.com/
[a]: https://www.baidu.com/ "百度"

## 十一、图片

Markdown插入图片同样支持行内式和参考式，两种都以感叹号加方括号 ![ ] 标记，方括号内填写图片的替代文字，同样支持给url添加标题。

行内式
```
![替代文字](url)
url可以是网址、相对路径、绝对路径
```
参考式
```
![替代文字][2]

[2]： wujin-master.jpg
```
效果如下：

![替代文字][2]

[2]: wujin-master.jpg

当然，图片的url最好还是在线网址更好。