---
title: 直接监听for循环，return出的i都是最后一个值
date: 2017-03-21 01:29:11
tags: 'JavaScript'
---
```
for(var i = 0;i<liList.length;i++){
                liList[i].addEventListener('click',function(){
                                console.log(i)
                },false)      //yyy
}
//xxx
```
不明白

> 这不是闭包的问题，是时机的问题。时机！

1.是三个xxx这一行还是yyy这一行先被运行到
    只要for循环遍历完成就到xxx，而不点击永远到不了yyy
    这时候i=4
 2.js没有循环的作用域，都在i的作用域
解决：
 1.标记法  给liList(i).setAttribute('data-id',i)，使用的时候获取data-id
 2.创建一个作用域
  ```
   for(var i = 0;i<liList.length;i++){
            function xxx(j){
                liList[j].addEventListener('click',function(){
                                console.log(j)
                },false)      //j的值只在xxx的作用域内有效。j的值只要得到了就不会变了。没人再操作过j
  xxx(i)    //最好再改成立即执行函数
}
// 把j改成i也可以。因为此i非彼i。 
// 函数内的i是函数内的，xxx(i)的i是for循环的
```