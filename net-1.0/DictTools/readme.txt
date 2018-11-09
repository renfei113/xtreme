Excel2XMl
   可以将excel表格输出为xml和js文件，并且相应的生成文件的读取代码，支持java，c#语言

输出命令：Excel2XMl.exe 表格目录 -out 输出目录 <-ext 扩展名>  -java 


1.表名命名规则：
	备注&模块名|子模块名|...|元素名>文件名

	输出文件"文件名.xml",内容如下:
	<模块名>
 	  <子模块名>
 	    <元素名  id="1"/>
 	    <元素名  id="2"/>
 	    。。。
 	  </子模块名>	
	</模块名>

2.表内表头名规则:
	备注&node1名|node2名|elem|columnid

	<node1>
	  <node2>
 	  <elem columnid="xxx" .../> 	   
	   。。。
	   </node2>
	</node1>
表明中包含parent<elem%i>可以将本表加入上一个包含parent<elem%i>路径的节点内去!其中%i表示parent的第几个elem数据


表内的column名字为rootName和rootValue表示模块参数

3.特殊配置表
    表明必须为 sheetConfig
    第一列为表的索引
    第二列为表名配置（见2.表内表头名规则）
    注意第一行为每列说明，正式配置从第2行开始

4.每个表内可以指定名字为 [group] 列，这个值一样的连续行作为一条数据。作用：自动跳过第2行开始的第一级数据(头不包含|的列数据).
	注意：头定义使用逗号，分割
	          数据使用竖线|分割
 例子:
	头定义为: 测试&ch1[elem1,elem2,ch1ch[elem3,elem4,ch1ch2[elem5,elem6,elem7],elem8],elem9]
	数据定义为: [11|12|[[21|22|[[31|32|33]|[34|35|36]]|24]|[25|26|[[37|38|39]|[40|41|42]]|28]]|14]|[111|112|[[121|122|[[131|132|133]]|124]]|114]

5.代码生成定义规则
 加入代码规则表头，格式如下
	类型|变量名<子类型定义>
	如果是数组(Array),list,hashmap,enum，那么类型格式为Array(类型),List(类型)，hashmap(key类型,value类型),enum(类型),func(json结构)
	子类型的格式规则相同
	如果是括号定义的子类型，那么代码规则也是[变量类型定义1，变量类型定义2,...]
   
        注意：func时，括号内必须为标准的json字串，格式为"java":"函数定义" ,"cs"："函数定义","as"："函数定义",函数定义不包含函数名和形参，只有函数体

举例：
	CTimeManager|m_cTimeManager<enum(ETimeType)|m_eType>
	Array(CEffectArea)<Array(int)>
	Array(CBuff)<[int,int]>
	int(255)
	string(5)

6.sheetConfig定义规则
	第一列：表，表示第几个sheet的定义
	第二列：表输出设置
	第三列：表头所在行
	第四列：数据起始行
	第五列：包名，代码生成的包名(文件夹名)
	第六列：类名,代码生成的类名
	第七列：代码定义行
	第八列：全局查找表hashmap所用的key的在表头名字
	第九列：包含或引入的库名
	第十列：isUnion,是否合并到一个代码定义中
	第XI列：extends，父类定义，json字串，格式 {"java":["名字"，"名字"],"cs":["名字"，"名字"]}
	第XII列：codeDefine:用户自定义函数，标准的json字串。格式：{"java":{"类名":{}},"cs":{"var":[变量定义,...],"func":[函数定义,...]},"as":{}}
        第XIII类: exclude ,忽略生成类的名字，格式["类名","类名","类名"]
        