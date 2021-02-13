# Kotlin 정리
**[테크과학! DiMo](https://www.youtube.com/c/CHDiMo/featured)의 코틀린강좌를 보고 정리하였습니다.**   
[Kotlin Playground](https://play.kotlinlang.org/)를 통해서 코드를 실행시켜볼 수 있다.   
  
*(복습할때 코드를 꼭 실행시켜보자.)*

<br>

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

<details>
<summary>11. 코틀린 프로젝트 구조</summary>
<div markdown='1'>
패키지명만으로도 패키지를 다르게 설정하는 것이 가능하다.   
(디렉토리의 구조가 패키지명이 되었던 자바와는 다르게 같은 디렉토리에 있어도 패키지명만 다르면 다른 패키지로 생각한다.)

---

</div>
</details>

<details>
<summary>12. 스코프와 접근제한자</summary>
<div markdown='1'>

## Scope
Scope : 언어차원에서  변수나 함수, 클래스 같은 멤버들을 서로 공유하여 사용할 수 있는 범위를 지정해둔 단위이다.   
패키지 내부, 클래스 내부, 함수 내부 등등이 있다.

1. 스코프 외부에서는 스코프 내부의 멤버를 참조연산자를 통해서만 참조 가능하다.
2. 동일스코프 내에서는 멤버를 공유할 수 있다.
3. 하위 스코프에서는 상위스코프의 멤버를 재정의 할 수 있다.

 `스코프의 같은 레벨에서는 같은 이름의 멤버를 만들어서는 안된다.`   
`스코프 외부에서 스코프 내부로 접근하려면 참조연산자를 사용해야한다.`
## 접근제한자
접근제한자 : 스코프 외부에서 스코프 내부에 접근할 때, 그 권한을 개발자가 제어할 수 있는 기능

~~~
- public    
- internal    
- private    
- protected
~~~

- 패키지 스코프
  1. public (기본값) : 어떤 패키지에서도 접근 가능
  2. internal : 같은 모듈 내에서만 접근 가능
  3. private : 같은 파일내에서만 접근가능 
  4. protected : 사용하지 않는다.

- 클래스 스코프
  1. public (기본값) : 클래스외부에서 늘 접근 가능
  2. private : 클래스 내부에서만 접근가능 
  3. protected : 클래스 자신과 상속받은 클래스에서 접근 가능
  4. internal : 사용하지 않음

```kotlin
var a ="패키지 스코프"

class B{
    var a = "클래스 스코프"
    fun print(){
        println(a)
    }
}
fun main(){
    var a = "함수 스코프"
    println(a)
    B().print()
} 
```
---
</div>
</details>

<details>
<summary>13. 고차함수와 람다함수</summary>
<div markdown='1'>

## 고차함수
**고차함수** :  함수를 마치 클래스에서 만들어낸 인스턴스처럼 취급하는 방법   
함수를 패러미터로 넘겨줄수도 있으며 결과값으로 반환받을수도 있는 방법   
코틀린에서는 모든 함수를 고차함수로 사용가능하다. 

~~~kotlin
function: (String) -> Unit(Unit은 반환형이 없다는 것을 의미) 
// 이러한 형태의 함수는 다 파라미터로 받을 수 있게됨
~~~

**::** -> 일반함수를 고차함수로 변경해주는 연산자

## 람다함수
**람다함수** : 그 자체가 고차함수임 따라서 별도의 연산자 없이도 변수에 담을 수 있다.


**고차함수와 람다함수를 사용하면 함수를 일종의 변수로 사용할 수 있다는 편의성도 있고, 컬렉션의 조작이나 스코프 함수의 사용에도 도움이 된다.**

```kotlin
fun main(){
	b(::a)
    val c: (String)->Unit = {str -> println("$str 람다함수")}
    //val c: {str:String -> println("$str 람다함수")}
    b(c)
} 
fun a(str:String){
    println("$str 함수 a")
}
fun b(function: (String)->Unit){
    function("b가 호출한")
}
```

---
</div>
</details>

<details>
<summary>14. 스코프 함수</summary>
<div markdown='1'>

## 스코프 함수
**스코프 함수** : 람다함수를 활용한 특별한 함수   
함수형 언어의 특징을 좀 더 편리하게 사용할 수 있도록 기본 제공하는 함수이다.    
인스턴스의 속성이나 함수를 좀 더 깔끔하게 불러 쓸 수 있다.

1. 람다함수도 여러 구문의 사용이 가능    
   람다함수가 여러줄일 경우 마지막 구문의 결과값이 반환된다. 
2. 람다함수에 패러미터가 없다면 실행할 구문들만 나열하면 된다.
3. 패러미터가 하나뿐이라면 it 사용    
   패러미터가 여러개라면 패러미터의 이름을 일일히 써야한다.    
   패러미터가 단 하나라면 it이라는 키워드로 대체해서 사용할 수 있다.

스코프함수에는  apply, run, with, also, let가 있다. 
- apply : 인스턴스를 생성한 후  변수에 담기전에 초기화과정을 수행할 때 많이 사용함   
main 함수와 별도의 scope에서 인스턴스의 변수와 함수를 조작함으로 코드가 깔끔해진다는 장점이 있다.
- run : apply처럼 run스코프 안에서 참조연산자를 사용하지 않아도 된다는 점은 같지만 일반 람다함수처럼 인스턴스 대신에 결과값을 반환한다는 점이 차이점   
따라서 인스턴스가 이미 만들어진 후에 인스턴스의 함수나 속성을 scope내에서  사용해야할 때 유용하다.
- with : run과 동일한 기능을 가지지만, 단지 인스턴스를 참조연산자 대신 패러미터로 받는다는 차이뿐
- also/let : 처리가 끝나면 인스턴스를 반환 (apply/ also)    
처리가 끝나면 최종값을 반환(run/ let)   
apply와 run이 인스턴스의 변수와 함수를 사용할 수 있었다면, also,let 은 마치 패러미터로 인스턴스를 넘긴것처럼 it을 통해서 인스턴스를 사용할 수 있다.
> **왜 패러미터를 통해서 인스턴스를 사용하는 귀찮은 과정을 거치는가?**
`이는 같은 이름의 변수나 함수가 scope 바깥에 중복되어있는경우 혼란을 방지하기 위해서`


```kotlin
fun main(){
    var price = 5000
	var a = Book("코틀린",10000).apply{
    	name = "[초특가]"+name
    	discount()
    }
    /* price가 5000원으로 출력되게 된다.
    why? run함수가 인스턴스 내의 price속성보다 run 이 속해있는 main함수의 price변수를 우선시하고 있기 때문이다.*/
    a.run{
        println("상품명: ${name}, 가격:${price}원")
    }
   a.let{
        println("상품명: ${it.name}, 가격:${it.price}원")
    }
	println(a.price)
} 
class Book(var name :String,var price:Int){
    fun discount(){
        price -= 2000
    }
}
```
---
</div>
</details>

<details>
<summary>15. 오브젝트</summary>
<div markdown='1'>

## 오브젝트
클래스는 단지 인스턴스 객체를 만들기 위한 틀이므로 내부에있는 속성이나 함수를 사용하려면 생성자를 통해 실체가 되는 인스턴스 객체를 만들어야했다.    
공통적인 속성과 함수를 사용해야하는 코드에서는 굳이 클래스를 쓸 필요없이 object를 사용하는 것.   
Singleton Pattern을 언어 차원에서 지원해주는 것이다.
```kotlin
// 인스턴스를 생성하지 않고 그 자체로 객체이기 때문에 생성자는 사용하지 않는다.  
object name {

} 
```

```kotlin
fun main(){
    println(Counter.count)
    
    Counter.countUp()
    Counter.countUp()

    println(Counter.count)
    Counter.clear()
    
    println(Counter.count)
}
object Counter{
    var count = 0;
    fun countUp(){
        count++;
    }
	fun clear(){
        count = 0
    }
}
```

object로 선언된 객체는 최초 사용시 자동으로 생성되며 이후에는 코드 전체에서 공용으로 사용될 수 있으므로 프로그램이 종료되기 전까지 공통적으로 사용할 내용들을 묶어 만드는 것이 좋다.

기존 클래스안에도 object를 만들 수 있다.    
`Companion Object`   
인스턴스가 사용할 공용 속성 및 함수를 이를 사용한다.
이는 기존의 언어가 가진 static 멤버와 비슷한 느낌이다.

```kotlin
fun main(){
	var a = FoodPoll("짬뽕")
    var b = FoodPoll("짜장")
	
    a.vote()
    a.vote()
    
    b.vote()
    b.vote()
    b.vote()
    
    println("${a.name} : ${a.count}")
	println("${b.name} : ${b.count}")
	println("통계 : ${FoodPoll.total}")

}
class FoodPoll(val name: String){
    companion object {
        var total = 0
    }
    var count =0;
    fun vote(){
        total++;
        count++
    }
}
```
---
</div>
</details>

<details>
<summary>16. 익명객체와 옵저버 패턴</summary>
<div markdown='1'>

## 옵저버패턴
**옵저버패턴** : 이벤트가 일어나는것을 감시하는 감시자의 역할을 만든다고 하는것

```kotlin
fun main(){
    EventPrinter().start()
}
interface EventListener{
    fun onEvent(count: Int)
}

class Counter(var listener:EventListener){
    fun count(){
        for(i in 1..100){
            if(i%5==0) listener.onEvent(i)
        }
    }
}
class EventPrinter:EventListener {
    override fun onEvent(count:Int){
        print("${count}-")
    }
    fun start(){
        val counter = Counter(this)
        counter.count()
    }
}
```
여기서 EventPrinter가 EventListener를 상속받아 구현하지 않고 임시로 만든 별도의 EventListener 객체를 대신 넘겨줄수도 있다.

이것을 <u>이름이 없는 객체</u>라고 해서 **익명객체**라고 한다.   
`object: …` 해서 object가 상속받도록 하고 상속받은 함수를 override해서 구현하면 된다.   
이렇게 만들면 인터페이스를 구현한 객체를 코드 중간에도 즉시 생성해서 사용할 수 있다.
```kotlin
fun main(){
    EventPrinter().start()
}
interface EventListener{
    fun onEvent(count: Int)
}

class Counter(var listener:EventListener){
    fun count(){
        for(i in 1..100){
            if(i%5==0) listener.onEvent(i)
        }
    }
}
class EventPrinter {
    fun start(){
        val counter = Counter(object:EventListener{
            override fun onEvent(count:Int){
                print("${count}-")
            }
        })
        counter.count()
    }
}
```

---
</div>
</details>

<details>
<summary>17. 다형성</summary>
<div markdown='1'>

## 다형성
코틀린 내부에서 생각해보자 -> **Drink** 라는 수퍼클래스를 **Cola**라는 서브클래스가 상속받았다고 해보자.   
여기서 **Cola**의 공간에는 **Drink**의 객체 공간에 *Cola*의 추가 공간이 생기는 것이다.   
따라서 **Drink** 기능을 사용하는 변수에 저장되면 **Drink**의 기능은 할 수 있지만, **Cola**의 기능은 하지 못한다.   
하지만 **Cola** 기능을 사용하는 변수에 저장되면 모든 기능을 사용할 수 있다.

- Up-casting 는 상위 자료형에 하위 자료형을 담는 것이다.
- Down-casting : 특별한 캐스팅 연산자가 필요하다.
   - as, is 연산자 : as는 변환시켜주는 연산자, is는 자료형에 호환되는지를 확인한 뒤 변환해주는 연산자이다.

```kotlin
fun main(){
    var a = Drink()
    a.drink()
    var b:Drink = Cola()
    b.drink()
    //이 상태에서 b.washDishes()를 호출하면 참조할 수 없다는 에러 발생
    //Down casting을 해야한다.
    if(b is Cola){	//is는 조건문 안에서만 잠시 다운캐스팅 된다
        b.washDishes()
    }
    var c = b as Cola
    c.washDishes()
    
    // 반환값 뿐만 아니라 변수 자체도 다운캐스팅된다
    b.washDishes()
}
open class Drink{
    var name ="음료"
    
    open fun drink(){
        println("${name}을 마십니다.")
    }
}
class Cola :Drink(){
    var type = "콜라"
    override fun drink(){
        println("${name}중에 ${type}을 마십니다.")
    }
    fun washDishes(){
        println("${type}를 설거지를 합니다.");
    }
}
```
---
</div>
</details>

<details>
<summary>18. 제네릭</summary>
<div markdown='1'>

## 제네릭
**제네릭** : 클래스나 함수에서 사용하는 자료형을 외부에서 지정할 수 있는 기능    
함수나 클래스를 선언할때 고정적인 자료형 대신 실제 자료형으로 대체되는 타입 패러미터를 받아서 사용하는 기능이다.

따라서 캐스팅연산 없이도 자료형을 그대로 사용할 수 있다.

Type의 이니셜인 T를 사용    
제네릭을 특정한 수퍼클래스를 상속받은 클래스 타입으로만 제한하려면 
`<T:SuperClass> ` 

#### 이렇게 선언된 제네릭은 어떻게 사용?   
제네릭을 사용하면 **캐스팅을 방지**하므로 성능을 높일 수 있다.

```kotlin
fun main(){
    // 타입 패러미터를 수동으로 전달할 수도 있지만
    // 생성자의 패러미터를 통해서 A라는것을 유추 가능. 생략해도 괜찮다.
    UsingGeneric(A()).doShouting()
    UsingGeneric(B()).doShouting()
    UsingGeneric(C()).doShouting()
    
    doShouting(B())
}
// 함수에도 사용이 가능하다.
fun <T:A>doShouting(t:T){
    t.shout();
}
open class A{
    open fun shout(){
        println("A가 소리칩니다.")
    }
}

class B:A(){
    override fun shout(){
        println("B가 소리칩니다.")
    }
}
class C:A(){
    override fun shout(){
        println("C가 소리칩니다.")
    }
}
class UsingGeneric<T:A> (val t:T){
    fun doShouting(){
        t.shout();
    }
}
```
---
</div>
</details>

<details>
<summary>19. 리스트</summary>
<div markdown='1'>

## 리스트 
- `List<out T>` : 생성시에 넣은 객체를 대체, 추가, 삭제할 수 없음
- `MutableList<T>` : 생성시에 넣은 객체를 대체, 추가, 삭제할 수 있음

생성은 listOf, mutableListOf 를 사용한다.   
mutableList에서는 shuffle이나 sort도 제공한다.   
list[인덱스] = 데이터도 가능하다.(배열과 같이 사용 가능)

```kotlin
fun main(){
    val a = listOf("사과", "딸기","배")
    println(a[1])
    
    for(fruit in a){
        print("${fruit} ")
    }
    println()
    
    val b = mutableListOf(6,3,1)
    println(b)
    
    b.add(4)
    println(b)
    
    b.add(2,8)
    println(b)
    
    b.removeAt(1)
    println(b)
    
    b.shuffle()
    println(b)
    
    b.sort()
    println(b)
    
}
```
---
</div>
</details>

<details>
<summary>20. 문자열을 다루는 방법</summary>
<div markdown='1'>

*(직접 실행시켜보자)*
#### **1. toLowerCase, toUpperCase, split, joinToString, substring**
```kotlin
fun main(){
    val test1 = "Test.Kotlin.String"
    
    println(test1.length)
    
    println(test1.toLowerCase())
    println(test1.toUpperCase())
    
    val test2 = test1.split(".")
    println(test2)
    
    println(test2.joinToString())
    println(test2.joinToString("-"))
    
    println(test1.substring(5..10))
}
```
#### **2. isNullOrEmpty, isNullOrBlank**
```kotlin
fun main(){
	val nullString: String? = null
    val emptyString = ""
    val blankString = " "
	val normalString = "A"
    
    println(nullString.isNullOrEmpty())
	println(emptyString.isNullOrEmpty())
	println(blankString.isNullOrEmpty())
	println(normalString.isNullOrEmpty())
	
    println()
    println(nullString.isNullOrBlank())
	println(emptyString.isNullOrBlank())
	println(blankString.isNullOrBlank())
	println(normalString.isNullOrBlank())    
}
```
#### **3. startsWith, endsWith, contains**
```kotlin
fun main(){
    var test3 = "kotlin.kt"
    var test4 = "java.java"
    
    println(test3.startsWith("java"))
	println(test4.startsWith("java"))

    println(test3.endsWith(".kt"))
	println(test4.endsWith(".kt"))

    println(test3.contains("lin"))
	println(test4.contains("lin"))
}
```
---
</div>
</details>

<details>
<summary>21. null 값을 처리하는 방법? 동일한지를 확인하는 방법?</summary>
<div markdown='1'>

## null값 처리하는 방법
꼭 if문으로 해야하는 것은 아니다.

1. `?.`null safe operator : 참조연산자를 실행하기전에 먼저 객체가 null인지 확인부터 하고 객체가 null이라면 뒤따라 오는 구문을 실행하지 않는다
2. `?:` elvis operator : 객체가 null이 아니라면 그대로 사용하지만 null이라면 연산자 우측의 객체로 대체되는 연산자 
3. `!!.` non-null assertion operator : 참조연산자를 사용할때, null여부를 컴파일시 확인하지 않도록 하여 런타임시 null point exception이 나도록 의도적으로 방치하는 연산자

> null safe 연산자는 스코프함수와 사용하면 더욱 편리하다

```kotlin
fun main(){
    var a: String? = null
    
    println(a?.toUpperCase())
    println(a?:”default”.toUpperCase())
    println(a!!.toUpperCase())
}

 fun main(){
    var a: String? = null 	//var a:String? = "kotlin EXAM"
    a?.run{
        println(toUpperCase())
        println(toLowerCase())
    }
}
```
## 동일성

- **내용의 동일성** : 메모리상에 서로 다르게 할당되어있어도 그 내용이 같다면 같다고 판단   
`a == b`
- **객체의 동일성** : 메모리 상에 서로 같은 객체를 가리키고 있을 때 같다고 판단   
`a === b`

```kotlin
fun main(){
    var a = Product("콜라",1000)
    var b = Product("콜라",1000)
    var c = a
    var d = Product("사이다",1000)

    println(a==b)
    println(a===b)
    
    println(a==c)
    println(a===c)
    
    println(a==d)
    println(a===d)
}
class Product(val name: String,val price:Int){
    override fun equals(other:Any?):Boolean{
        if(other is Product){
            return other.name == name && other.price == price
        }else {
            return false;
        }
    }
}
```
---
</div>
</details>

<details>
<summary>22. 함수의 argument를 다루는 방법과 infix 함수</summary> 
<div markdown='1'>

## 함수의 argument를 다루는 방법

코틀린은 함수의 overloading을 지원함 
> oveloading : 같은 이름의 함수를 여러개 지원하는 것 (패러미터의 개수와 타입이 다르다면)

```kotlin
fun main(){
	read(8)
    read("감사합니다.")
}
fun read(a:Int){
    println("숫자 $a 입니다.")
}
fun read(a:String){
    println(a)
}
```
- **dafault arguments** : 패러미터를 받아야 하지만, 별다른 패러미터가 없다면 기본값으로 작동하도록 한다.

- **named arguments** : 패러미터의 순서와 관계없이 패러미터의 이름을 사용하여 직접 패러미터의 값을 할당하는 기능

```kotlin
 fun main(){
	delivery("짬뽕")
    delivery("책",3)
    delivery("노트북",30,"학교")
    
    delivery("책상",destination="친구집")
}
fun delivery(name:String,count:Int = 1,destination:String = "집"){
	println("${name}, ${count}개를 ${destination}에 배달하였습니다.")
}
```
**variable number of arguments(vararg)** : 개수가 지정되지 않은 패러미터이다.    
따라서 다른 패러미터와 같이 사용할때 맨 마지막에 넣어야 한다.   
마치 배열처럼 for문으로 참조 가능
```kotlin
fun main(){
	sum(1, 2, 3, 4)
}
fun sum(vararg numbers:Int){
    var sum = 0
    
    for(n in numbers){
        sum +=n
    }
    print(sum)
} 
```
## Infix 함수
Infix 함수 : 함수를 연산자처럼 사용할 수 있게하는 함수  

클래스 안에서 infix함수를 적용할 때는 적용할 클래스가 자기 자신이므로 클래스의 이름은 쓰지 않는다.

```kotlin
fun main(){
	println(6 multiply 4)
    println(6.multiply(4))
}
infix fun Int.multiply(x:Int) = this*x
```
---
</div>
</details>

<details>
<summary>23. 중첩 클래스와 내부 클래스</summary> 
<div markdown='1'>

## 중첩클래스
**중첩클래스** : 하나의 클래스가 다른 클래스의 기능과 강하게 연관되어 있다는 의미를 전달하기 위해 만들어진형식

형태만 내부에 존재할 뿐, *외부클래스의 내용을 공유할 수 없다.* (별개의 클래스이다.)

```kotlin
class Outer{
	class Nested{
	
	}
}
Outer.Nested() // 이렇게 사용
```

## 내부 클래스
**내부클래스** : 혼자서는 객체를 만들 수 없고 외부 클래스의 객체가 있어야만 생성과 사용이 가능한 클래스

내부에 선언된 것이므로 *외부 클래스의 속성과 함수의 사용이 가능*하다. 

```kotlin
class Outer{
	inner class Nested{
	
	}
}
```

```kotlin
 fun main(){
     Outer.Nested().introduce()
     val outer = Outer()
     val inner = outer.Inner();
     
     inner.introduceInner();
     inner.introduceOuter()
     
     outer.text = "Changed Outer Class"
     inner.introduceOuter()
 }
 class Outer{
     var text = "Outer Class"
     
     class Nested{
         fun introduce(){
             println("Nested Class")
         }
     }
     inner class Inner{
         var text = "Inner Class"
         
         fun introduceInner(){
             println(text)
         }
         fun introduceOuter(){
             println(this@Outer.text)
         }
     }
 }
```
> 중첩클래스와 내부클래스는 클래스간의 연계성을 표현하여 코드의 가독성 및 작성 편의성이 올라갈 수 있으므로 적절한 상황에서 사용해야한다.

---
</div>
</details>

<details>
<summary>24. 데이터 클래스와 이넘 클래스 </summary> 
<div markdown='1'>

## 데이터클래스
**데이터클래스(Data class)** : 데이터를 다루는 데에 최적화된 class, 5가지 기능을 내부적으로 자동으로 생성해줌

1. 내용의 동일성을 판단하는 equals()의 자동구현
2. 객체의 내용에서 고유한 코드를 생성하는 hashcode()의 자동구현
3. 포함된 속성을 보기쉽게 나타내는 toString()의 자동구현
4. 객체를 복사하여 똑같은 내용의 새 객체를 만드는 copy()의 자동구현    
   copy함수는 똑같은 패러미터를 받을수도 있지만 패러미터를 주어서 해당 패러미터를 교체하여 생성할 수도 있다.
5. 속성을 순서대로 반환하는 componentX()의 자동구현

```kotlin
fun main(){
    val a = General("보영",212)
    println(a == General("보영",212))
    println(a.hashCode())
    println(a)
    
    val b = Data("루다",306)
    println(b == Data("루다",306))
    println(b.hashCode())
    println(b)
    
    println(b.copy())
    println(b.copy("아린"))
    println(b.copy(id = 412))
    println()
    val list = listOf(Data("보영",212),Data("루다",306),Data("아린",618))
    
    for ((a,b) in list){
        println("${a}, ${b}")
    }
}
class General(val name:String, val id:Int)

data class Data(val name:String, val id:Int)
```
## 이넘클래스
**이넘 클래스(Enum class)** : enumerated type 열거형의 준말이다.   
enum class안의 객체들은 관행적으로 상수를 나타낼때 표현하는 대문자로 표현한다.  
enum class의 객체들은 고유한 속성과 함수를 가질 수 있다.

```kotlin
fun main(){
	var state = State.SING
    println(state)
    
    state = State.SLEEP
    println(state.isSleeping())
    
    state = State.EAT
    println(state.message)
}
enum class State(val message:String){
    SING("노래를 부릅니다."),
    EAT("밥을 먹습니다."),
    SLEEP("잠을 잡니다.");
    
    fun isSleeping() = this == State.SLEEP
}
```
---
</div>
</details>

<details>
<summary>25. Set과 Map</summary> 
<div markdown='1'>

## Set
Set은 List와 달리 순서가 정렬되지 않으며 중복이 허용되지 않는 클래스이다.  
객체의 추가, 삭제가 가능한지 여부에 따라 Set과 mutableSet으로 나눈다.

```kotlin
fun main(){
	var a = mutableSetOf("귤","바나나","키위")
    
    for (item in a){
        println("${item}")
    }
    
    a.add("자몽")
    println(a)
    
    a.remove("바나나")
    println(a)
    
    println(a.contains("귤"))
}
```
## Map
Map은 객체를 넣을때, 그 객체를 찾아낼수 있는 key를 쌍으로 넣어주는 컬렉션이다.   
key ,value

객체의 위치가 아니라 key를 통해서 객체의 위치를 찾는다.   
같은 key에 다른 객체를 넣으면 기존의 객체가 대체되니 주의해서 사용해야한다.   
객체의 추가, 삭제가 가능한지 여부에 따라 Map과 mutableMap 으로 나눈다.
```kotlin
fun main(){
    val a = mutableMapOf("레드벨벳" to "음파음파",
                        "트와이스" to "FANCY",
                        "ITZY" to "ICY")
    for (entry in a){
        println("${entry.key} : ${entry.value}")
    }
    
    a.put("오마이걸","번지")
    println(a)
    
    a.remove("ITZY")
    println(a)
    
    println(a["레드벨벳"])
}
```
---
</div>
</details>

<details>
<summary>26. 컬렉션 함수 #1</summary> 
<div markdown='1'>

## 컬렉션 함수  
컬렉션 함수 : list, set, map 과 같은 컬렉션 또는 배열에 일반 함수 또는 람다 함수 형태를 사용하여 for 문 없이 아이템을 순회하며 참조하거나 조건을 걸고, 구조의 변경까지 가능한  여러가지 함수를 지칭

- forEach
- filter 
- map 
- any, all, none
- first, last
- firstOrNull, lastOrNull
- count
```kotlin
fun main(){
    var nameList = listOf("박수영","김지수","김다현","신유나","김지우")
	
    nameList.forEach{print(it+" ")}
    println()
    
    println(nameList.filter{it.startsWith("김")})
    
    println(nameList.map{"이름 : "+ it})
    
    println(nameList.any{it=="김지연"})
    println(nameList.all{it.length ==3})
    println(nameList.none{it.startsWith("이")})
    
    println(nameList.first{it.startsWith("김")})
    println(nameList.last{it.startsWith("김")})
    println(nameList.count{it.contains("지")})
}
```
---
</div>
</details>

<details>
<summary>27. 컬렉션 함수 #2</summary> 
<div markdown='1'>

### **1. associateBy, groupBy, partition**
associateBy : 아이템에서 key를 추출하여 map으로 변환하는 함수
groupBy : key가 같은 아이템 끼리 배열로 묶어 map으로 만드는 함수
partition : 아이템에 조건을 걸어 두개의 컬렉션으로 나누어줌 그리고 이를 Pair객체에 담아준다.

```kotlin
fun main(){
    data class Person(val name:String, val birthYear:Int)
 	
    val personList = listOf(Person("유나",1992),
                           Person("조이",1996),
                           Person("츄",1999),
                           Person("유나",2003))
    
    println(personList.associateBy{it.birthYear})
    println(personList.groupBy{it.name})
    
    val (over98,under98) = personList.partition{it.birthYear>1998}
    println(over98)
    println(under98)
}
```
### **2. flatMap, getOrElse, zip**
flatMap : 아이템마다 만들어진 컬렉션을 합쳐서 반환하는 함수
getOrElse : 인덱스 위치에 아이템이 있으면 아이템을 반환하고 아닌 경우 지정한 기본값을 반환하는 함수
zip : 컬렉션 두 개의 아이템을 1:1로 매칭하여 새 컬렉션을 만들어 줌
```kotlin
fun main(){	
    val numbers = listOf(-3,7,2,-10,1)
    
    println(numbers.flatMap{listOf(it*10,it+10)})
    
    println(numbers.getOrElse(1){50})
    println(numbers.getOrElse(10){50})
    
    val names = listOf("A","B","C","D")
	
    println(names zip numbers)
}
```
---
</div>
</details>

<details>
<summary>28. 변수의 고급 기술, 상수, lateinit, lazy</summary> 
<div markdown='1'>

## 상수
- var은 한번 할당한 객체가 있더라도 다른 객체로 변경해서 할당 가능하다.   
- val은 한번 할당한 객체가 있다면 다른 객체로 변경이 불가하다

`주의! val은 할당된 객체를 바꿀 수 없을 뿐이지 객체 내부의 속성을 변경할 수 없는것은 아니다.`

**상수** : 컴파일 시점에 결정되어 절대 바꿀 수 없는 값이다.   
const를 앞에 붙여서 선언   
상수의 이름은 이례적으로 대문자와 언더바만 사용한다.   
상수로 선언할 수 있는 것은 기본 자료형만 가능하다.   

companion object안에 선언하여서 객체의 생성과 관계없이 클래스와 관계된 고정적인 값으로만 사용하게 된다.   
  
>상수를 사용하는 이유는 변수의 경우 런타임시 객체를 생성하는데 시간이 더 소요되어 성능의 하락이 있기 때문   
>고정적으로 사용하는 값을 상수를 통해서 객체의 생성없이 성능을 향상시킬 수 있다.
```kotlin
fun main(){	
	val foodCourt = FoodCourt()
    
    foodCourt.searchPrice(FoodCourt.FOOD_CREAM_PASTA)
	foodCourt.searchPrice(FoodCourt.FOOD_STEAK)
	foodCourt.searchPrice(FoodCourt.FOOD_PIZZA)

}
class FoodCourt{
    fun searchPrice(foodName:String){
        val price = when(foodName){
            FOOD_CREAM_PASTA -> 13000
            FOOD_STEAK -> 25000
            FOOD_PIZZA -> 15000
            else -> 0
        }
        println("${foodName}의 가격은 ${price}원 입니다.")
    }
    companion object{
        const val FOOD_CREAM_PASTA ="크림파스타"
        const val FOOD_STEAK = "스테이크"
        const val FOOD_PIZZA = "피자"
    }
 }
```
## 늦은초기화
변수를 선언할때 객체를 할당 하지 않는경우에는 컴파일이 되지 않는다.
**lateinit**을 통해서 초기값의 할당은 나중에 할 수 있도록 하는 키워드

#### **lateinit var 변수의 제한사항**
초기값 할당 전까지 변수를 사용할 수 없음 (에러발생)   
기본 자료형에는 사용할 수 없음(String 클래스에는 사용 가능)

lateinit 변수가 초기화 하였는지 여부를 확인할때는 `::a.isinitialized` 를 통해 확인가능하다.   
이를 통해 오류를 막을 수 있다.
```kotlin
fun main(){
    var a = LateInitSample()
    println(a.getLateInitText())
    a.text = "새로 할당한 값"
    println(a.getLateInitText())
}
class LateInitSample{
    lateinit var text: String
    
    fun getLateInitText():String{
        if(::text.isInitialized){
            return text;
        }else {
            return "기본값";
        }
    }
}
```
## 지연 대리자 속성
변수를 사용하는 시점까지 초기화를 자동으로 늦춰주는 지연 대리자 속성(lazy delegate properties)

```kotlin
val a : Int by lazy{7}
```
코드에서는 선언시 즉시 객체를 생성 및 할당하여 변수를 초기화 하는 형태를 갖고 있지만, 실제 실행시에는 변수를 사용할 때 초기화 된다.   
코드의 실행시간을 최적화 할 수 있는 코드이다.
람다함수로 초기화가 진행되므로 여러개의 구문이 들어갈 수 있다.(맨 마지막 값이 결과값이 되므로)
```kotlin
fun main(){
    val number:Int by lazy{
        println("초기화를 합니다.")
        7
    }
    
    println("코드를 시작합니다.")
    println(number)
    println(number)
}
```
---
</div>
</details>

<details>
<summary>29. 비트연산</summary> 
<div markdown='1'>

## 비트연산

10진수를 2진법으로 계산   
코틀린에서 정수형은 부호를 포함하므로 최상위 비트가 부호비트로 사용된다.   
따라서 최상위 비트에는 정보를 담지 않는 것이 좋다.
- shl(<<), shr(>>), ushr(>>>)
- and 비트를 확인하는 방법으로 주로 사용, 비트를 clear하는 방법으로 사용
- or 비트의 set연산을 할때 주로 사용
- xor 비트들이 같은지 비교하는 방법
- inv 비트를 반전시키는 역할

```kotlin
fun main(){
    var bitData:Int = 0b10000

    bitData = bitData or(1 shl 2)
    println(bitData.toString(2))
    
    var result = bitData and(1 shl 4)
    println(result.toString(2))
        
    println(result shr 4)
    
    bitData = bitData and((1 shl 4).inv())
	println(bitData.toString(2))
    
    println((bitData xor(0b10100)).toString(2))
}
```
---
</div>
</details>

<details>
<summary>30. 코루틴을 통한 비동기 처리</summary> 
<div markdown='1'>

여러개의 루틴을 동시에 실행하여 결과를 내고싶다면? —> 비동기처리 == 코루틴
메인루틴 -> 실행과 종료를 결정할 수 있다. -> 코루틴

코루틴을 사용할 때는 `import kotlinx.coruotines.*` 을 import해야한다.

코루틴은 제어범위 및 실행범위를 지정할 수 있는데 이를 코루틴의 scope라고 한다.

GlobalScope와 CoroutineScope를 지원한다.
- GlobalScope : 프로그램 어디서나 제어 동작이 가능한 기본 범위
- CoroutineScope : 특정한 목적의 DIspatcher를 지정하여 제어 및 동작이 가능한 범위


CoroutineScope 를 만들때 적용가능한 DIspatcher
- Dispathcers.Default : 기본적인 백그라운드 동작
- Dispathcers.IO : I/O에 최적화 된 동작
- Dispathcers.Main : 메인(UI) 스레드에서 동작

코루틴은 이러한 스코프에서 제어되도록 생성될 수 있다.   
생성된 스코프에서 launch나 async를 통해서 코루틴을 생성할 수 있다. 

**launch** vs **async** -> 반환값이 있는지의 차이

- launch : 반환값이 없는 Job객체
- async : 반환값이 있는 Deffered 객체

두 함수는 람다함수의 형태를 가지고 있으며, 그렇기 떄문에 async는 마지막 결과값이 반환된다.

코루틴은 제어되는 스코프 또는 프로그램 전체가 종료되면 함께 종료된다.   
코루틴이 끝까지 실행되는 것을 보장하려면 일정한 범위에서 코루틴이 모두 실행될 때까지 잠시 기다려줘야한다.
```kotlin
import kotlinx.coroutines.*
fun main(){
    val scope = GlobalScope
    
    scope.launch{
        for(i in 1..5){
            println(i)
        }
    }
}
```

runBlocking을 통해서 코루틴이 종료될때까지 메인 루틴을 잠시 대기시켜준다.
```kotlin
import kotlinx.coroutines.*
fun main(){
    runBlocking{
        launch{
            for(i in 1..5){
                println(i)
            }
        }
    }
}
```
루틴의 대기를 위한 추가적인 함수들도 있다.   
- delay(milisecond:Long) : milisecond단위로 루틴을 잠시 대기시키는 함수
- Job.join() : Job의 실행이 끝날때까지 대기하는 함수
- Defferred.await() : Defferred의 실행이 끝날때까지 대기하는 함수   
  —> Defferred의 결과도 반환한다.

세 함수는 코루틴 내부 또는 runBlocking과 같은 루틴의 대기가 가능한 구문 안에서만 동작이 가능
```kotlin
import kotlinx.coroutines.*
fun main(){
    runBlocking{
        val a = launch{
            for(i in 1..5){
                println(i)
                delay(10)
            }
        }
        val b = async{
            "async 종료"
        }
        println("async 대기")
        println(b.await())
        
        println("launch 대기")
        a.join()
        println("launch 종료")
    }
}
```
코루틴 실행도중 중단하는 방법   
코루틴에 cancel()을 걸어주면 다음 두가지 조건이 발생하며 코루틴을 종료할 수 있다.   
1. 코루틴 내부의 delay()함수 또는 yeild()함수 가 사용된 위치까지 수행된 뒤 종료됨
2. cancel()로 인해 속성인 isActive가 false가 되므로 이를 확인하여 수동으로 종료함

```kotlin
import kotlinx.coroutines.*
fun main(){
    runBlocking{
        val a = launch{
            for(i in 1..5){
                println(i)
                delay(10)
            }
        }
        val b = async{
            "async 종료"
        }
        println("async 대기")
        println(b.await())
        
        println("launch 취소")
        a.cancel()
        println("launch 종료")
        
    }
}
```
제한시간 내에 수행되면 결과값을 아닌경우 null을 반환하는 함수
withTimeoutOrNull()   
이 함수도 join이나 await처럼 blocking함수다. 
```kotlin
import kotlinx.coroutines.*
fun main(){
    runBlocking{
        val result = withTimeoutOrNull(50){
            for(i in 1..10){
                println(i)
                delay(10)
            }
            "Finish"
        }
        println(result)
    }
}
```
---
</div>
</details>