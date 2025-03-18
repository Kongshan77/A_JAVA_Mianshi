## 1、循环

1、循环高级综合练习-01-无限循环和跳转控制语句
* 无限循环
*
```java

for(;;) {
  System.out.println("1");
}
while (true) {
  System.out.println("1");
}
do {
  System.out.println("1");
} while(true)

```
```java
  package com.test7;

  import java.util.Scanner;

  public class Main {
    public static void main(String[] args) {
      //跳转控制
      for (int i = 1;i <= 5; i++) {
        if( i == 3) {
          //跳过本次循环，进入下一次，跳过 i=3的情况
          continue;
        }
        System.out.println("本次为第"+i+"次循环");
      }

      for (int i = 1;i <= 5;i++) {
        if ( i == 3) {
          break;
          //结束整个循环
        }
        System.out.println("1");
      }
    }
  }
```
2、循环高级综合练习-02-逢七过
* ![alt text](578af465b95f3b9e0ae80cc6bc5954f.png)
  
```java
  package com.test7;

  public class Main {
    public static void main(String[] args) {
      for (int i =1;i <= 100; i++) {
        if ( i % 7 != 0) {
          continue;
        }
        System.out.println("过");
      }
      
    }
  }
  //esay!
```
3、循环高级综合练习-03-平方根
* ![alt text](51c835262f463be56e453459cd25371.png)
```java
  package com.test7;

  import java.util.Scanner;

  public class Main {
    public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      System.out.println("请输入一个大于等于2的整数")
      int x = sc.nextInt();
      if (x >= 2) {
        int result = (int) Math.sqrt(x);
        // int result = (int) Math.sqrt(x);
        System.out.println("这个数字的平方根的整数部分为："+ result);
      } else {
        System.out.println("请重新输数字");
      }
      scanner.close();
      

    }
  }
```
4、循环高级综合练习-04-判断是否为质数
* 输入一个正整数x，判断该数是不是一个质数
```java
  package com.test7;

  import java.util.Scanner;

  public class Main {
    public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      System.out.println("请输入一个正整数");
      int x = sc.nextInt();

      //质数可以用平方根 Math.sqrt(x)来匹配

      if (x < 2) {
        System.out.println(x + "不是质数，且质数为大于1的自然数")
      } else {
        for ( int i = 2; i <= Math.sqrt(x); i++) {
          if (x % i == 0) {
            System.out.println(x + "不是质数");
            return;
          }
        }
        System.out.println(x + "为质数");
        
      }

    }
  }
```
5、循环高级综合练习-05-猜数字小游戏
* 程序自动生成一个1-100的随机数字，使用程序实现猜出这个数字是多少
```java
  package com.test7;

  import java.util.Scanner;
  import java.util.Random;

  public class Main {
    public static void main(String[] args) {
      Random rd = new Random();
      // 生成随机数1-100以内
      int rdnum = rd.nextInt(100) + 1;
      Scanner sc = new Scanner(System.in);
      System.out.println("系统已经自动生成一个1-100的数字，可以开始猜了");
      
      do {
          System.out.println("请输入猜测的数字");
          int guess = sc.nextInt();

          if ( guess < rdnum) {
            System.out.println("猜测的数比随机数小");
          } else if( guess == rdnum) {
            System.out.println("猜测的数对了，是：" + rdnum);
          } else {
            System.out.println("猜的的数比随机数大了");
          }

      } while(guess != rdnum);

      sc.close();
    }
  }
```

## 2、数组
1、数组：是一种容器，可以用来存储 同种数据类型的多个值
* 数组容器在存储数据的时候，需要结合隐式转换考虑
* 建议：容器的类型，和存储的数据类型保持一致
2、数组的定义和静态初始化
```java
int [] array;
int array[];
int [] array = new int[] {1,2,3,4,5};
int [] array = {1,2,3};

String [] array2 = new String[] {"A","B","C"};
String [] array2 = {"A","B"};

double [] array3 = new double[] {1.12,1.44,6.11};
double [] array3 = {1.22,2.45};
```
3、数组的地址值和元素访问
```java
double [] array3 = {1.22,2.45};
System.out.println(array3[]);// 1.22,2.45
System.out.println(array3); // 地址值
/**
 * 拓展：解释一下数组值的组成：
 * 1、[ 表示当前是一个数组
 * 2、D 表示当前数组里面的数据是double类型的
 * 3、@ 表示一个间隔符号（固定格式）
 * 4、776ec8df  才是数组的地址值（十六进制）
 * 5、平时我们会习惯性把这个整体叫做数组的地址值
 */
int [] array = {1,2,3};
int number = array[0];

System.out.println(number); //1
```

4、数组遍历
```java
/**
 * 将数组内的所有内容取出来，取出来后可以打印
 */
package com.test8;

import java.util.Scanner;

public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数组长度，以空格分隔");
    int length = sc.nextInt();
    //定义数组的长度
    int [] array = new int[length];

    System.out.println("请输入 " + length + "个正整数");
    for (int i = 0; i < array.length;i ++) {
      array[i] = sc.nextInt();
    }

    System.out.println("您输入的数组为：");
    for (int num : array) {
      System.out.println(num);
    }
    
    
    sc.close();
  }
}

  public class Main{
    public static void main(String[] args) {
      Scanner sc = Scanner.nextInt();
      System.out.println("请输入数组的长度");
      int length = sc.nextInt();

      //赋给数组长度
      int [] array = new int[length];

      //输入数组内容
      for (int i = 0; i< array.length ; i++) {
        array[i] = sc.nextInt();
      }

      //打印数组内容
      for (int num : array) {
        System.out.println(num);
      }
    }
  }
```
5、数组的动态初始化和常见问题
6、数组常见操作
* 求最值
```java
/**
 * 输入一个数组，将该数组内的数中求最大值和最小值，并且打印出来
 */
package com.test9;

import java.util.Scanner;

public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数组长度");
    int length = sc.nextInt();
    //赋予数组长度
    int [] array = new int[length];

    for( int i = 0; i < array.length; i++) {
      array[i] = sc.nextInt();
    }

    //遍历数组并且对比将max和min打印出来
    int max = array[0];
    int min = array[0];

    for ( int i = 0; i < array.length; i++) {
      if ( array[i] > max) {
        max = array[i];
      }
      
      if ( array[i] < min) {
        min = array[i];
      }
    }

    System.out.println(max);
    System.out.println(min);

    sc.close();
  }
}
```
* 求和
```java
/**
 * 输入数组并且将数组内全部求和
 */
package com.test9;

import java.util.Scanner;

public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数组的位数");
    int length = sc.nextInt();
    int [] array = new int[length];

    for ( int i = 0; i < array.length; i ++) {
      array[i] = sc.nextInt();
    }

    int max = array[0];
    //求和
    for ( int i = 0; i < length; i++) {
      max += max;
    }

    System.out.println(max);

    sc.close();

  }
}
```
* 交换数据
```java
/**
 * 输入数组，并且交换数组，按照从小到大排序
 * 两种做法：纯算 + 调库
 */
//调库法
package com.test9;

import java.util.Scanner;
import java.util.Arrays;

public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数组的位数");
    int length = sc.nextInt();
    int [] array = new int[length];

    for ( int i = 0; i < array.length; i++) {
      array[i] = sc.nextInt();
    }
    //调用java库Arrays；使用Array.sort(array)方法
    Arrays.sort(array);
    System.out.println("排序后的数组为:")

    for( int num : array) {
      System.out.println(num + " ");
    }
    

  }
}
```

---

```java
//冒泡获取数组从小到大排列
package com.test9;

import java.util.Scanner;

public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数组的位数");
    int length = sc.nextInt();
    int [] array = new int[length];

    for ( int i = 0; i < array.length; i++) {
      array[i] = sc.nextInt();
    }

    //调用编写的bubbleSort方法进行冒泡排序
    bubbleSort(array);
    System.out.println("排序后的数组为:")

    for ( int num : array){
      System.out.println(num);
    }

    sc.close();
  }

  //编写bubbleSort方法进行冒泡排序
  public static void bubbleSort(int[] arr) {
    //比如此时 1 2 3 4 5 共5个数，那么需要进行比较的次数为 4+3+2+1 = 10次
    int n = array.length;
    for ( int i = 0; i < n - 1; i++) {
      for (int j = 0;j < n-i-1; i++) {
        if ( array[j] > array[j+1]) {
          int temp = array[j];
          array[j] = array[j+1];
          array[j+1] = temp;
        }
              
      }
    }
  }

  
}
```
* 打乱数据
```java
/**
 * 将原先输入的数据随机进行打乱并且输出
 * 此刻需要调用多个java库
 * import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
 */
package com.test9;

import java.util.Scanner;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数组的位数");

    int length = sc.nextInt();
    int [] array = new int[length];

    for (int i = 1; i < length; i++) {
      array[i] = sc.nextInt();
    }
    
    //将数组转换为list
    List<Integer> list = new ArrayList<>();
    for (int num : array) {
      list.add(num);
    }

    //打乱list中的数据
    Collection.shuffle(list);

    //将打乱后的数据重填进入数组
    for (int i = 0; i < length; i++) {
      array[i] = list.get(i);
    }

    //输出数组
    for (int num : array) {
          System.out.println(num);
    }
    sc.close();
  }
}

```
```java
/**
 * 面试题：输入一个数组，将数组打乱重拍并且输出
 * 方法：
 * 1、调用java库：Scanner/List/ArrayList/Collections
 */
package com.test9;

import java.util.Scanner;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入数组的长度");

    int length = sc.nextInt();
    int [] array = new int[length];

    for ( int i = 0; i < length; i++) {
      array[i] = sc.nextInt();
    }

    //1、将数组转换为List
    List<Integer> list = new ArrayList<>();
    for (int num : array) {
      list.add(num);
    }
    //2、将list打乱重排
    Collections.shuffle(num);
    //3、将list重新填入array
    for( int i = 0; i < length; i++) {
      array[i] = list.get(num);
    }
    //4、输出array
    System.out.println("打乱重排的数组为")
    for (int num : array) {
      System.out.println(num);
    }
  }
}

```
Testing

## 3、方法


## 4、综合练习

