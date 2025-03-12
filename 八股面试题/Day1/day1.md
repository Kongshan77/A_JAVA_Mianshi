## 此文件为JAVA实习面试所用八股

该文件为bilibili地址：https://www.bilibili.com/video/BV1Ag4y1A7bg?buvid=XX6EF3B6E4F7132F9EB7C940551F4DB87DE12&from_spmid=playlist.playlist-detail.0.0&is_story_h5=false&mid=psB79T810OYGeAyXAiTpIQ%3D%3D&plat_id=116&share_from=ugc&share_medium=android&share_plat=android&share_session_id=0455e81e-f406-433b-baf7-b338fcefa7d6&share_source=WEIXIN&share_tag=s_i&spmid=main.ugc-video-detail.0.0&timestamp=1727512080&unique_k=hn1LRou&up_id=615412417

---
Day1
---
1、JAVA中有哪几种方式创建线程执行任务

(1)继承Thread类

``` JAVA
public class Thread1 extends Thread {

  public static void main(String[] args){
    Thread1 thread = new Thread1();
    thread.start();
  }

  @Override
  public void run(){
    System.out.println("hello")
  }
}
```

**1/重写的是run()方法，而不是start方法，但是占用了继承的名额，JAVA中的类是单继承**

**2/但是接口可以多继承**

(2)实现Runnable接口

``` JAVA
public class Thread1 implements Runnable {
  public class void main(String[] args) {
    Thread  thread = new Thread(new Thread1()) ;
    thread.start;
  }
  public void run(){
    System.out.Println("hello");
  }
}

```

(3)

(4)
