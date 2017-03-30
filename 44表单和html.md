##表单的使用
 - action 和method特性
  - action特性告知web浏览器信息往哪里发
  - method特性告知浏览器使用的是post还是get
  - 默认的方法是get
  - 实例：
```
<form action="/Home/search" method="get">
     <input name="q" type="text" />
     <input type = submit value="search" />
</form>
```
  - 如果不想让浏览器把输入的值放入查询字符串，而是想放入http请求的主体，就可以给method赋值post。尽管向搜索引擎发送Post请求也能看到搜索结果。但是使用get请求会好一些。不像post，get请求所有的参数都在URL中。因此可以为get请求建立书签，更主要的是get方法代表的是幂等操作和只读操作。所以是做这些工作的最好选择。换而言之，get不会改变服务器状态，所以客户端可以向服务器重复的发送get请求而不会产生负面影响。
 - 在web应用中，get请求用于读操作，而Post请求用于写操作（更新改删）

##HTML辅助方法
  - 可以通过视图的Html属性调用HTML辅助方法，响应的也可以通过URL属性调用URL辅助方法。通过AJAX调用AJAX辅助方法

###Html.BeginForm
 ```
@using(Html.BeginForm("Search","Home",FormMethod.Get)){
    <input type="text" name="q">
    <input type="submit" value="search">
}
```
 - BeginForm辅助方法输出的标记与前面的第一次实现搜索的表单一样，然而该方法与路由引擎一起协调生产合适的url。
  - 注意，BeginForm辅助方法输出的是起始的<form>和结束的</form>标签。辅助方法在调用BeginForm期间生产了一个起始标签。并返回一个实现了接口的IDisposable的对象。当视图中的代码执行到结束using语句的花括号的时候。隐式的调用了Dispose方法。因此会生成一个</form>标签。使用Using语句使得代码变得简洁而优雅。也可不使用using
```
@{Html.BeginForm("search","Home",FormMehod.Get);}
  <input type="text" name="q" />
  <input type="submit" value="search">
@{Html.EndForm();}
```
- 辅助方法在保护代码的同时，也给了方法适当的控制
```
@using(Html.BeginForm（"Search","Home",FormMethod.Get,new{targe="_blank",@class="editForm"})){
    <input type="text" name="q" />
    <input type="submit" value="search" />
}
```
- 可以看到上面使用htmlAttributes参数设置了target="_blank",class="editForm".由于class是C#的关键字。所以，必须在class前面加个@号

###自动编码
 - 许多辅助方法都可以采用输出模型值。所以这些输出的模型值的辅助方法都会在渲染前对值进行HTML编码。如TextArea辅助方法
```
    @Html.TextArea("text","hello <br/> world");
```
 - 输出的是
```
<textarea cols="20" id="text" name="text" rows="2">
  hello &lt;br/ &gt; world
</textarea>
```
输出的值是经过HTML编码的。默认的编码可以帮助避免跨站点的脚本攻击（Cross Site Scripting，XSS）；

###辅助方法的工作原理
 - 每个
