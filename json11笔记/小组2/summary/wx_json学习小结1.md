#Json学习小结
_雾晓_ &emsp;2/12/2017 3:47:43 PM 
##Json背景知识
Step1: 理解Json是什么~
###简介
&emsp;Json的全称为*”JavaScript Object Notation”*，即**JavaScript对象表示法。**
是类似XML的存储和交换文本信息的语法。但Json更小、更快、更容易解析（能使用内建的Javascript eval() / parse()等方法进行解析）。

&emsp;Json大概就是一个能够统一http标准传输格式的表示对象的字符串。
###结构
&emsp;Json简单说就是javascript中的对象和数组，所以其结构包含对象和数组，经过对象、数组两种结构就可以组合成复杂的数据结构。

+ 对象：

	在花括号 { } 中，可包含多个**key/value（对象的属性/对应的属性值）对**，中间以 ,（逗号）相隔，如下所示：

	`{
	    key1:value1,
	    key2:value2,
	    ...
	}`

	**key**: 字符串

	**value**: 数字 / 字符串 / 逻辑值 / 数组（方括号） / 对象（花括号） / null

	**取值方法**：对象.key&emsp;即可获取属性值

+ 数组：

	在方括号 [ ] 中，可包含多个**对象**，中间以 ,（逗号）相隔，如下所示：

	`[
	    {
	        key1:value1,
	        key2:value2 
	    },
	    {
	         key3:value3,
	         key4:value4   
	    },
		...
	]`

###JS中Json的使用方法
&emsp;首先定义一个Json对象:

	var obj = {"objs":[
				{
					"group":2,
					"task":"reading source code",
					"person": [ 
			                        {
			                            "id": 0,
			                            "role": "leader"
			                        },
			                        {
			                            "id": [1,2,3,4,5,6,7,8,9,10],
			                            "role": "member"
			                        }
			                   ]
					},	
					{
						"group":1
					}		
				]
	};

+ 读Json

	**对象.属性**
	
	例如：obj.objs[0].group


+ 写Json

	- 修改/添加
	 
		**对象.属性 = 属性值**
	
		1. 若属性已存在，则可进行修改操作；
		2. 若属性不存在，则可进行添加操作；

		例如：obj.objs[0].mess = null;

	- 删除
	
	  	**delete 对象.属性**
		
		例如：delete obj.objs[0];

##源码阅读
+ README
	+ Json11能够提供Json的解析和序列化；
	+ Json对象代表了任意Json值；
	+ dump（）方法可以将Json对象序列化成一个字符串；？
	+ parse方法可以将一个字符串解析成一个Json对象；
	+ 还有一些构造器支持将用户自定义的格式转换成Json；
	+ Json值可以被查询；
+ hpp
	+ Json
		+ type声明： NUL, NUMBER, BOOL, STRING, ARRAY, OBJECT
		+ vector<Json> array
		+ <string, Json> object
	+ JsonValue
+ cpp
	+ dump
	+ parse


##参考来源
- http://www.json.cn/
- http://www.runoob.com/json/json-tutorial.html
- http://www.cnblogs.com/mcgrady/archive/2013/06/08/3127781.html
	

