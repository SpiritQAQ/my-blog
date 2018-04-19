---
title: 正则
date: 2017-03-10 15:07:11
tags: '常识'
---
1. ** \d，\w,\s,[a-zA-Z0-9],\b,.,*,+,?,x{3},^,$分别是什么?**
\d 匹配数字
\w 匹配字母或数字或下划线
\s 匹配任意的空白符
[a-zA-Z0-9] 匹配中括号中的字符，也就是大写字母、小写字母和数字。
\b 匹配单词的开始或者结束
. 匹配除了换行符之外的任意字符
\* 重复0次或更多次，也就是{0,}
\+ 重复1次或更多次，也就是{1,}
? 重复0次或1次，也就是{0,1}
x{3} x重复三次
^ 匹配字符串的开始
$ 匹配字符串的结束
2. ** 写一个函数trim(str)，去除字符串两边的空白字符**
```
var str = ' haha '
function trim(str){
      return  str.replace(/^\s+|\s+$/g,'')
  }
console.log(trim(str))
```
3. ** 写一个函数isEmail(str)，判断用户输入的是不是邮箱**
```
var a = '2154353241@qq.com'
var b = 'asdf645654@yahoo.cn'
var c = '31231sad@xxx.com.cn'
function isEmail(str){
      return /^\S+@\w+\.\w+(\.\w+)?/g.test(str)
}
console.log(isEmail(a))
console.log(isEmail(b))
console.log(isEmail(c))
```
4. **写一个函数isPhoneNum(str)，判断用户输入的是不是手机号**
```
function isPhoneNum(str){
      return /^1\d{10}$/g.test(str)
}
```
5. ** 写一个函数isValidUsername(str)，判断用户输入的是不是合法的用户名（长度6-20个字符，只能包括字母、数字、下划线）**
```
function isValidUsername(str){
      return /^\w{6,20}$/g.test(str)
}
```
6. **写一个函数isValidPassword(str), 判断用户输入的是不是合法密码（长度6-20个字符，只包括大写字母、小写字母、数字、下划线，且至少至少包括两种）**
```
function isValidPassword(str){
  if(str.length <6||str.length > 20){
        return '错误，密码长度为6到20个字符'
  }else{
        if(/^[A-Z]+$|^[a-z]+$|^\d+$|^_+$/g.test(str)){
           return '错误，密码应包括大写字母、小写字母、数字、下划线中至少两种'
         }else{
           return '正确的密码'
         }
      }
}
console.log(isValidPassword('1'))     //"错误，密码长度为6到20个字符"
console.log(isValidPassword('1111111'))  //"错误，密码应包括大写字母、小写字母、数字、下划线中至少两种"
console.log(isValidPassword('AAAAAAA'))   //"错误，密码应包括大写字母、小写字母、数字、下划线中至少两种"
console.log(isValidPassword('aaaaaaaaa'))   //"错误，密码应包括大写字母、小写字母、数字、下划线中至少两种"
console.log(isValidPassword('__________'))   //"错误，密码应包括大写字母、小写字母、数字、下划线中至少两种"
console.log(isValidPassword('As_2dasasd'))   //"正确的密码"
console.log(isValidPassword('das_0123123A')) // "正确的密码"
```
7. ** 写一个正则表达式，得到如下字符串里所有的颜色**
```  
var re = /#[a-zA-z0-9]{6}/g
var subj = "color: #121212; background-color: #AA00ef; width: 12px; bad-colors: f#fddee "
console.log( subj.match(re) )  // ['#121212', '#AA00ef']
```

8. ** 下面代码输出什么? 为什么? 改写代码，让其输出[""hunger"", ""world""].**
```
var str = 'hello  "hunger" , hello "world"';
var pat =  /".*"/g;
console.log(str.match(pat));
```

 代码输出为[""hunger" , hello "world""]
第二个hello和前面的逗号也在引号内
修改后的代码
```
var str = 'hello  "hunger" , hello "world"';
var pat =  /"\S*"/g;
console.log(str.match(pat));   //[""hunger"", ""world""]
```
 答案
`var pat = /".*?"/g;`
  该题是考察贪婪和懒惰匹配
贪婪匹配:正则表达式一般趋向于最大长度匹配，也就是所谓的贪婪匹配
懒惰匹配就是匹配到结果就好，就少的匹配字符

 然而还不是很明白  为什么" , hello"不能被匹配到而"world"却能匹配到呢
测试代码
```
var a = '1hunger1 , hello 1world1'   //将引号换成1测试下是不是引号分左右的问题。发现不是引号的问题
var b = '1bbbbbb1ccccccc1dddd1' 
var c = '1 a 1 b 1 c 1 d 1 e 1' 
var d = '1 a 1 b 1 c 1 d 1 e 1 f 1'
console.log(a.match(/1.*?1/g))  //["1hunger1", "1world1"]
console.log(b.match(/1.*?1/g))  //["1bbbbbb1", "1dddd1"]
console.log(c.match(/1.*?1/g))  //["1 a 1", "1 c 1", "1 e 1"]
console.log(d.match(/1.*?1/g))  //["1 a 1", "1 c 1", "1 e 1"]
```
 很气，所以非贪婪模式就是通过确定了左右界限做到更少的匹配字符?
.*?意味着匹配任意数量的重复，但是使用最少次数的重复