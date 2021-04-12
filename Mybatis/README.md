## #{}和${}区别

MyBatis中使用parameterType向SQL语句传参，parameterType后的类型可以是基本类型int,String,HashMap和java自定义类型。

　　　　在SQL中引用这些参数的时候，可以使用两种方式#{parameterName}或者${parameterName}。

#{}
　　　　#将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。

　　　　　　例如：order by #{parameterName} //或取Map中的value#{Key}也是一样操作。

　　　　　　假设传入参数是“Smith”

　　　　　　会解析成：order by "Smith"

${} 
　　　　$将传入的数据直接显示生成在sql中。
　　　　　　例如：order by ${parameterName} //或取Map中的value${Key}也是一样操作。

　　　　　　假设传入参数是“Smith”

　　　　　　会解析成：order by Smith

概念
#{}方式能够很大程度防止sql注入，${}方式无法防止Sql注入。
${}方式一般用于传入数据库对象，例如传入表名。
从安全性上考虑，能使用#尽量使用#来传参，因为这样可以有效防止SQL注入的问题。

#### 重点
　　　MyBatis排序时使用order by 动态参数时需要注意，用$而不是#！

　　　　例如：ORDER BY ${columnName} //这里MyBatis不会修改或转义字符串，可实现动态传入排序。

　　　　建议：接受从用户输出的内容并提供给语句中不变的字符串，这样做是不安全的。

　　　　　　　这会导致潜在的SQL注入攻击，因此你不应该允许用户输入这些字段，或者通常自行转义并检查
