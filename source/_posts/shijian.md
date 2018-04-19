---
title: DOM事件
date: 2017-03-21 01:09:11
tags: ['浏览器','常识' ]
---

1. ** DOM0 事件和DOM2级在事件监听使用方式上有什么区别？**
DOM0事件，就是将一个函数赋值给一个事件处理程序属性。每个元素包括window和document都有自己的时间处理程序属性，这些属性通常全部小写，例如onclick，将这种属性的值设置为一个参数，就可以指定事件处理程序。
```
 var btn = document.getElementById('myBtn')
btn.onclick = function(){
         alert(this.id);    //myBtn
}
```
 使用DOM0指定事件的处理程序被认为是元素的方法。因此这时候的事件处理程序是在元素的作用域中运行。this引用的是当前元素。例子在上面

  DOM2定义了两个方法，用于处理指定和删除事件处理程序的操作：addEventListener()和removeEventListener()。所有DOM节点中的包含这两个方法，并且它们都接受三个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。
最后这个布尔值参数如果是true，表示在捕获阶段调用事件处理程序，如果是false，表示在冒泡阶段调用事件处理程序。如果不写，默认为false，老式浏览器规定该参数必写，较新版本的浏览器允许该参数可选。为了保持兼容，建议总是写上该参数。
```
  var btn = document.getElementById('myBtn')
  btn.addEventListener('click',function(){
      alert(this.id)
  },false)
```
  使用DOM2级方法添加事件处理程序的主要好处是可以添加多个事件处理程序，按照添加它们的顺序触发。
 
   移除事件处理程序的方法
   DOM : 将事件处理程序设置为null   `btn.onclick = null`
   DOM2 : 用removeEventListener()
2. ** attachEvent与addEventListener的区别？**
IE实现了与DOM中类似的两个方法: attachEvent()和detachEvent()
这两个方法接受相同的两个参数：事件处理程序名称和事件处理程序函数。而addEventListener接受三个参数。
```
var btn = document.getElementById('myBtn')
btn.attachEvent('onclick',function(){
      alert('clicked')
})
```
 要注意的是，attachEvent()第一个参数是'onclick'，而非DOM的addEventListener()方法中的'click'

  在IE中使用attachEvent()与使用DOM方法的主要区别在于事件处理程序的作用域。在使用attachEvent()方法的情况下，事件处理程序会在全局作用域中运行，因此this 等于 window

  多次调用的调用顺序和DOM方法不用，事件处理程序不是以添加他们的顺序执行，而是以相反的顺序被触发。

3. ** 解释IE事件冒泡和DOM2事件传播机制？**
IE的事件流叫做事件冒泡，即事件开始时是由最具体的元素接受，然后逐级向上传播到较为不具体的节点。
网景的事件流叫做事件捕获，不太具体的节点更早接收到事件，而最具体的节点应该最后接收到事件。用意是在事件到达预定目标前捕获它。

  *DOM2事件传播机制*
 DOM2级事件 规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的事件捕获，为截取事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶段，在这个阶段对事件作出响应。
在DOM2事件传播机制中，实际的目标在捕获阶段不会接收到事件，捕获阶段只到实际目标的父节点。进入 处于目标阶段 ，事件在目标是发生，并在事件处理中被看成冒泡阶段的一部分。
但是IE9、Safari、Chrome、Firefox和Opera9.5及更高版本都会在捕获阶段触发事件对象上的事件。就是有两个机会在目标对象上操作

4. ** 如何阻止事件冒泡？ 如何阻止默认事件？**
event.stopPropagation()
阻止捕获和冒泡阶段中当前事件的进一步传播
event.preventDefault()
preventDefault方法取消浏览器对当前事件的默认行为，比如点击链接后，浏览器跳转到指定页面，或者按一下空格键，页面向下滚动一段距离。该方法生效的前提是，事件的cancelable属性为true，如果为false，则调用该方法没有任何效果。


5. **有如下代码，要求当点击每一个元素li时控制台展示该元素的文本内容。不考虑兼容**
```
var ul = document.getElementsByClassName('ct')
var li = ul[0].getElementsByTagName('li')

 //1

 for(var i=0;i<li.length;i++){
    li[i].addEventListener('click',function(e){
      console.log(this.innerText)
    },false)
}

 //2
 
 ul[0].addEventListener('click',function(e){
     console.log(e.target.innerText)
 })   //返回事件触发的节点，是ul中的li

 //3

  for(var i=0;i<li.length;i++){
     li[i].addEventListener('click',function(e){
       console.log(e.currentTarget.innerText)
     },false)
 }    //返回事件当前所在的节点 也就是li

 //4

 for(var i=0;i<li.length;i++){
    li[i].setAttribute('onclick','')
    li[i].onclick = function(){
      console.log(this.innerText)
    }
```
 event.currentTarget和event.target
currentTarget返回的是事件当前所在的节点。在监听函数中，currentTarget属性实际上等同于this对象。
target返回的是触发事件的节点
6. 补全代码，要求：
当点击按钮开头添加时在`<li>这里是</li>`元素前添加一个新元素，内容为用户输入的非空字符串；当点击结尾添加时在最后一个 li 元素后添加用户输入的非空字符串.
当点击每一个元素li时控制台展示该元素的文本内容。
```
<ul class="ct">
    <li>这里是</li>
    <li>123</li>
    <li>345</li>
</ul>
<input class="ipt-add-content" placeholder="添加内容"/>
<button id="btn-add-start">开头添加</button>
<button id="btn-add-end">结尾添加</button>
<script>
var ul = document.getElementsByClassName('ct')
var li = ul[0].getElementsByTagName('li')
var addStart = document.getElementById('btn-add-start')
var addEnd = document.getElementById('btn-add-end')
var change = document.getElementsByClassName('ipt-add-content')
addStart.addEventListener('click',function(){
  var myString = document.createElement('li')
  var stringContent = document.createTextNode('')
  stringContent.data = change[0].value
  myString.appendChild(stringContent)
  if(myString.innerText===''){
    console.log('请输入一个非空字符串')
    alert('请输入一个非空字符串')
  }else{
    ul[0].insertBefore(myString,li[0])
  }
},false)

 addEnd.addEventListener('click',function(){
  var myString = document.createElement('li')
  var stringContent = document.createTextNode('')
  stringContent.data = change[0].value
  myString.appendChild(stringContent)
  if(myString.innerText===''){
    console.log('请输入一个非空字符串')
  }else{
    ul[0].appendChild(myString)
  }
},false)
ul[0].addEventListener('click',function(e){
  console.log(e.target.innerText)
</script>
```