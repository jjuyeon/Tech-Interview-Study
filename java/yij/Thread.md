# Thread

## 프로세스와 쓰레드

**프로세스는 실행 중인 프로그램의 단위**를 말한다. 현재 OS의 특성 상 동영상을 보면서 동시에 문서 작업을 하는 것과 같은 다중 동시 작업을 지원해야 할 때 여러 프로세스가 메모리에 동시에 적재되고 번갈아가며 실행된다. **프로세스 또한 여러 작업 단위를 동시에 실행**해야 할 수 있다. **프로세스는 그래서 한 개 이상의 쓰레드**로 이루어져 있다. 각각의 쓰레드는 고유한 <code>stack</code> 영역을 가지며, <code>code, data, heap</code> 영역을 공유한다.

<br>

## Java에서의 Thread

JVM은 프로그램의 실행 동안 여러 개의 쓰레드를 동시에 실행할 수 있다.

각각의 Thread는 **priority**를 가지며, 이는 실행의 우선순위를 말한다. 모든 쓰레드는 데몬 쓰레드(사용자가 직접 제어하지 않고 백그라운드에서 실행. 대개 부모 프로세스를 갖지 않는다)가 될 수 있으며, 부모 프로세스가 데몬일 경우에만 자식 프로세스가 데몬이 된다. 자식 쓰레드의 priority는 부모 쓰레드의 priority를 따른다.
JVM이 시작되면 single non-daemon thread(main 메소드가 있는 클래스)가 실행되며, 아래 상황이 발생하면 쓰레드를 종료한다.

- <code>Runtime</code>클래스의 <code>exit</code>메소드가 호출되고 종료가 승인될 때
- 1️⃣<code>run</code>메소드를 반환하거나  2️⃣<code>run</code>메소드가 처리할 수 있는 범위를 벗어난 예외를 throws해서 데몬이 아닌 모든 쓰레드가 종료되었을 때

<br>

Java에서는 아래의 두 가지 방법을 통해 쓰레드를 이용할 수 있다.

<br>

### extends Thread

: <code>Thread</code> Class를 상속받는 SubClass를 정의한다. 해당 클래스는 <code>run</code> 메서드를 Override해야한다.

``` java
     class PrimeThread extends Thread {
         long minPrime;
         PrimeThread(long minPrime) {
             this.minPrime = minPrime;
         }

         public void run() {
             // compute primes larger than minPrime
              . . .
         }
     }
```

아래의 코드를 이용해 쓰레드를 생성하고 시작할 수 있다.

``` java
     PrimeThread p = new PrimeThread(143);
     p.start();
```

<br>

### implements Runnable

: <code>Runnable</code> Interface를 구현한다. 해당 클래스는 반드시 <code>run</code> 메서드를 구현해야 한다. 참고로 <code>Thread</code> Class는 <code>Runnable</code> Interface를 implements한다.

``` java
     class PrimeRun implements Runnable {
         long minPrime;
         PrimeRun(long minPrime) {
             this.minPrime = minPrime;
         }

         public void run() {
             // compute primes larger than minPrime
              . . .
         }
     }
```

<code>Thread</code> 클래스를 생성하면서 인자를 넘겨주는 방식으로 쓰레드를 실행할 수 있다.

``` java
     PrimeRun p = new PrimeRun(143);
     new Thread(p).start();
```