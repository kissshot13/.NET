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

