# Kotlin 정리
  **[테크과학! DiMo](https://www.youtube.com/c/CHDiMo/featured)의 코틀린강좌를 보고 정리하였습니다.**   
  [Kotlin Playground](https://play.kotlinlang.org/)를 통해서 코드를 실행시켜볼 수 있다.

# 목차
<details>
<summary>1. 변수와 자료형</summary>
<div markdown='1'>

## 변수   

코틀린에서는 var과 val로 변수를 선언할 수 있다.   
- var : 일반적으로 통용되는 변수. 언제든지 읽기 쓰기가 가능하다.   
- val : 선언시에만 초기화 가능, 중간에 값을 변경할 수 없다.
<br>

값을 할당하는 것은 변수를 참조하여 사용하기 전까지만 해주면 된다.
<br>

kotlin은 **기본변수에서 null을 허용하지 않으며**, 또한 변수에 값을 할당하지 않은채로 사용하게 되면 컴파일을 막는다.    
의도치 않은 동작 및 NullPointException을 막아준다.   
<br>

Nullable 변수 : 자료형 뒤에 물음표를 붙이면 null을 허용하는 nullable 변수로 가용 가능하다.   
단, NullPointException이 발생할 수 있으므로 주의해서 사용해야한다.

## 자료형
기본자료형? 자바와의 호환을 위해서 거의 비슷하다.
- 정수형 : Byte Short Int Long   
- 실수형 : Float Double   
- 문자형 : Char   
- 논리형 : Boolean    
- 문자열 : String   

`Any : 어떤 자료형이든 상관 없다는 의미`
```kotlin
fun main() {
    val valVar:Int = 1
    var nullableVar: Int? = null;	//nullable
    // 정수형
    var intVar: Int = 123;
    var longVar: Long = 231231L;
    var binVar :Int = 0b101010;
    var hexVar = 0x123a;
    //실수형
    var doubleVar:Double = 123.1;
    var floatVar: Float = 123.1f;
    //문자
    var charVar = 'A';	//var charVar:Char = 'A';
    //논리
    var boolVar = true;
    //문자열
    var strVar1 = "hello world"
    var strVar2:String = """Hello"""	//줄바꿈 가능
}
```

---
</div>
</details>

<details>
<summary>2. 형변환과 배열</summary>
<div markdown="1">

---

## 형변환
형변환 : 하나의 변수에 지정된 자료형을 호환되는 다른 자료형으로 변경하는 기능   
toByte(), toShort(), toInt(), toLong(), toFloat(), toDouble(), toChar() 함수를 통해서 가능하다.

- 명시적 형변환 : 변환될 자료형을 개발자가 직접 지정하는것   

코틀린은 형변환시 발생할 수 있는 오류를 막기 위해 다른 언어에서 제공하는 암시적 형변환을 지원하지 않는다.


## 배열
Array클래스로 재공되는 기능임.
arrayOf를 통해서 배열 생성

`null을 가지는 빈 배열을 생성하고 싶을때는, arrayOfNulls<자료형>(size)를 통해서 생성한다.`

값을 할당 or 사용?  intArr[2]   
(*기존의 언어들의 배열과 같은 방식으로 사용 가능하다.*)

배열은 처음 선언했을때의 크기를 변경할 수 없다는 단점이 있지만, 한 번 선언해두면 다른 자료구조보다 빠른 입출력이 가능하다는 장점이 있다.

```kotlin
fun main() {
	//형변환
	var intVar:Int = 12345;
    var longVar:Long = intVar.toLong();
    
    //배열 선언
    var intArr = arrayOf(1,2,3,4,5);
    var nullArr = arrayOfNulls<Int>(5);
    
    intArr[2] = 8;
    println(intArr[2])
}
```

---
</div>
</details> 

<details>
<summary>3. 타입추론과 함수</summary>
<div markdown='1'>

## 타입추론
타입추론 : 변수나 함수들을 선언할 때나 연산이 이루어 질 때, 자료형을 코드에 명시하지 않아도 코틀린이 자동으로 자료형을 추론해주는 기능   

이는 변수가 선언될 때 할당된 값의 형태로 해당 변수가 어떤 자료형을 가지는지 추론이 가능하기 때문이다.

기본 자료형들도 선언시 값을 할당만 해준다면 대부분 자료형을 명시할 필요가 없다.

```kotlin
fun main(){
    var a = 1;
    var b = 1L;
    
    var c = 1.1;
    var d = 1.1f;
    
    var e = true;
    var f = 'A';
    var g = "string";
}

```

## 함수

코틀린에서 함수는 어디든지 둘수 있다.   
함수는 fun으로 시작해야한다.   
파라미터에 어떤 타입으로 받을지를 정하고, 괄호를 닫은 다음 반환할 자료형을 명시해야한다.
(반환값이 없으면 명시하지 않아도 괜찮다.)
<br>

단일표현식함수: 반환형의 타입추론이 가능하므로 반환형을 생략할 수 있다.
자료형이 결정된 변수라는 개념으로 접근하면 좋다.

```kotlin
fun main(){
    println(add(1,2,3))
}
/* 단일표현식 함수
fun add(a:Int,b:Int,c:Int) = a+b+c
*/
fun add(a:Int,b:Int,c:Int):Int{
    return a+b+c
}
```
---
</div>
</details>

<details>
<summary>4. 조건문과 비교연산자</summary>
<div markdown='1'>

## 조건문
1. if문과 else문 사용(다른 언어와 비슷함)
2. when (switch와 비슷)

```kotlin
when(a){   
    // 조건값들   
}  
``` 
**여러 조건들이 부합하게 된다면 먼저 부합하는 조건이 실행됨을 유의**

## 비교연산자
- 부등호(>, >=, <, <=)
- ==
- is, !is (자료형을 검사하는 연산자이다.)

```kotlin
fun main(){
	var a = 20;
    if(a is Int) println("Int")
    doWhen(1)
    doWhen("Hello")
    doWhen(3.14)
}
fun doWhen(a:Any){
    var result = when(a){
    	1 -> "Int"
        "Hello" -> "Str"
        else -> "else"
    }
    println(result)
}
```

</div>
</details>

<details>
<summary>5. 반복문과 증감연산자</summary>
<div markdown='1'>

## 반복문
1. 조건형 반복문 : **while**, **do..while**
2. 범위형 반복문 : **for**
    - for(i in 0..9) 증가값 = 1
    - for(i in 0..9 step 3)
    - for(i in 9 downTo 0)
    - for(i in 9 downTo 0 step 2)
    - for(i in ‘a’..’e’) 

## 증감연산자
증감연산자 : a++, ++a , a—, —a (전위연산자 후위연산자)   

---

</div>
</details>

<details>
<summary>6. 흐름제어와 논리연산자</summary>
<div markdown='1'>

## 흐름제어
1. return 함수를 종료하고 값을 반환하는 역할을 하고있다.
2. break, continue — 기존 언어의 break, continue와 같은역할

\+ 코틀린에서 추가된 기능 label   
`**@loop** , **break@loop**`   
- 기존의 언어에서는 2중 for문을 나오기 위해서는 두 조건이 필요했지만 label을 통해서 지정한 loop를 break한다.
```kotlin
fun main(){
    loop@for(i in 1..10){
        for(j in 1..10){
            if(i==1 && j==3) break@loop
        	println("i: $i, j: $j")
        }
    }
}
```

## 논리연산자

논리연산자 : && , || , !  —> 기존 언어와 같은 역할

---
</div>
</details>

<details>
<summary>7. 클래스의 기본 구조</summary>
<div markdown='1'>

## 클래스
클래스는 값과 그 값을 사용하는 기능들을 묶어 놓은것이다.   
클래스는 고유의 특징값인 속성, 기능의 구현인 함수로 이루어져 있다.   
클래스는 인스턴스를 만드는 틀이라는 것을 이해해야한다.   
> 인스턴스 : 클래스를 통해서 만들어낸, 서로 다른 속성의 객체를 지칭하는 용어

코틀린은 객체지향언어를 기반으로 함수형 언어의 장점을 흡수한 실용적인 언어이다.

```kotlin
fun main(){
	var a = Person("박보영",1990)
    var b = Person("전정국",1997)
    var c = Person("장원영",2004)
    
    a.introduce()
}

class Person(var name:String, val birthYear:Int){
    fun introduce(){
        println("안녕하세요. ${birthYear}년생 ${name}입니다.") 
    }
}
```
---

</div>
</details>

<details>
<summary>8. 클래스의 생성자</summary>
<div markdown='1'>

## 클래스의 생성자
생성자 : 새로운 인스턴스를 만들기 위해서 호출하는 특수한 함수
1. 인스턴스의 속성을 초기화
2. 인스턴스 생성시 구문을 수행   

```kotlin
/*클래스의 속성도 선언하는 동시에 생성자 역시 선언하는 방법이다.*/
class Person(var name:String, val birthYear:Int) 
```   
init함수를 통해서 이를 수행할 수 있다.

<br>

클래스를 만들때 생성하는 기본 생성자 외에 보조생성자라는 것을 통해 속성에 기본값을 정해줄 수 있다.   
* 보조생성자 : 기본 생성자와 다른 형태의 생성자를 제공하여 인스턴스 생성시 편의를 제공하거나, 추가적인 구문을 수행하는 기능을 수행한다.

보조생성자를 사용하기 위해서 construct를 사용한다.   
construct를 사용할 때, 반드시 기본 생성자를 통해 속성을 초기화 해줘야 한다.   
```kotlin
    //기본생성자를 호출할 수 있다.
    construct():this(...)  
```
```kotlin
fun main(){
	var a = Person("박보영",1990)
    var b = Person("전정국",1997)
    var c = Person("장원영",2004)
    
    var d = Person("차은우")
    var e = Person("류수정")
}

class Person(var name:String, val birthYear:Int){
    init{
        println("안녕하세요. ${this.birthYear}년생 ${this.name}입니다.") 
    }
    constructor(name:String):this(name,1997){
        println("보조 생성자가 사용되었습니다.")
    }
}
```
---

</div>
</details>

<details>
<summary>9. 클래스의 상속</summary>
<div markdown='1'>

## 클래스의 상속

### 상속이 필요한 경우
1. 기존 클래스를 확장해서 새로운 속성이나 함수를 가진 클래스를 만들어야 할 떄
2. 여러개의 클래스를 만들었는데, 공통적인 기능을 가진 것을 뽑아 코드관리를 쉽게 하고싶을 때

- SuperClass = 상속을 하는 쪽
- SubClass = 상속을 받는 쪽

`코틀린에서 상속 금지가 기본값이다. 상속을 하기위해서는 클래스를 open상태로 만들어주어야한다.`
- open은 클래스가 상속을 받을 수 있도록, 클래스 선언시 붙여줄 수 있는 키워드이다.
 
### 상속에 대한 2가지 규칙
1. 서브클래스는 수퍼클래스에 존재하는 속성과 같은이름의 속성을 가질 수 없다.
2. 서브클래스가 생성될때는 반드시 수퍼클래스의 생성자까지 호출해야한다.
   
```kotlin
fun main(){
	var a = Animal("별이",5,"개")
    var b = Dog("별이",5)
    var c = Cat("냥",5)
	
    a.introduce();
    b.introduce();
    b.bark();
    c.introduce();
    c.meow();
}

open class Animal(var name:String, var age:Int,var type:String){
	fun introduce(){
        println("저는 ${type} ${name}이고, ${age}살 입니다.")
    }
}
class Dog (name: String, age:Int):Animal(name,age,"개")
{
	fun bark(){
        println("멍멍");
    }
}
class Cat(name: String, age:Int):Animal(name,age,"고양이")
{
	fun meow(){
        println("냥냥");
    }
}

```
---

</div>
</details>

<details>
<summary>10. 오버라이딩과 추상화</summary>
<div markdown='1'>

## 오버라이딩
오버라이딩을 통해서 상속관계에 있을 때 같은 이름과 형태로 된 함수의 내용을 다시 구현할 수 있다.

서브클래스에서는 수퍼클래스의 함수를 재구현하는 것이 불가능하다.
하지만 수퍼클래스에서 함수에 open 키워드가 붙어있다면 재구현이 허용된다.

서브클래스에서 override를 붙여서 재구현 하면 된다.
```kotlin
fun main(){
    var a = Animal()
    var t = Tiger()
    a.eat()
    t.eat()
}
open class Animal{
    open fun eat(){
        println("음식을 먹습니다.")
    }
}
class Tiger :Animal(){
    override fun eat(){
        println("고기를 먹습니다.")
    }
}
```
> 수퍼클래스에서 구현이 끝난 함수를 서브클래스에서 오버라이딩을 통해 재구현 하는 경우

## 추상화
오버라이딩과 다르게 수퍼클래스에서는 함수의 구체적인 구현은 없고, 단지 Animal의 모든 서브클래스는 eat이라는 함수가 반드시 있어야한다는 점만 명시하여 각 서브클래스가 비어있는 함수의 내용을 필요에 따라 구현하도록 하려면 추상화라는 개념이 필요하다.   

추상화 : 선언부만 있고 기능이 구현되지 않은 추상함수, 추상함수를 포함하는 추상클래스라는 개념이 있어한다.

추상클래스를 만들기 위해서는 class앞에 abstract를 붙여준다. 
추상함수에도 abstract를 붙여주고 함수의 내용은 작성하지 않는다.

#### 추상화 방법 **1. 추상클래스**
추상클래스는 미완성 클래스이므로 단독으로는 인스턴스를 만들 수 없다.
따라서 반드시 서브클래스에서 상속을 받아 abstract 표시가 된 함수들을 구현해줘야 한다
```kotlin
fun main(){
    var r = Rabbit()
    r.sniff()
    r.eat()
}

abstract class Animal{
    abstract fun eat();
    fun sniff(){
        println("킁킁")
    }
}
class Rabbit:Animal(){
    override fun eat(){
        println("당근을 먹습니다.")
    }
}
```
#### 추상화 방법 **2. 인터페이스**   
인터페이스 : 속성 추상함수 일반함수 모두를 가질 수 있다.   

인터페이스는 생성자를 가질수는 없으며 인터페이스에서 구현부가 있는 함수는 open함수로 간주, 구현부가 없는 함수는 abstract함수로 간주 별도의 키워드가 없어도 서브클래스에서 구현 및 재정의가 가능하다.

그리고 하나의 서브클래스에서 여러개의 인터페이스를 상속받을 수 있으므로 좀 더 유연한 설계가 가능하다.

> **주의**   
여러개의 인터페이스에서 같은 이름과 형태를 가진 함수를 구현하고 있다면 서브클래스에서는 혼선이 일어나지 않도록 반드시 override를 명시해서 재구현해주어야 합니다.
```kotlin
fun main(){
    var d = Dog()
    d.run()
    d.eat()
}

interface Runner{
 	fun run()   
}
interface Eater{
    fun eat(){
        println("음식을 먹습니다.")
    }
}
class Dog: Runner,Eater{
    override fun run(){
        println("우다다다 달립니다.")
    }
    override fun eat(){
        println("허겁지겁 먹습니다.")
    }
}
```
---

</div>
</details>