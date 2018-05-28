在Java中， `final`关键字可以用来修饰类、方法和变量（包括成员变量和局部变量）。<br>
比较详细的总结[《浅析Java中的final关键字》](http://www.cnblogs.com/dolphin0520/p/3736238.html)，其中比较容易出错的是`final`与`static`的比较。<br>
作者举例中使用对象来访问`static`变量，违反了[《Java编码规范》](https://yq.aliyun.com/articles/69327)，容易造成混淆，更直接的应该用类名直接访问`static`变量。
<br>
另外,注意`final`定义的String变量与普通变量也有一定的区别，这一点以前也没有意识到。
```
public class Test {
    public static void main(String[] args)  {
        String a = "hello2"; 
        final String b = "hello";
        String d = "hello";
        String c = b + 2; 
        String e = d + 2;
        System.out.println((a == c));
        System.out.println((a == e));
    }
}
```
`==`比较的是两个String对象的地址。
