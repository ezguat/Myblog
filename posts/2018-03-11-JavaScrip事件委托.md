# 了解前提
&ensp;&ensp;当我们想对JS事件驱动的时候，一般情况下会执行DOM事件，比如说<br>
```scss
<p>点击一下我</p>
```
&ensp;&ensp;这个时候我们会写<br>
```scss
<p onclick="alert("已经点击)">点击一下我</p>
```
&ensp;&ensp;或者是<br>
```scss
<p id="click">点击一下我</p>
```
&ensp;&ensp;然后在后面的代码通过DOM事件去监听<br>
```scss
document.getElementById("click").click(function () {
        alert("已经点击");
    })
```
&ensp;&ensp;但其实以上方法都有几个缺陷，一个是单个标签的时候还能够使用，当标签数量超过几百个的时候，再为每个标签事件监听，代码会显得很冗余；还有就是部分浏览器对DOM事件实际上并不是完全支持，比如说Firefox，所以实际上更推荐JS原生的事件委托，这就是下面例子<br>
```scss
<ul id="myLinks">
 <li id="goSomewhere">Go somewhere</li>
 <li id="doSomething">Do something</li>
 <li id="sayHi">Say hi</li>
</ul>
var item1 = document.getElementById("goSomewhere");
var item2 = document.getElementById("doSomething");
var item3 = document.getElementById("sayHi");
EventUtil.addHandler(item1, "click", function(event){
 location.href = "http://www.wrox.com";
});
EventUtil.addHandler(item2, "click", function(event){
 document.title = "I changed the document's title";
});
EventUtil.addHandler(item3, "click", function(event){
 alert("hi");
});
```
&ensp;&ensp;当事件执行的时候，便会自动的执行与ID相对应的事件，不用在onlick事件中再写详细的事件，这便是JS中的事件委托。
