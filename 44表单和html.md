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
 - 每个Razor视同都继承了他们基类Html属性。Html属性的类型是System.Web.Mvc.HtmlHelper<T>，框架定义的大多数辅助方法都是扩展方法。
在vv中有个向下的箭头。
- 在C#扩展方法中只有当在它的名称空间范围内，才能用
- MVC所有的HtmlHelper扩展方法都是在名称空间System.Web.Mvc.Html中。

###Html.TextBox、Html.TextArea和Html.Lable
 - Html.TextBox
```
@Html.TextBox("Age","23",new{@class="text1"})
```
output:
```html
<input class="text1" id="Age" type="text" name="Age" value="23" />
```
 - Html.TextArea
```
@Html.Area("text","hello </br> world",10,80,null)
```
output:
```
<textarea id="text" name="text" rows="10" cols="80">
    hello &lt;br/&gt word
</textarea>
```
 - Html.Label <label>标签
```
@Html.Lable("label1","你好")
```
output:
```
<label for="label1">你好</label>
```

###Html.DropDownList 和 Html.ListBox
- dropdownlist和listbox都返回select元素，dropdownlist允许单项选择。而listbox支出多项（multiple特性设为multiple）
- 这些辅助方法需要一些特定信息.因此当在控制器中使用的时候需要做一些额外设置工作。
- 下拉列表它需要一个包含所有可选项目SelectListItem对象集合。其中每个SelectListItem对象又包含Text，Value，Selected三个属性。可以根据自己的需要构建自己的SelectListItem对象集合。也可以使用框架中的SelectList或者MultiSelectList辅助方法类来构建。这些类可以查看任意类型的IEnumerable对象并将其转化为SelectListItem。
```
@{
    List<SelectListItem> list= new List<SelectListItem>{
    new SelectListItem{ Text="启用",vlaue="0",Selected = true},
    new SelectListItem{ Text="禁止"，value="1"}
    };
}
@Html.DropDownList("state",list,null,new{})
//生成的Html代码
<select id="state" name="state">
    <option selected="selected" value="0">启用</option>
    <option value="1">禁用</option>
</select>
```
或者在控制器中（比如edit中,使用MusicStore的demo）：
```
public ActionResult Edit（int id）
{
    var album= db.Albums.Single（a=>a.AlbumId == id）;
    ViewBag.Genres = db.Genres.OrderBy(g=>g.Name).AsEnumerable.Select(g=>new SelectListItem{
        Text = g.Name,
        Value = g.GenreId.ToString(),
        Selected = album.GenreId == g.GenreId
    });
 return View(album);
}
```
使用SelectList：
```
public ActionResult Edit(int id)
{
    var album = db.Albums.Single(a=>a.AlbumId==id);
    ViewBag.GenreId = new SelectList(db.Genre.OrderBy(g=>g.Name),"GenreId","Name",album.GenreId)；
    return View(album);
}
//在view中
<div class="form-group">
    @Html.LabelFor(model=>model.ArtistId,"ArtistId",new {@class="control-label col-md-2"})
    <div class="col-md-10">
        @Html.DropDownList("GenreId",String.Empty)
        @Html.ValidationMessageFor(model=>model.ArtistId)
    </div>
</div>
```
 - 这里的控制器操作不仅是构建了主要模型（用于编辑的专辑）。还构建了下拉列表辅助方法所需要的模型。从上面的代码可以看出。SelectList构造函数指定了原始集合（数据库中的Genre表）、作为后台值使用的属性名称（GenreId）、作为显示文本使用的属性（Name）,以及当前所选项的值。

###Html.ValidationMessage
 - 当ModelState字典中的某个特定字段出现错误时，可以使用ValidationMessage辅助方法来显示相应的错误提示消息。在下面的控制器中为模型状态添加一个错误。
```
[HttpPost]
public ActionResult Edit(int id,FormCollection collection)
{
    var album  = db.Albums.Find(id);
    ModelState.AddModelError("Title","What a terrible name!");
    return View(album);
}
```
- 在视图中可以使用下面的代码显示错误信息
`@Html.ValidationMessage("Title")`


###强类型辅助方法
 - 如果不适应使用字符串字面值从视图数据中提取值的话。也可以使用MVC提供的各种强类型辅助方法。使用强类型辅助方法时，只需要为其传递一个lambda表达式来指定要渲染的视图模型。表达式的模型类型必须和视图指定的模型类型（使用@model指令）一致。
```
    @Html.TextBoxFor(m=>m.Title)
```

###辅助方法和模型元数据
- 辅助方法不仅查看了ViewData内部的数据。他们也利用可得到的模型元数据。例如，专辑编辑表单使用Label辅助方法来为流派选择列表显示一个label元素。
```
@Html.Label("GenreId")
```
这个辅助方法生成的HTML如下：
```
    <label for="GenreId">Genre</label>
```

