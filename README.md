# 期末总结

## 暑假期间

对安卓各方面简单的知识进行了使用，主要就是写代码，每天大概写几百行，看了一部分kotlin，参照《kotlin实战》。



## 开学期间

### 第一部分：自定义View

- 动画篇：
  - 视图动画：补间动画，逐帧动画。
  - 属性动画：ValueAnimator，ObjectAnimator，AnimatorSet，插值器与Evaluator的自定义以及原理。
  - 属性动画进阶：PropertyValuesHolder与Keyframe，ViewPropertyAnimator， 为ViewGroup组件添加动画。
  - 动画进阶：利用PathMeasure实现路径动画，SVG动画。
- 绘图篇：
  - 画笔画布的基本使用。
  - 画布的离层绘制等。
  - Shader着色器。 利用这个完成了放大镜效果。
  - 混合模式。 完成刮刮卡效果。
  - 贝赛尔曲线。 可以完成动态波浪线效果。
- 结合使用： 绘图知识和动画相结合可以完成很多效果，包括控件动画等。



### 第二部分：消息机制

- 消息机制的原理：handler，MessageQueue，looper。

- 常见的问题：

  ###  Android的主线程ActivityThread

  ​	在其中有一个main方法，为整个安卓应用的入口。

  ​	在main方法中调用getMainLooper方法构造一个Looper，并调用loop方法进行消息循环，并且通过Thread的attach方法构造一个binder，通过AMS（服务端）与ApplicationThread间的沟通与ActivityThread进行沟通。

  ​	ActivityThread本身并不是一个线程，只是一个final类，它的Looper所在线程是安卓应用启动时自动创建的进程所构造的。

  ### Android 运行机制

  ​	在ActivityThread中通过loop方法进行消息的循环，所有安卓的动作都是一条消息，包括生命周期的处理等。

  ###  为什么ActivityThread中调用loop方法却没有ANR

  ​	因为一旦loop结束，main方法也随之结束，应用就直接结束，loop中通过不断地循环MessageQueue里的消息，来持续运行。

  ​	但是为什么在主线程中执行耗时任务就会ANR，因为每个动作都是一个消息，最终他们会被作为一个message插入到messageQueue中，并在loop方法中执行，一旦耗时过长，这个事件就阻塞了loop方法，导致ANR。

  ###  内存泄漏

  ​	当一个对象没有引用后，jvm将通过GC回收这个对象，但是如果这个对象没有引用，但是却没有被回收，就称为内存泄漏。

  - 场景： 非静态内部类（匿名内部类）持有外部类的引用。
  - handler引起的内存泄漏：如果直接在活动中构造handler，因为ActivityThread中的handler持有Activity引用，而MessageQueue又持有handler引用，所以MessageQueue相当于持有activity引用，所以在activity销毁时，如果消息队列还有消息未处理，又因为它持有Activity引用，所以无法被GC回收，造成内存泄漏。
  - 解决方法：
    - 自定义静态内部类继承handler。
    - 写一个外部类继承handler。
    - 在OnDestroy中调用handler.removeCallbacksAndMessages方法。



### 第三部分：MVVM模式

- MVVM模式的优点：解耦，同时通过viewmodel管理数据与界面间的通信。
- livedata + databinding：解决了在代码中findviewbyid的操作。
- room：本地数据库存储，可以与LiveData一起使用，更加便捷。
- viewmodel：用于连接View和Model，对逻辑进行管理，使View层专注于界面的处理。
- repository：用作对数据的管理，让viewmodel管理逻辑更方便清晰，防止viewmodel太混乱，导致view与model层没有做到完全隔离。

### 第四部分：设计模式以及工具封装

- 自己是构造了一个游戏场景，对代码的复用性，观赏性都进行了修改，也运用了设计模式。
- 自己封装了一个自定义Toast，Log工具。
- 自己封装网络请求框架，应对不同类型的接口。（未完成）
- 封装了多线程的工具。
- 封装网络状态与权限的工具。（未完成）



### 第五部分：其余部分

- 进程间通信：Sokect编程，内容提供器，AIDL。
- 重新过了一遍第一行代码：四大组件，启动模式等。
- 安卓开发艺术探索：window，进程通信，消息机制，自定义View。
- 数据结构：包括上课的内容，自己也把java的弄了一遍。
- EventBus：了解了实现方法和使用，主要还是利用了消息机制。
- 剩下这几周忙着六级和期末考试，就没有很多进度了。
