misc常见编码题目总结

#### 1.

##### 题目

```
01100001
01010111
00111001 
01101011
01100001
01101110
01110100
00110110
01100001
01000100
01000110
01101101
01010001
01001000
01000010
01101111
01011000
00110011
01100011
01110111
01011000
00110011
01000001
01111000
01100100
01101101
01011010
00111001
```

##### 分析：

8位二进制，0-255，

但第一位都是0，那就是0-127，

正好在ASCII的范围，容易想到转为10进制，再转为字符，

那就试一试！

##### 进制转换&ASCII转换

写一个程序，把这些每8位一组，转为数字，再输出字符；

```c
 char nums[224] = "01100001010101110011100101101011011000010110111001110100001101100110000101000100010001100110110101010001010010000100001001101111010110000011001101100011011101110101100000110011010000010111100001100100011011010101101000111001";
	int i;
	int j = 0;
	int num = 0;
	for(i = 0; i < 224; i++){
		j++;
		//1111
		num += (nums[i] - 48) * pow(2,8-j);
		
		if(j == 8){
			printf("%c",num);
			j=0;
			num = 0;
		}
	}
```



```
结果：aW9kant6aDFmQHBoX3cwX3AxdmZ9
```

这一串`aW9kant6aDFmQHBoX3cwX3AxdmZ9`明显不是flag，所以继续解码

##### base64和凯撒密码

base64解码之后得到，`iodj{zh1f@ph_w0_p1vf}`，

这和flag的格式非常相似，flag和iodj经过相同的位移之后就是flag，

凯撒密码位移三位；解码就是`flag{we1c@me_t0_m1sc}`

##### 总结

`iodj{zh1f@ph_w0_p1vf}`这种格式的：有密钥就是维吉尼亚密码，没有就是凯撒密码

`01100001`这种格式的密文：单纯的0和1的不规律出现，想到进制转换和ASCII码转换，

`aW9kant6aDFmQHBoX3cwX3AxdmZ9`这种格式的：只有大写字母和小写字母及数字的，考虑base64编码



#### 2.

##### 题目

```
bbaabbaabbabbaaabbaaaababbaabbbabbbbabbabaabaaaabbaabababbbbaaaababbbbbaabbaaababbbaabbababbbbbaabbaaababbabbababbbaaaaaabbaaaaabbbaabaabbbabaaabaaaaaaabbabbbaabbbabaaabbbbbab
```

##### 分析

考虑培根密码和ab转换为01二进制，实在不行摩斯密码

##### 思路一：培根密码

转换结果:`ztmmftxwsdfpbptcgfwxamdsoragd`

这只有小写字母，而且没有数字，试一下base64确实解不出来什么

==暂时抛弃

##### 思路二：01二进制

先编程序

```c
char str[] = "bbaabbaabbabbaaabbaaaababbaabbbabbbbabbabaabaaaabbaabababbbbaaaababbbbbaabbaaababbbaabbababbbbbaabbaaababbabbababbbaaaaaabbaaaaabbbaabaabbbabaaabaaaaaaabbabbbaabbbabaaabbbbbab";
int len = strlen(str);
int i;
printf("%d\n", len);
for(i = 0; i< len; i++){
    if(str[i] == 'b'){
		printf("%c", '1');
    }else if(str[i] == 'a'){
		printf("%c", '0');
    }
}
```

结果：

```
175
1100110011011000110000101100111011110110100100001100101011110000101111100110001011100110101111100110001011011010111000000110000011100100111010001000000011011100111010001111101
```

分析：

175位，8位8位也不好分啊

176正好是22个8位，还有点可能

##### 思路三：摩斯密码

b为-

a为.

程序：

```c
char str[] = "bbaabbaabbabbaaabbaaaababbaabbbabbbbabbabaabaaaabbaabababbbbaaaababbbbbaabbaaababbbaabbababbbbbaabbaaababbabbababbbaaaaaabbaaaaabbbaabaabbbabaaabaaaaaaabbabbbaabbbabaaabbbbbab";
int len = strlen(str);
int i;
printf("%d\n", len);
for(i = 0; i< len; i++){
    if(str[i] == 'b'){
		printf("%c", '-');
	}else if(str[i] == 'a'){
		printf("%c", '.');
	}
}
```

结果：

```
--..--..--.--...--....-.--..---.----.--.-..-....--..-.-.----....-.-----..--...-.---..--.-.-----..--...-.--.--.-.---......--.....---..-..---.-...-.......--.---..---.-...-----.-
```

解码：

什么都没有，直接放弃

##### 01二进制

由于这样题的8位二进制数超过了ASCII的值，

但除以2正好是f，

借助题目一的程序：

```c
char nums[175] = "1100110011011000110000101100111011110110100100001100101011110000101111100110001011100110101111100110001011011010111000000110000011100100111010001000000011011100111010001111101";
	int i;
	int j = 0;
	int num = 0;
	for(i = 0; i < 175; i++){
		j++;
		//1111
		num += (nums[i] - 48) * pow(2,8-j);
		
		if(j == 8){
			printf("%c",num/2);
			j=0;
			num = 0;
		}
	}
	printf("%c", num/2);
```

开心的得出：`flag{Hex_1s_1mp0rt@nt}`



