并发编程三大特性 ： 可见性，有序性，原子性

可见性：volatile  缓存与主存    volatile  缓存与内存保持一致性

     private static boolean running = true; this can't stop.
    //private static volatile boolean running = true;  this can stop.
    
    
    private static void m(){
        System.out.println("m start");
        while (running){

        }
        System.out.println("m end");
    }

    public static void main(String[] args) {
        new Thread(HelloVolatile::m, "t1").start();

        running = false;
    }
    
    // 修改对象时，只对引用对象的可见性，不对对象的内置值起效。
    
    A a1 = new A();
    
    volatile A a = new A();
    
    // it work.
    a = a1;
    
    // it does't work.
    a.val = 1;
    
    
    
    
    
    
