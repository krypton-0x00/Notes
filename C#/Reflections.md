```cs
namespace Test {
    class Program {
        static void Main(string[] args) {
            Console.WriteLine("Hello, World!");

            Test t1 = new Test();
            
            MethodInfo mi = typeof(Test).GetMethod("function");
            mi.Invoke(t1, new object[]{1});

        }
    }

    class Test{
        public static int function(int x){
            Console.WriteLine(x);
            return x;
        }
    }

}

```