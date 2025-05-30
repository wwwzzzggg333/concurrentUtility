# concurrentUtility
 
## LatchUtils
- 对多线程并行同步CountDownLatch进行封装，让线程并发等待更优雅

_________________
以一个常见的功能为例：多个线程并行处理，主线程等待所有线程处理完成再继续执行。
_________________
传统使用CountDownLatch方法如下
```java
//创建线程并行处理parm1和parm2，主线程等待两个线程处理完成再继续执行
void yourSubmitTask(Object parm1, Object parm2) {
        CountDownLatch countDownLatch = new CountDownLatch(2);
        executorService.execute(() -> {
        try {
        log.info("task1");
        //process parm1
        } finally {
        countDownLatch.countDown();
        }
        });

        executorService.execute(() -> {
        try {
        log.info("task2");
        //process parm2
        } finally {
        countDownLatch.countDown();
        }
        });

        try {
        countDownLatch.await(10, TimeUnit.SECONDS);
        } catch (InterruptedException e) {

        }

        //go on do something
        }
```

_________________
使用LatchUtils方法如下
```java
//创建线程并行处理parm1和parm2，主线程等待两个线程处理完成再继续执行
    void mySubmitTask(Object parm1, Object parm2) {
        LatchUtils.submitTask(executorService, () -> {
            log.info("task1");
            //process parm1
        });

        LatchUtils.submitTask(executorService, () -> {
            log.info("task2");
            //process parm2
        });

        LatchUtils.waitFor(10L, TimeUnit.SECONDS);
        //go on do something
    }
```
_________________
从上代码可以看出使用LatchUtils 工具类可以有效的隐藏在多线程中操作线程并发对象的代码，减少线程同步的误操作，让代码更优雅。

_________________
This is a simple example of how to use the LatchUtils class to wait for a set number of tasks to complete before continuing with the rest of the program. The LatchUtils class uses a CountDownLatch to track the number of tasks that have completed, and a Waiter to wait for the latch to count down to zero. The submitTask method is used to submit a task to the executor service, which will execute the task asynchronously. The waitFor method is used to wait for a specified number of tasks to complete.

