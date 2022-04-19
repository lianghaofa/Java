1.进程

2.线程

3.程序

QQ.EXE -> loaded in memory 

3.1 多次开启QQ.EXE，一个QQ对应一个进程。可以限制一个程序一个进程。
  资源分配的基本单位。
3.2 程序开启进程后，启动一个主线程执行任务。
  执行的基本单位-线程
  
  ![7a630739f720944f30c7c30f06264ef](https://user-images.githubusercontent.com/24481784/163958247-839490b4-a44d-4add-895a-7c3750a41406.png)
  
4.create threads in java  
4.1
    static class MyThreads extends Thread {
        @Override
        public void run() {
            System.out.println("MyThreads");
        }
    }
    
    static class MyRun implements Runnable{
        @Override
        public void run() {
            System.out.println("MyRun");
        }
    }
    
    static class MyCall implements Callable<String> {
        @Override
        public String call() throws Exception {
            System.out.println("MyCall");
            return "success";
        }
    }
    
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 0
        new MyThreads().start();
        // 1
        new Thread(new MyRun()).start();
        // 2
        new Thread(()->{
            System.out.println("lam");
        }).start();
        
        // 3   FutureTask    Future Callable
        FutureTask<String> task = new FutureTask<String>(new MyCall());
        Thread t = new Thread(task);
        t.start();
        // success
        System.out.println("FutureTask<String> = " + task.get());

        // 4
        ExecutorService service = Executors.newCachedThreadPool();
        service.execute(()->{
            System.out.println("ExecutorService");
        });
        Future<String> f = service.submit(new MyCall());
        String s = f.get();
        // success
        System.out.println(s);
        service.shutdown();
    }
    
   本质都是 new Thread()
   
5 java的6种线程状态  
NEW – a newly created thread that has not yet started the execution

RUNNABLE – either running or ready for execution but it's waiting for resource allocation

BLOCKED – waiting to acquire a monitor lock to enter or re-enter a synchronized block/method

WAITING – waiting for some other thread to perform a particular action without any time limit

TIMED_WAITING – waiting for some other thread to perform a specific action for a specified period

TERMINATED – has completed its execution

![Life_cycle_of_a_Thread_in_Java](https://user-images.githubusercontent.com/24481784/163965686-74d57c46-c591-49c0-ba51-dc57d321ac77.jpg)

  
6 how to stop a thread.

        Thread t = new Thread(()->{
            while (true){
                System.out.println("go on");
            }
        });

        t.start();
        // not recommend.   it will release everything including lock,data inconsistent...it will influence others.
        t.stop();
        
        // not recommend.   it will not release everything ...it will cause dead lock ,data inconsistent  ?
        t.suspend();
        t.resume();


    //  set a flag
    //  volatile
    private static volatile boolean running = true;

    public static void main(String[] args) throws ExecutionException, InterruptedException {

        Thread t = new Thread(()->{
            while (running){

                System.out.println("go on");
            }
        });
        t.start();
        // but you don't when it will stop.
        running = false;
    }
    //  interrupt
    public static void main(String[] args) throws ExecutionException, InterruptedException {

        Thread t = new Thread(()->{
            while (!Thread.interrupted()){

                System.out.println("go on");
            }
        });
        t.start();
        // but you don't when it will stop.
        t.interrupt();
    }

