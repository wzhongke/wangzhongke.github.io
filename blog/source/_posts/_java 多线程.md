## 术语解释
1. CAS(Compare and Swap, 比较并交换)： 
> 解释： CAS操作需要两个输入，一个旧值和一个新值。在操作期间先比较旧值，如果没有变化，才交换成新值，否则不交换。
> 缺点：CAS会存在ABA的问题。如果一个值原来是A，变成了B，又变成了A，那么使用CAS检查时会发现它的值没有变化。
> 解决方案：在变量前边追加版本号，每次变量更新的时候，把版本号加1，那么A->B->A就会编程1A->2B->3C
2. 原子操作是指不可被中断的一个或一系列操作。

## volatile
volatile变量自身具有下列特性：
1. 可见性：对一个volatile变量的读，总能看到线程对这个volatile变量的最后的写入
2. 原子性：对任意单个volatile变量的读/写具有原子性，但类似volatile++这种复合操作不具备原子性。
过多地使用volatile变量会降低程序执行的效率

# synchronized
 Java中每个对象都可以作为锁，具体有：
1. 对于普通同步方法，锁是当前实例对象
2. 对于静态同步方法，锁是当前的类
3. 对于同步方法块，锁是synchronized括号中的对象
**JVM是基于进入和退出Monitor对象来实现方法同步和代码块同步**

## 锁的升级与对比
在Java SE1.6中，锁有四种状态：**无锁状态->偏向锁状态->轻量级锁状态->重量级锁状态**。这几种锁会随着竞争而升级，但是不能降级。不能降级的策略是为了提高获得锁和释放锁的效率。
### 偏向锁
偏向锁使用了一种等到竞争出现才释放锁的机制，如果有竞争则升级成轻量级锁。
大多数情况下，锁不仅不存在多线程竞争，而且总是由同一线程多次获得。因此为了降低获取锁的代价，引入偏向锁。当同一线程访问同步块并获得锁时，不需要进行CAS操作来加锁和解锁，只需要测试一下对象头的Mark Word里是否存储着指向当前线程的偏向锁。如果测试成功，表示线程获得了锁。
如果测试失败要再测试一下Mark Word中偏向锁的标志是否设置成1（表示当前是偏向锁）：如果没有，则使用CAS竞争；如果设置了，则尝试使用CAS将对象头的偏向锁执行当前线程。
### 轻量级锁
线程在执行同步块之前，JVM会首先在当前线程的栈桢中创建用于存储锁记录的空间，并将对象头中的Mark Word复制到锁记录中。然后线程尝试使用CAS将对象头中的Mark Word替换为指向锁记录的指针。如果成功，当前线程获得锁。如果失败，表示其他线程竞争锁，当前线程尝试使用自旋来获得锁。
轻量级锁释放时，会使用CAS操作将复制到栈桢中的Mark Word替换回对象头，如果成功，表示没有竞争发生；如果失败，表示当前存在竞争，锁就会膨胀成重量级锁。
### 重量级锁
重量级锁状态下，其他线程试图获取锁时，都会被阻塞，当持有锁的线程释放锁之后，会唤醒被阻塞的线程，这些线程就会进行锁的争夺。
|优点|缺点|使用场景
--------------------
| 锁       | 优点           | 缺点  |  使用场景 |
| -------- |-------------| -----|-------|
| 偏向锁    | 加锁和解锁不需要额外消耗，和执行非同步方法存在纳秒级的差距| 如果线程间存在锁竞争会带来额外的锁撤销的消耗 |适用于只有一个线程访问同步块场景
| 轻量级锁  | 竞争的线程不会阻塞，提高了程序的相应速度|如果始终得不到锁竞争的线程，使用自旋会消耗CPU|追求响应时间，同步块执行速度非常快
| 重量级锁  | 线程竞争不使用自旋，不会消耗CPU |线程阻塞，响应时间慢|追求吞吐量，，同步块执行速度较长

## 线程优先级
在Java线程中，通过一个整型成员变量`priority`来控制优先级，范围是从1-10，默认优先级是5。一般情况下优先级高的线程分配时间片的数量要多于优先级低的线程。
针对频繁阻塞的线程需要设置的优先级较高，而需要较多CPU时间的线程，要设置较低的优先级，确保处理器不会被独占。
```
public class Priority {
    private static volatile boolean notStart = true;
    private static volatile boolean notEnd = true;

    public static void main(String [] args) throws InterruptedException {
        List<Job> jobs = new ArrayList<>();
        IntStream.range(0, 10).forEach(item -> {
            int priority = item < 5 ? Thread.MIN_PRIORITY : Thread.MAX_PRIORITY;
            Job job = new Job(priority);
            jobs.add(job);
            Thread thread = new Thread(job, "Thread-" + item);
            thread.setPriority(priority);
            thread.start();
        });
        notStart = false;
        TimeUnit.SECONDS.sleep(10);
        notEnd = false;
        jobs.forEach(j ->
            System.out.println("job priority:" + j.priority + " count:" + j.jobCount)
        );
    }

    static class Job implements Runnable {
        private int priority;
        private long jobCount;
        Job(int priority) {
            this.priority = priority;
        }

        public void run() {
            while (notStart) {
                Thread.yield();
            }

            while (notEnd) {
                Thread.yield();
                jobCount ++;
            }
        }
    }
}
```
运行该实例，可以看到优先级1和10有明显的差距，说明优先级生效了。但是程序正确性不能依赖线程优先级的高低，因为有些操作系统会忽略Java设定的优先级。
```
job priority:1 count:195451
job priority:1 count:195475
job priority:1 count:195460
job priority:1 count:195443
job priority:1 count:195462
job priority:10 count:3726352
job priority:10 count:3729393
job priority:10 count:3710086
job priority:10 count:3733771
job priority:10 count:3731136
```
## Daemon线程
Daemon线程是一种支持型线程，因为它主要被用作后台调度以及支持性工作。当一个Java虚拟机中不存在非Daemon线程时，Java虚拟机将会退出。通过调用`Thread.setDaemon(true)`将线程设置为Daemon线程。
> Daemon属性需要在启动线程之前设置才有效

