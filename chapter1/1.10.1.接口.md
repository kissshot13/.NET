##接口（Interface）
 - API 的意思就是Application Programma Interface（应用程序接口）
 - 接口定义了所有类继承接口时应遵循的语法合同。接口定义了语法合同是“什么”部分。派生类定义了“怎么做”部分。
 - 接口定义了属性、方法、事件和索引器及这四种的组合。但是不能定义字段、变量、静态成员、里面不能用修饰符。
 - 接口可以多继承。类只能单根继承。
 
```
 interface IFlyable
    {
        void fly();
    }
    
    abstract class Bird
    {
       public abstract void Named();

    }
    
    class Sparrow:Bird,IFlyable
    {
        public override void Named()
        {
            Console.WriteLine("我是麻雀");
        }

        public void fly()
        {
            Console.WriteLine("我是麻雀，我可以飞");
        }
    }
    
     class Eagle:Bird,IFlyable
    {
        public override void Named()
        {
            Console.WriteLine("我是老鹰");
        }
       void IFlyable.fly()
        {
            Console.WriteLine("我是老鹰，我会飞");
        }
    }
    
     class Program
    {
        static void Main(string[] args)
        {
            IFlyable[] flys = { new Sparrow(), new Eagle() };
            foreach(IFlyable outfly in flys)
            {
                outfly.fly();
            }
            Console.ReadKey();
        }
    }
```


###显式实现接口
 - 隐式实现接口
   - 既可用接口调用方。也可用具体类调用方法。
 - 显式实现接口
   - 实现接口前不能使用public。必须指定接口的名字
    - `返回值类型`+`接口名称`+`接口方法`
    - 只能通过接口来调用。而不能通过具体的类来做。
 -  显式才是真正的实现方式
 -  当显式隐式一起的时候，隐式就失效了。
 - 实例
 
  

```
 interface IFlyable1
    {
        void fly();
    }
   
   interface IFlyable2
    {
        void fly();
    } 

   class Sparrow:Bird,IFlyable1,IFlyable2
    {
        public override void Named()
        {
            Console.WriteLine("我是麻雀");
        }

        public void fly()//隐式接口
        {
            Console.WriteLine("我是麻雀，我可以飞");
        }
    }  
      
   class Eagle:Bird,IFlyable1,IFlyable2
    {
        public override void Named()
        {
            Console.WriteLine("我是老鹰");
        }
        void IFlyable1.fly()//显式实现接口
        {
            Console.WriteLine("我是老鹰，我会飞(IF1)");
        }
        void IFlyable2.fly()//显式实现接口
        {
            Console.WriteLine("我是老鹰，我会飞(IF2)");
        }

    }
    
   class Program
    {
        static void Main(string[] args)
        {
            Sparrow s1 = new Sparrow();
            s1.fly();//隐式调用
            IFlyable1 e1 = new Eagle();
            e1.fly();//显式调用
            IFlyable2 e2 = new Eagle();
            e2.fly();//显式调用，采用接口
            Console.ReadKey();
        }
    }
```

