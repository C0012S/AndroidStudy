# 💡 Thread

# ✅ Thread
휴대기기에는 프로세서가 있다. 오늘날 대부분의 기기에는 각기 프로세스를 동시에 실행하는 여러 하드웨어 프로세서가 있습니다. 이를 다중 처리라고 한다.  
운영체제는 프로세서를 더 효율적으로 사용하기 위해 애플리케이션이 프로세스 내에서 둘 이상의 실행 스레드를 만들도록 할 수 있다. 이를 **멀티 스레딩**이라고 한다.  

<br/>

![Untitled](https://developer.android.com/static/courses/extras/images/multi-threading-1.png?hl=ko)

<br/>

동시에 여러 권의 책을 읽을 때 각 챕터 단위로 책을 번갈아 가며 읽다 보면 결국에는 모든 책을 다 읽을 수 있지만 정확히 동시에 두 권 이상의 책을 읽을 수 없는 것과 같다고 생각할 수 있다.  
이러한 모든 스레드를 관리하려면 약간의 인프라가 필요하다.

<br/>

![Untitled](https://developer.android.com/static/courses/extras/images/multi-threading-2.png?hl=ko)

<br/>

**스케줄러**는 우선 순위와 같은 사항을 고려하며 모든 스레드가 실행되고 완료되도록 한다. 책이 영원히 선반에 놓인 상태로 먼지가 쌓이도록 허용되지 않지만, 책이 너무 길거나 기다릴 수 있는 경우 전달되기까지 시간이 걸릴 수 있다.  
**디스패처**는 스레드를 설정한다. 즉, 읽어야 하는 책을 보내고 독서가 진행될 컨텍스트를 지정한다. 컨텍스트는 별도의 전문적인 독서실로 생각할 수 있다. 일부 컨텍스트는 사용자 인터페이스 작업에 가장 적합하고 일부는 입력/출력 작업을 처리하는 데 특화되어 있다.  
사용자 대상 애플리케이션은 일반적으로 포그라운드에서 실행되는 기본 스레드를 가지고 있으며 백그라운드에서 실행될 수 있는 다른 스레드를 전달할 수 있다.

<br/>

Android에서 기본 스레드는 UI에 관한 모든 업데이트를 처리하는 단일 스레드이다.   
UI 스레드라고도 하는 이 기본 스레드는 모든 클릭 핸들러, 기타 UI, 수명 주기 콜백을 호출하는 스레드이기도 하다.  
앱이 명시적으로 스레드를 전환하거나 다른 스레드에서 실행되는 클래스를 사용하는 경우를 제외하고 앱이 하는 모든 작업은 기본 스레드에 있다.  
이로 인해 어려운 상황이 발생할 수 있다.   
즉, 뛰어난 사용자 환경을 보장하려면 UI 스레드가 원활하게 실행되어야 한다.  
앱이 눈에 띄는 일시중지 없이 사용자에게 표시되도록 하려면 기본 스레드가 16ms마다 또는 더 자주 화면을 업데이트하거나 초당 약 60프레임으로 화면을 업데이트해야 한다.  
이 속도에서 인간은 프레임 변경이 완전히 원활하다고 인식한다. 즉, 적은 시간에 많은 프레임을 렌더링해야 한다.  
따라서 Android에서는 UI 스레드 차단을 방지하는 것이 중요하다.  
여기에서 차단은 데이터베이스가 업데이트를 완료할 때까지와 같은 상황에서 기다리는 동안 UI 스레드가 아무 작업도 하지 않는 것을 의미한다.

<br/>

![Untitled](https://developer.android.com/static/courses/extras/images/multi-threading-3.png?hl=ko)

<br/>

인터넷에서 데이터 가져오기, 대용량 파일 읽기, 데이터베이스에 데이터 쓰기와 같은 많은 일반적인 작업은 16ms 이상 걸린다.  
따라서 코드를 호출하여 기본 스레드에서 이와 같은 작업을 실행하면 앱이 일시 중지되거나 끊기거나 멈출 수 있다.  
그리고 기본 스레드를 너무 오랫동안 차단하면 앱이 비정상 종료될 수 있으며 '애플리케이션 응답 없음'(ANR) 다이얼로그가 표시될 수도 있다.

<br/>

### 콜백
기본 스레드에서 작업을 완료하는 방법으로는 몇 가지 옵션이 있다.  
기본 스레드를 차단하지 않고 장기 실행 작업을 이행하는 한 가지 패턴은 콜백이다.  
콜백을 사용함으로써 백그라운드 스레드에서 장기 실행 작업을 시작할 수 있다.  
작업이 완료되면 인수로 제공되는 콜백이 호출되어 기본 스레드의 결과를 코드에 알린다.

<br/>

콜백은 훌륭한 패턴이지만 몇 가지 단점이 있다.  
콜백을 매우 많이 사용하는 코드는 읽기 어렵고 추론하기가 더 어려워질 수 있다.  
코드는 순차적으로 보이지만 콜백 코드는 나중에 비동기 시간에 실행되기 때문이다.  
또한 콜백은 예외와 같은 일부 언어 기능의 사용을 허용하지 않는다.

<br/>

### 코루틴
Kotlin에서 코루틴은 장기 실행 작업을 원활하고 효율적으로 처리하기 위한 솔루션이다.  
Kotlin 코루틴을 사용하면 콜백 기반 코드를 순차 코드로 변환할 수 있다.  
순차적으로 작성된 코드는 일반적으로 읽기가 더 쉬우며 예외와 같은 언어 기능을 사용할 수도 있다.  
결국 코루틴과 콜백은 정확히 동일한 작업을 한다. 즉, 장기 실행 작업에서 결과를 사용할 수 있을 때까지 기다렸다가 실행을 계속한다.

<br/>

## Thread 생성 및 실행
1. Thread 클래스 구현
2. Runnable을 이용한 구현
    1. Runnable 구현
    2. Runnable 구현 (Lambda)
3. Kotlin thread() 함수

<br/>

### Thread 클래스 구현
Thread를 상속하여 새로운 Thread 클래스를 구현할 수 있다.  
run()을 오버라이드하여 처리해야 하는 내용을 구현하면 된다.

<br/>

```kotlin
class MyThread: Thread() {
    public override fun run() {
        println("${Thread.currentThread()}: it's running.")
    }
}

fun main(args: Array<String>) {
    println("${Thread.currentThread()}: run the thread")

    val myThread = MyThread()
    myThread.start()

    println("${Thread.currentThread()}: wait until the thread is done")
    myThread.join()
}
```

<br/>

- 실행 결과
    
    ```kotlin
    Thread[main,5,main]: run the thread
    Thread[main,5,main]: wait until the thread is done
    Thread[Thread-0,5,main]: it's running.
    ```
    
<br/>

> join()은 thread가 종료될 때까지 기다린다.
> 
> 
> run()을 호출하지 않으면 Main thread가 종료되며, 새로운 thread가 수행되기 전에 프로그램이 종료될 수 있다.
> 

<br/>

### Runnable을 이용한 구현
#### Runnable 구현
새로운 Thread를 구현하는 대신, Runnable을 구현하고 Thread에 인자로 전달하여 실행시킬 수 있다.  
실행 결과는 이전 코드와 동일하다.

<br/>

```kotlin
class MyRunnable: Runnable {
    public override fun run() {
        println("${Thread.currentThread()}: it's running.")
    }
}

fun main(args: Array<String>) {
    println("${Thread.currentThread()}: run the thread")

    val myThread = Thread(MyRunnable())
    myThread.start()

    println("${Thread.currentThread()}: wait until the thread is done")
    myThread.join()
}
```

<br/>

- 실행 결과
    
    ```kotlin
    Thread[main,5,main]: run the thread
    Thread[main,5,main]: wait until the thread is done
    Thread[Thread-0,5,main]: it's running.
    ```

<br/>    

#### Runnable 구현 (Lambda)
Runnable 클래스를 구현하는 대신, 아래와 같이 Lambda 표현식으로 간단히 구현부를 전달할 수 있다.  
코드는 더 짧아졌고, 실행 결과는 이전 코드와 동일하다.

<br/>

```kotlin
fun main(args: Array<String>) {
    println("${Thread.currentThread()}: run the thread")

    val myThread = Thread {
        println("${Thread.currentThread()}: it's running.")
    }
    myThread.start()

    println("${Thread.currentThread()}: wait until the thread is done")
    myThread.join()
}
```

<br/>

- 실행 결과    
    ```kotlin
    Thread[main,5,main]: run the thread
    Thread[main,5,main]: wait until the thread is done
    Thread[Thread-0,5,main]: it's running.
    ```

<br/>    

join()을 호출하지 않는다면 아래와 같이 더 간단하게 thread를 생성하고 어떤 작업을 처리할 수 있다.

```kotlin
Thread {
    println("${Thread.currentThread()}: it's running.")
}.start()
```

<br/>

### Kotlin thread() 함수
코틀린은 thread()함수를 제공하며, thread() { ... }는 thread 객체를 리턴한다.  
기본적으로 Thread.start()가 호출된 상태로 리턴되기 때문에 직접 호출할 필요가 없다.

<br/>

```kotlin
import kotlin.concurrent.thread

fun main(args: Array<String>) {
    println("${Thread.currentThread()}: run the thread")

    val myThread = thread() {
        println("${Thread.currentThread()}: it's running.")
    }

    println("${Thread.currentThread()}: wait until the thread is done")
    myThread.join()
}
```

<br/>

- 실행 결과
    
    ```kotlin
    Thread[main,5,main]: run the thread
    Thread[main,5,main]: wait until the thread is done
    Thread[Thread-0,5,main]: it's running.
    ```
    
<br/>

start()가 호출되지 않은 thread를 생성하고 리턴받고 싶다면, start = false를 인자로 전달하면 된다.

```kotlin
val myThread = thread(start = false) {
    println("${Thread.currentThread()}: it's running.")
}
```

<br/>

join()을 호출하지 않는다면 아래와 같이, 더 간단하게 thread를 생성하고 어떤 작업을 처리할 수 있다.

```kotlin
thread() {
    println("${Thread.currentThread()}: it's running.")
}
```

<br/>

## Thread에서 UI로 접근하기
### runOnUiThread로 UI에 접근
- Thread.sleep() 함수를 사용해서 초단위로 TextView에 반영한 코드로, runOnUiThread를 사용해서 간단하게 UI에 접근한다.

```kotlin
class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		
		
		thread(start = true) {
			var i = 0

			while (i < 10) {
				i += 1

				runOnUiThread {
					textView.text = "카운트 : ${i}"
				}

				Thread.sleep(1000)
			}
		}
	}
}
```

<br/>
<br/>

# 🗂 참고
- [멀티스레딩 및 콜백 기본 지침서  |  학습 과정  |  Android Developers](https://developer.android.com/courses/extras/multithreading?hl=ko)
- [Kotlin - Thread 생성 및 실행 (codechacha.com)](https://codechacha.com/ko/kotlin-thread/)
- [코틀린으로 안드로이드 스레드(Thread) 한방에 끝내기 (tistory.com)](https://magicalcode.tistory.com/entry/%EC%BD%94%ED%8B%80%EB%A6%B0%EC%9C%BC%EB%A1%9C-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C)
- [[안드로이드] 코틀린 Thread 클래스 / Runnable 인터페이스 사용 (tistory.com)](https://enter.tistory.com/277)
