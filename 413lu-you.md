## 路由

### URL

* 统一资源定位符（Uniform Resource Locator）
* URI（Uniform Resource Identifier），统一资源标识符。技术上来说所有的URL都是URI。
* 好的URL应该满足：
  * 域名便于记忆
  * 简短
  * 便于输入
  * 可以反映出站点的结构
  * 应该是可`破解的`，用户可以通过移除URL的末尾，进而达到更高层次的信息体系结构
  * 持久，不能改变


### 路由概述

* ASP.NET MVC框架中的路由主要有两个用途
  * 匹配传入的请求（该请求不匹配服务器文件系统的文件）。并把这些请求映射到控制器操作中
  * 构造传出的URL，用来响应控制器操作


### 对比路由和URL重写

* URL重写和路由都可以分离传入的URl和结束处理请求
* 他们之间也有很大的差别。URl重写是关注从一个URL映射到另一个URL上去，而路由关注的是如何将URL映射到资源上去

### 特性路由

* 这是在MVC5中新增的。
* 创建MVC web应用程序后，浏览一下Global.saax.cs文件。Application\_Start方法中调用了一个名为RegisterRoutes的方法。先删除
  其他方法。修改后的RegisterRouters方法如下：

  ```c#
  public static void RegisterReoutes(RouteCollection routers)
  {
  routes.MapMvcAttributeRoutes();
  }

  ```

* 编写第一个路由

  ```
  public static voil RegisterRoutes(RouterCollection routes)
  {
  [Route("About")]
  public ActionResult About()
  {
     return View（）；
  }
  }
  ```

- 每当收到URL为/about的请求时，这个路由特性就会运行About方法。我们告诉MVC使用URL
- 对于操作多个URL,就可以使用多个路由特性。例如，我们可能想让首页通过/、/home和/home/index这几个URL都能访问。这时，路由如下：
```
[Route("home")]
[Route("home/index")]
public ActionResult Index()
{
    return View();
}
```
- 传入路由特性的字符串叫做路由模板，他就是一个模式匹配规则。
###路由值
- 对于最简单的路由，非常适合刚才的静态路由。但是不是每个URL都是静态的。可以通过添加路由参数解决这个问题
```
    [Route("Person/{id}")]
    public ActionResult Details(int id)
    {
        return View();
    }

    [Route("{year}/{month}/{day}")]
    public ActionResult index(string year,string month,string day)
    {
        return View();
    }
```

###控制器路由
 - 控制器路由可以避免写重复代码
 - 在操作方法级别指定路由特性，会覆盖控制器级别的
```
    [Route("home/{action}")]
    public class HomeController:Controller
    {
        [Route("home")]
        [Route("home/index")]//因为会覆盖，所以要在写一次home/index
        public ActionResult Index()
        {
            return View();
         }
        public ActionResult Contact()
        {
            return View();
        }
    }
```
 - RoutePrefix
  - 前面的类依然带有重复性，每个路由都是以home开头。通过使用RoutePrefix可以仅在一个地方使用home开头
```
    [RoutePrefix("home")]
    [Route("{action}")]
    public class HomeController : Controller
    {
        [Route("")]
        [Route("index")]
    }
```