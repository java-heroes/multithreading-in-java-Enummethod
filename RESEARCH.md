Kaynaklar:
java, çok iş parçacıklı bir programlama dilidir , yani Java kullanarak çok iş parçacıklı program geliştirebiliriz. Çok iş parçacıklı bir program, aynı anda çalışabilen iki veya daha fazla parça içerir ve her bölüm, özellikle bilgisayarınızda birden fazla CPU'ya sahip olduğunda, mevcut kaynakları en iyi şekilde kullanarak aynı anda farklı bir görevi yerine getirebilir.

Tanım olarak: çoklu görev, birden çok işlemin CPU gibi ortak işlem kaynaklarını paylaştığı zamandır. Çoklu iş parçacığı, çoklu görev fikrini, tek bir uygulama içindeki belirli işlemleri tek tek iş parçacıklarına bölebileceğiniz uygulamalara genişletir. İş parçacıklarının her biri paralel olarak çalışabilir. İşletim sistemi, işlem süresini yalnızca farklı uygulamalar arasında değil, aynı zamanda bir uygulama içindeki her bir iş parçacığı arasında da böler.

Çoklu iş parçacığı, aynı programda birden fazla etkinliğin aynı anda ilerleyebileceği bir şekilde yazmanıza olanak tanır.

Bir Threadin Yaşam Döngüsü
Bir Thread, yaşam döngüsünde çeşitli aşamalardan geçer. Örneğin, bir iş parçacığı doğar, başlar, çalışır ve sonra ölür. Aşağıdaki şema, bir iş parçacığının tüm yaşam döngüsünü göstermektedir.

Java Konusu
Yaşam döngüsünün aşamaları aşağıdadır -

Yeni - Yeni bir iş parçacığı yaşam döngüsüne yeni durumda başlar. Program iş parçacığını başlatana kadar bu durumda kalır. Aynı zamanda doğuştan iplik olarak da adlandırılır .

Çalıştırılabilir - Yeni doğan bir iş parçacığı başlatıldıktan sonra iş parçacığı çalıştırılabilir hale gelir. Bu durumdaki bir iş parçacığının görevini yürüttüğü kabul edilir.

Bekliyor – Bazen, bir iş parçacığı başka bir iş parçacığının bir görevi gerçekleştirmesini beklerken bir iş parçacığı bekleme durumuna geçer. Bir iş parçacığı, yalnızca başka bir iş parçacığı bekleyen iş parçacığına yürütmeye devam etmesi için sinyal verdiğinde çalıştırılabilir duruma geri döner.

Zamanlanmış Bekleme - Çalıştırılabilir bir iş parçacığı, belirli bir zaman aralığı için zamanlanmış bekleme durumuna girebilir. Bu durumdaki bir iş parçacığı, bu zaman aralığı sona erdiğinde veya beklediği olay gerçekleştiğinde çalıştırılabilir duruma geri döner.

Sonlandırıldı (Ölü) - Çalıştırılabilir bir iş parçacığı, görevini tamamladığında veya başka bir şekilde sonlandırıldığında sonlandırılmış duruma girer.

Konu Öncelikleri
Her Java iş parçacığının, işletim sisteminin iş parçacıklarının zamanlandığı sırayı belirlemesine yardımcı olan bir önceliği vardır.

Java iş parçacığı öncelikleri, MIN_PRIORITY (1 sabiti) ile MAX_PRIORITY (10 sabiti) arasındaki aralıktadır. Varsayılan olarak, her iş parçacığına öncelik NORM_PRIORITY (5 sabiti) verilir.

Daha yüksek önceliğe sahip iş parçacıkları bir program için daha önemlidir ve düşük öncelikli iş parçacıklarından önce işlemci zamanı tahsis edilmelidir. Ancak, iş parçacığı öncelikleri, iş parçacıklarının yürütülme sırasını garanti edemez ve platforma çok bağlıdır.

Çalıştırılabilir Bir Arayüz Uygulayarak Bir Konu Oluşturun
Sınıfınızın bir iş parçacığı olarak yürütülmesi amaçlanıyorsa, bunu bir Runnable arabirimi uygulayarak başarabilirsiniz . Üç temel adımı izlemeniz gerekecek -

Aşama 1
İlk adım olarak, Runnable arabirimi tarafından sağlanan bir run() yöntemini uygulamanız gerekir . Bu yöntem, iş parçacığı için bir giriş noktası sağlar ve tüm iş mantığınızı bu yöntemin içine koyacaksınız. Aşağıda, run() yönteminin basit bir sözdizimi verilmiştir -

public void run( )
Adım 2
İkinci adım olarak, aşağıdaki yapıcıyı kullanarak bir Thread nesnesinin örneğini oluşturacaksınız -

Thread(Runnable threadObj, String threadName);
Burada threadObj , Runnable arabirimini uygulayan bir sınıfın örneğidir ve threadName yeni iş parçacığına verilen addır.

Aşama 3
Bir Thread nesnesi oluşturulduktan sonra, onu run() yöntemine bir çağrı yürüten start() yöntemini çağırarak başlatabilirsiniz . Start() yönteminin basit bir sözdizimi aşağıdadır -

void start();
Örnek
İşte yeni bir iş parçacığı oluşturan ve onu çalıştırmaya başlayan bir örnek -

Canlı Demo
class RunnableDemo implements Runnable {
   private Thread t;
   private String threadName;
   
   RunnableDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }
   
   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // Let the thread sleep for a while.
            Thread.sleep(50);
         }
      } catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }
   
   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
}

public class TestThread {

   public static void main(String args[]) {
      RunnableDemo R1 = new RunnableDemo( "Thread-1");
      R1.start();
      
      RunnableDemo R2 = new RunnableDemo( "Thread-2");
      R2.start();
   }   
}
Bu, aşağıdaki sonucu üretecektir -

Çıktı
Creating Thread-1
Starting Thread-1
Creating Thread-2
Starting Thread-2
Running Thread-1
Thread: Thread-1, 4
Running Thread-2
Thread: Thread-2, 4
Thread: Thread-1, 3
Thread: Thread-2, 3
Thread: Thread-1, 2
Thread: Thread-2, 2
Thread: Thread-1, 1
Thread: Thread-2, 1
Thread Thread-1 exiting.
Thread Thread-2 exiting.
Bir Konu Sınıfını Genişleterek Bir Konu Oluşturun
Bir iş parçacığı oluşturmanın ikinci yolu , aşağıdaki iki basit adımı kullanarak Thread sınıfını genişleten yeni bir sınıf oluşturmaktır . Bu yaklaşım, Thread sınıfında mevcut yöntemler kullanılarak oluşturulan birden çok iş parçacığının işlenmesinde daha fazla esneklik sağlar.

Aşama 1
Thread sınıfında bulunan run() yöntemini geçersiz kılmanız gerekecek . Bu yöntem, iş parçacığı için bir giriş noktası sağlar ve tüm iş mantığınızı bu yöntemin içine koyacaksınız. Aşağıda, run() yönteminin basit bir sözdizimi verilmiştir -

public void run( )
Adım 2
Thread nesnesi oluşturulduktan sonra, run() yöntemine bir çağrı yürüten start() yöntemini çağırarak başlatabilirsiniz . Start() yönteminin basit bir sözdizimi aşağıdadır -

void start( );
Örnek
İş parçacığını genişletmek için yeniden yazılan önceki program -

Canlı Demo
class ThreadDemo extends Thread {
   private Thread t;
   private String threadName;
   
   ThreadDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }
   
   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // Let the thread sleep for a while.
            Thread.sleep(50);
         }
      } catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }
   
   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
}

public class TestThread {

   public static void main(String args[]) {
      ThreadDemo T1 = new ThreadDemo( "Thread-1");
      T1.start();
      
      ThreadDemo T2 = new ThreadDemo( "Thread-2");
      T2.start();
   }   
}
Bu, aşağıdaki sonucu üretecektir -

Çıktı
Creating Thread-1
Starting Thread-1
Creating Thread-2
Starting Thread-2
Running Thread-1
Thread: Thread-1, 4
Running Thread-2
Thread: Thread-2, 4
Thread: Thread-1, 3
Thread: Thread-2, 3
Thread: Thread-1, 2
Thread: Thread-2, 2
Thread: Thread-1, 1
Thread: Thread-2, 1
Thread Thread-1 exiting.
Thread Thread-2 exiting.
Konu Yöntemleri
Thread sınıfında bulunan önemli metotların listesi aşağıdadır.

Sr.No.	Yöntem & Açıklama
1	
genel boşluk başlangıcı()

İş parçacığını ayrı bir yürütme yolunda başlatır, ardından bu Thread nesnesinde run() yöntemini çağırır.

2	
genel geçersiz çalıştırma()

Bu Thread nesnesi ayrı bir Runnable hedefi kullanılarak başlatıldıysa, o Runnable nesnesinde run() yöntemi çağrılır.

3	
public final void setName(Dize adı)

Thread nesnesinin adını değiştirir. Adı almak için bir getName() yöntemi de vardır.

4	
public final void setPriority(int öncelik)

Bu Thread nesnesinin önceliğini ayarlar. Olası değerler 1 ile 10 arasındadır.

5	
public final void setDaemon(boolean açık)

true parametresi bu Konuyu bir arka plan programı dizisi olarak belirtir.

6	
public final void birleştirme(uzun milisaniye)

Geçerli iş parçacığı, bu yöntemi ikinci bir iş parçacığında çağırır ve mevcut iş parçacığının, ikinci iş parçacığı sona erene veya belirtilen milisaniye sayısı geçene kadar engellenmesine neden olur.

7	
genel boşluk kesme()

Bu iş parçacığını keser ve herhangi bir nedenle engellenmişse yürütmeye devam etmesine neden olur.

8	
public final boolean isAlive()

İş parçacığı canlıysa, yani iş parçacığı başlatıldıktan sonra ancak tamamlanmadan önceki herhangi bir zamanda doğru döndürür.

Önceki yöntemler belirli bir Thread nesnesinde çağrılır. Thread sınıfındaki aşağıdaki yöntemler statiktir. Statik yöntemlerden birini çağırmak, o anda çalışan iş parçacığında işlemi gerçekleştirir.

Sr.No.	Yöntem & Açıklama
1	
genel statik boşluk verimi()

Şu anda çalışmakta olan iş parçacığının, programlanmayı bekleyen aynı önceliğe sahip diğer iş parçacıklarına vermesine neden olur.

2	
genel statik boşluk uykusu (uzun milisaniye)

Şu anda çalışan iş parçacığının en az belirtilen milisaniye sayısı kadar engellenmesine neden olur.

3	
genel statik boolean holdLock(Object x)

Geçerli iş parçacığı verilen Nesne üzerindeki kilidi tutuyorsa true döndürür.

4	
genel statik Konu currentThread()

Bu yöntemi çağıran iş parçacığı olan, şu anda çalışan iş parçacığına bir başvuru döndürür.

5	
genel statik boşluk dumpStack()

Çok iş parçacıklı bir uygulamada hata ayıklarken yararlı olan, çalışmakta olan iş parçacığı için yığın izini yazdırır.

Örnek
Aşağıdaki ThreadClassDemo programı, Thread sınıfının bu yöntemlerinden bazılarını gösterir. Runnable'ı uygulayan bir DisplayMessage sınıfı düşünün -

// File Name : DisplayMessage.java
// Create a thread to implement Runnable

public class DisplayMessage implements Runnable {
   private String message;
   
   public DisplayMessage(String message) {
      this.message = message;
   }
   
   public void run() {
      while(true) {
         System.out.println(message);
      }
   }
}
Thread sınıfını genişleten başka bir sınıf aşağıdadır -

// File Name : GuessANumber.java
// Create a thread to extentd Thread

public class GuessANumber extends Thread {
   private int number;
   public GuessANumber(int number) {
      this.number = number;
   }
   
   public void run() {
      int counter = 0;
      int guess = 0;
      do {
         guess = (int) (Math.random() * 100 + 1);
         System.out.println(this.getName() + " guesses " + guess);
         counter++;
      } while(guess != number);
      System.out.println("** Correct!" + this.getName() + "in" + counter + "guesses.**");
   }
}
Yukarıda tanımlanan sınıfları kullanan ana program aşağıdadır -

// File Name : ThreadClassDemo.java
public class ThreadClassDemo {

   public static void main(String [] args) {
      Runnable hello = new DisplayMessage("Hello");
      Thread thread1 = new Thread(hello);
      thread1.setDaemon(true);
      thread1.setName("hello");
      System.out.println("Starting hello thread...");
      thread1.start();
      
      Runnable bye = new DisplayMessage("Goodbye");
      Thread thread2 = new Thread(bye);
      thread2.setPriority(Thread.MIN_PRIORITY);
      thread2.setDaemon(true);
      System.out.println("Starting goodbye thread...");
      thread2.start();

      System.out.println("Starting thread3...");
      Thread thread3 = new GuessANumber(27);
      thread3.start();
      try {
         thread3.join();
      } catch (InterruptedException e) {
         System.out.println("Thread interrupted.");
      }
      System.out.println("Starting thread4...");
      Thread thread4 = new GuessANumber(75);
      
      thread4.start();
      System.out.println("main() is ending...");
   }
}
Bu, aşağıdaki sonucu üretecektir.

Çıktı
Starting hello thread...
Starting goodbye thread...
Hello
Hello
Hello
Hello
Hello
Hello
Goodbye
Goodbye
Goodbye
Goodbye
Goodbye
.......


Kaynaklar

https://www.youtube.com/watch?v=-BbRdNwRghg
https://www.tutorialspoint.com/java/java_multithreading.htm
https://mkyong.com/spring/spring-and-java-thread-example/
https://codegym.cc/quests/QUEST_JAVA_MULTITHREADING
https://dzone.com/articles/multi-threading-in-spring-boot-using-completablefu