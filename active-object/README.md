---
title: Active Object
category: Concurrency
language: en
tag:
 - Performance
---


## Intent
The active object design pattern decouples method execution from method invocation for objects that each reside in their thread of control. The goal is to introduce concurrency, by using asynchronous method invocation, and a scheduler for handling requests.

活动对象设计模式将每个驻留在其控制线程中的对象的方法执行与方法调用解耦。目标是通过使用异步方法调用和处理请求的调度器来引入并发性。

## Explanation

The class that implements the active object pattern will contain a self-synchronization mechanism without using 'synchronized' methods.

Real-world example

>The Orcs are known for their wildness and untameable soul. It seems like they have their own thread of control based on previous behavior.

To implement a creature that has its own thread of control mechanism and expose its API only and not the execution itself, we can use the Active Object pattern.


**Programmatic Example**

```java
public abstract class ActiveCreature{
  private final Logger logger = LoggerFactory.getLogger(ActiveCreature.class.getName());

  private BlockingQueue<Runnable> requests;
  
  private String name;
  
  private Thread thread;

  public ActiveCreature(String name) {
    this.name = name;
    this.requests = new LinkedBlockingQueue<Runnable>();
    thread = new Thread(new Runnable() {
        @Override
        public void run() {
          while (true) {
            try {
              requests.take().run();
            } catch (InterruptedException e) { 
              logger.error(e.getMessage());
            }
          }
        }
      }
    );
    thread.start();
  }
  
  public void eat() throws InterruptedException {
    requests.put(new Runnable() {
        @Override
        public void run() { 
          logger.info("{} is eating!",name());
          logger.info("{} has finished eating!",name());
        }
      }
    );
  }

  public void roam() throws InterruptedException {
    requests.put(new Runnable() {
        @Override
        public void run() { 
          logger.info("{} has started to roam the wastelands.",name());
        }
      }
    );
  }
  
  public String name() {
    return this.name;
  }
}
```

We can see that any class that will extend the ActiveCreature class will have its own thread of control to invoke and execute methods.

For example, the Orc class:

```java
public class Orc extends ActiveCreature {

  public Orc(String name) {
    super(name);
  }

}
```

Now, we can create multiple creatures such as Orcs, tell them to eat and roam, and they will execute it on their own thread of control:

```java
  public static void main(String[] args) {  
    var app = new App();
    app.run();
  }
  
  @Override
  public void run() {
    ActiveCreature creature;
    try {
      for (int i = 0;i < creatures;i++) {
        creature = new Orc(Orc.class.getSimpleName().toString() + i);
        creature.eat();
        creature.roam();
      }
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      logger.error(e.getMessage());
    }
    Runtime.getRuntime().exit(1);
  }
```

## Class diagram

![alt text](./etc/active-object.urm.png "Active Object class diagram")

## Tutorials

* [Android and Java Concurrency: The Active Object Pattern](https://www.youtube.com/watch?v=Cd8t2u5Qmvc)