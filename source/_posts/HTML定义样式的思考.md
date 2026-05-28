---
title: HTML定义样式的思考
date: 2020-04-19 12:00:00
categories:
  - H5
tags:
  - HTML
---

[](#思考并整理设置文字”123”显示红色有多少种定义样式方法)思考并整理设置文字”123”显示红色有多少种定义样式方法

<!--more-->

**例**



1
2
3
4
5
6
7
8
9
10
11
12
body>
div class=“classDiv” id=“idDiv>
p class=“classP” id=“idP”>
123
p>
div>
div class="classDiv2" id="idDiv2">div>
p class="classP2" id="idP2">
456
p>
body>







**答**



1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
*&#123;
color: red;
&#125;
/*通用选择器*/

p&#123;
color: red;
&#125;
/*标记选择器*/

#idDiv&#123;
color: red;
&#125;
#idP&#123;
color: red;
&#125;
/*ID选择器*/

.classDiv&#123;
color: red;
&#125;
.classP&#123;
color: red;
&#125;
/*类选择器*/

div.classDiv&#123;
color: red;
&#125;
p.classP&#123;
color: red;
&#125;
/*交集选择器*/

div,p&#123;
color: red;
&#125;
/*选择div和p元素（多元素选择器）*/

div p&#123;
color: red;
&#125;
/*选择div下的所有p元素（后代选择器）*/

div>p&#123;
color: red;
&#125;
/*（直接）子选择器  
这里也可以与交集选择器进行组合，形成直接类或直接ID选择器（我也不知道，自己起的名字）
随着代码的复杂度或者嵌套程度，还有好多组合表达式...
*/

div+p&#123;
color: red;
&#125;
/*相邻兄弟选择器，拥有相同父元素，选中紧随在后面的一个兄弟元素，456变红*/

div~p&#123;
color: red;
&#125;
/*普通兄弟选择器，拥有相同父元素，元素间可不直接紧随，456变红*/









属性选择器 |
语法 |
实例 |
描述 |



存在选择器 |
[attribute] |
[id]或p[id] |
任何带id属性的所有标记或p标记 |


相等选择器 |
[attribute=value] |
[name=“yy”] |
name属性为“yy”的所有标记 |


包含选择器 |
[attribute~=value] |
[name~=“y”] |
name属性中包含“y”，并于其它内容通过空格隔开的所有标记 |


前缀选择器 |
[attribute^=value] |
[name^=“y”] |
选择name属性值以“y”开头的所有元素 |


子串选择器 |
[attribute*=value] |
[name*=“y”] |
选择name属性包含“y”的所有元素 |


后缀选择器 |
[attribute$=value] |
[name$=“y”] |
选择name属性值以“y”结尾的所有元素 |













赏




好活当赏！




![](/assets/paywechat.jpg)
微信















**本文作者：**
Alituofo


**本文链接：**
[http://alituofo.github.io/2020/04/19/HTML定义样式思考并整理/](http://alituofo.github.io/2020/04/19/HTML定义样式思考并整理/)



**版权声明： **

本博客所有文章除特别声明外，均采用 [MIT](https://github.com/alituofo/hexo-theme-yilia-plus/blob/master/LICENSE) 许可协议。转载请注明出处！
