---
title: HTML定义样式的思考
date: 2020-04-19 20:00:00
update: 2020-04-20-18:00:00
tags: HTML
top: 2
categories: 
- HTML
---

### 思考并整理设置文字“123”显示红色有多少种定义样式方法

---

**例**

```html
<body>
    <div class=“classDiv” id=“idDiv>
        <p class=“classP” id=“idP”>
            123
        </p>
    </div>
    <div class="classDiv2" id="idDiv2"></div>
	<p class="classP2" id="idP2">
		456
	</p>
</body>

```

<!-- more -->



**答**

```css
*{
	color: red;
}
/*通用选择器*/

p{
	color: red;
}
/*标记选择器*/

#idDiv{
	color: red;
}
#idP{
	color: red;
}
/*ID选择器*/

.classDiv{
	color: red;
}
.classP{
	color: red;
}
/*类选择器*/

div.classDiv{
	color: red;
}
p.classP{
	color: red;
}
/*交集选择器*/

div,p{
	color: red;
}
/*选择div和p元素（多元素选择器）*/

div p{
	color: red;
}
/*选择div下的所有p元素（后代选择器）*/

div>p{
	color: red;
}
/*（直接）子选择器  
这里也可以与交集选择器进行组合，形成直接类或直接ID选择器（我也不知道，自己起的名字）
随着代码的复杂度或者嵌套程度，还有好多组合表达式...
*/


div+p{
	color: red;
}
/*相邻兄弟选择器，拥有相同父元素，选中紧随在后面的一个兄弟元素，456变红*/

div~p{
	color: red;
}
/*普通兄弟选择器，拥有相同父元素，元素间可不直接紧随，456变红*/
```



| 属性选择器 | 语法               | 实例        | 描述                                                  |
| :--------: | ------------------ | ----------- | ----------------------------------------------------- |
| 存在选择器 | [attribute]        | [id]或p[id] | 任何带id属性的所有标记或p标记                         |
| 相等选择器 | [attribute=value]  | [name=“yy”] | name属性为“yy”的所有标记                              |
| 包含选择器 | [attribute~=value] | [name~=“y”] | name属性中包含“y”，并于其它内容通过空格隔开的所有标记 |
| 前缀选择器 | [attribute^=value] | [name^=“y”] | 选择name属性值以“y”开头的所有元素                     |
| 子串选择器 | [attribute*=value] | [name*=“y”] | 选择name属性包含“y”的所有元素                         |
| 后缀选择器 | [attribute$=value] | [name$=“y”] | 选择name属性值以“y”结尾的所有元素                     |

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=31961584&auto=1&height=66"></iframe>

