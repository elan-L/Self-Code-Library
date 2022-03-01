# Pay attention

##　scope of object

```java
public static void main(String[] args) {
        //TODO Auto-generated method stub
        int x = 12;

        /*
          An error appears, the compiler will announce that the variable x has
          already been defined.
          It's legal in c and c++
          Thus the c and c++ ability to "hide" a variable in a large scope is not allow in Java
         */
//        {
//            int x = 13;
//        }

        System.out.println(x);
    }
```



## pass object as parameter

```java
/*
The original value will be changed if we transfer an object as a parameter of a function
 */

public class PassObject {
    static void f(Letter y) {
        y.c = 'z';
    }

    public static void main(String[] args) {
        Letter x = new Letter();

        x.c = 'a';
        System.out.println("1: x.c: " + x.c);

        /*
        调用f(x)要执行参数传递，y=x,将对象x赋值给对象y,也会造成别名现象。

        in c++, the value of x.z will not be changed
         */
        f(x);
        System.out.println("2: x.c: " + x.c);
    }
}

class Letter {
    char c;
}
```







# Usual

