# MarkDown学习笔记

#### 一、标题

# H1
## H2
### H3
#### H4
##### H5
### 字符效果和横线等
#### 分割线
***
* * *
*****
- - -
----------------
~~删除线~~
*斜体字*  _斜体字_
**粗体字** __粗体字__
***粗斜体*** ___粗斜体___
下标：X<sub>2</sub> 上标 ： 0<sup>2</sup>
**缩写(同HTML的abbr标签)**

>即更长的单词或短句的缩写形式，前提是开启识别HTML标签时，已默认开启 

The  <abbr title="Hyper Text Mark Languge">H</abbr> spacification is maintained by the <abbr title="World Wide Web Consortium">W3C</abbr>

### 引用 Blockquotes

> 引用文本 Blockquotes

引用的行内混合 Blockquotes

> 引用：如果想要插入空白换行`即<br />标签`,在插入处先键入两个以上的空格再回车即可，[普通链接](https://www.mdeditor.com/)。

### 锚点与链接 Links

[普通链接](www.baidu.com)

[普通链接带标题](www.baidu.com"普通链接带标题")

直接链接：<www.baidu.com>

[锚点链接][anchor-id]

[anchor-id]: www.baidu.com

[mailto:test@gmail.com](mailto:tst.test@gmail.com)

GFM a-tail link @pandao
邮箱地址自动链接 test.test@gmail.com  www@vip.qq.com

> @pandao

### 多语言代码高亮 Codes

#### 行内代码 Inline code



执行命令：`npm install marked`

#### 缩进风格

即缩进四个空格，也做为时间类似`<pre>`预格式化文本（Preformatted Text）的功能



	<?php
	  echo "Hello world!";
	?>

预格式化文本：


	| First Header	| Second Header |
	| ------------  |  -----------  |
	|  Content Cell | Content Cell	|
	|  Content Cell | Content Cell	|

#### JS代码
```javascript
function test(){
	console.log("Hello world!");
}
```

#### HTML 代码 HTML codes

````html
	<!DOCTYPE html>
<html>
    <head>
        <mate charest="utf-8" />
        <meta name="keywords" content="Editor.md, Markdown, Editor" />
        <title>Hello world!</title>
        <style type="text/css">
            body{font-size:14px;color:#444;font-family: "Microsoft Yahei", Tahoma, "Hiragino Sans GB", Arial;background:#fff;}
            ul{list-style: none;}
            img{border:none;vertical-align: middle;}
        </style>
    </head>
    <body>
        <h1 class="text-xxl">Hello world!</h1>
        <p class="text-green">Plain text</p>
    </body>
</html>
````

### 图片 Images

图片加链接(Image+Link):

[![](https://www.mdeditor.com/images/logos/markdown.png)](https://www.mdeditor.com/images/logos/markdown.png"markdown")

> Follow your heart.  

----

#### 列表 Lists



#### 无序列表(减号) Unordered Lists (-)



- 列表一
- 列表二
- 列表三



#### 无序列表(星号) Unordered Lists (*)



* 列表一
* 列表二
* 列表三

#### 无序列表 (加号和嵌套) Unordered Lists (+)

+ 列表一
+ 列表二
  + 列表二-1
  + 列表二-2
  + 列表二-3
+ 列表三
  + 列表三-1
  + 列表三-2
  + 列表三-3

#### 有序列表 Ordered Lists(-)

1. 第一行
2. 第二行
3. 第三行

#### GFM task list

- [ ] GFM task list 1
- [ ] GFM task list 2
- [ ] GFM task list 3
  - [ ] GFM task list 3-1
  - [ ] GFM task list 3-2
  - [ ] GFM task list 3-3
- [ ] GFM task list 4
	- [ ] GFM task list 4-1
	- [ ] GFM task list 4-2

----

#### 绘制表格 Tables
> 使用-来分割表头和内容

|Title|Title|
|-|-|
|S|S|

|项目|价格|数量|
|-|-|
|计算机|￥1600|5|
|手机|￥12|12|
|管线|￥1|234|

First Header|Second Header
-|-
Content Cell|Content Cell
Content Cell|Content Cell

#### 对齐方式
1. -: 右对齐
2. :- 左对齐
3. :-: 居中对齐

|左对齐|右对齐|居中对齐|
|:-|-:|-:-|
|单元格|单元格|单元格|



----

#### 特殊符号 HTML Entities Codes

&copy; & &uml; &trade; &iexcl; &pound; 

&amp; &lt; &gt; &yen; &euro; &reg; &plusmn; &para;  &sect; &brvbar; &macr; &laquo; &middot; 

X&sup2; Y&sup3; &frac12; &frac34; &times; &divide; &raquo;

18&ordm;C &quot; &apos;

#### Emoji表情 :smiley:

Blockquotes :star:

#### 反斜杠 Escape

反斜杠可以打出符号

\* literal asterisks\*

\~ asd

#### 科学公式 Tex(KaTeX)

$$
E=mc^2
$$

$-b \pm \sqrt{b^2 - 4ac} \over 2a$


$$
E=mc^2
$$

行内公式$E=mc^2$行内公式

$x>y$

$(\sqrt{3x-1}+(1+x)^2)$

$(\sqrt{3x-1}+(1+x)^2)$

$\sin(\alpha)^{\theta}=\sum_{i=0}^{n}(x^i + \cos(f))$

##### 多行公式

$$
\displaystyle
\left( \sum\_{k=1}^n a\_k b\_k \right)^2
\leq
\left( \sum\_{k=1}^n a\_k^2 \right)
\left( \sum\_{k=1}^n b\_k^2 \right)
$$

$$
\displaystyle
    \frac{1}{
        \Bigl(\sqrt{\phi \sqrt{5}}-\phi\Bigr) e^{
        \frac25 \pi}} = 1+\frac{e^{-2\pi}} {1+\frac{e^{-4\pi}} {
        1+\frac{e^{-6\pi}}
        {1+\frac{e^{-8\pi}}
         {1+\cdots} }
        }
    }
$$

$$
f(x) = \int_{-\infty}^\infty
    \hat f(\xi)\,e^{2 \pi i \xi x}
    \,d\xi
$$

------

### 绘制流程图 Sequence Diagram

```flow
st=>start: 用户登陆
op=>operation: 登录操作
cond=>condition: 登录成功 Yes or No?
e=>end: 进入后台

st->op->cond
cond(yes)->e
cond(no)->op
```





#### 绘制序列图 Sequence Diagram

``` sequence
Andrew->China: Says Hello
Note right of China: China thinks\nabout it
China-->Andrew: How are you?
Andrew->>China: I am good thanks!

```

