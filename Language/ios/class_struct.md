# Class & Struct

- 다른 프로그래밍 언어와 달리 Swift는 사용자 정의 구조체와 클래스에 대해 별도의 인터페이스와 구현 파일을 만들 필요가 없습니다. Swift에서 단일 파일로 구조체 또는 클래스를 정의하면 해당 클래스 또는 구조체에 대한 외부 인터페이스가 자동으로 다른 코드에서 사용할 수 있습니다.

<br>

구조체와 클래스는 다음과 같은 공통점을 가집니다.
- 값을 저장하는 프로퍼티 정의
- 기능 제공을 위한 메서드 정의
- 서브 스크립트 구문을 사용하여 값에 접근을 제공하는 서브 스크립트 정의
- 초기화 상태를 설정하기 위한 초기화 정의
- 기본 구현을 넘어 기능적 확장을 위한 확장
- 특정 종류의 표준 기능을 제공하는 프로토콜 준수

<br>

구조체는 struct 키워드로 클래스는 class 키워드로 시작해 정의할 수 있습니다. 구조체나 클래스의 변수를 속성(property)이라고 하고 함수를 메서드(method)라고 합니다. 
```
class Dog {
    var name = "강아지"
    var weight = 0
    
    func sit() {
        print("앉았습니다.")
    }
    
    func layDown() {
        print("누웠습니다.")
    }
}

struct Bird {
    var name = "새"
    var weight = 0.0
    
    func fly() {
        print("날아갑니다.")
    }
}
```
<br>

클래스와 구조체 둘다 메모리에 찍어낸 것을 인스턴스(instance)라고 하고 인스턴스는 실제로 메모리에 할당되어 구체적 실체를 갖춘 것입니다.
구조체와 클래스 모두 새로운 인스턴스를 위해 초기화 구문을 사용합니다. 가장 간단한 형태는 구조체나 클래스 이름 뒤에 빈 소괄호를 붙여 사용하는 것입니다. 이렇게 되면 모든 프로퍼티가 기본값으로 초기화되는 클래스 또는 구조체의 새로운 인스턴스를 생성합니다.
이러한 인스턴스의 프로퍼티에 접근하기 위해 점 구문(.)을 사용합니다.

```
var bori = Dog()
var aBird = Bird()

bori.weight	= 0.3
bori.weight			// 0.3
aBird.fly()			// 날아갑니다.
```
<br>

# 구조체와 클래스의 차이점
구조체와 클래스의 차이점을 공부하면서 그냥 그렇구나 하면서 넘어갔던 부분들을 메모리 구조와 연관지어 왜 차이가 생기는 지를 확실하게 알게되었고 이를 공부하고 정리해보았습니다.

프로그램이 실행하게 되면 OS는 메모리에 공간을 할당해주는데 할당해주는 메모리 공간은 Code, Data, Heap, Stack 4가지가 있습니다.

![](https://user-images.githubusercontent.com/35067611/105793085-d9ae2300-5fcb-11eb-824d-7d389fc2db29.png)

Code 영역은 우리가 작성한 **소스코드**가 기계어 형태로 들어 가는 부분으로 CPU가 순차적으로 한줄씩 실행합니다. 프로그램 실행 중 코드가 바뀌면 안되니 Read-Only로 저장됩니다.

Data 영역은 **전역변수나 타입속성**이 할당되는 영역으로 프로그램의 시작과 동시에 할당되고, 프로그램이 종료되어야 메모리가 소멸되는 영역입니다.

Heap 영역은 사용자에 의해 메모리 공간이 동적으로 할당되고 해제되며 **클래스의 객체나 클로저 같은 참조타입의 값**이 할당됩니다. 사용하고 난 후에는 메모리 해제를 해줘야 하지만 스위프트는 ARC를 통해 힙에 할당된 메모리가 더 이상 쓸모 없어지면(참조되지 않으면) 자동으로 해제해준다고 합니다.
_ARC도 나중에 꼭 공부해서 정리해봐야겠어요_

Stack 영역은 함수의 실행시 필요 데이터가 생성되고 사용 완료 후에는 저장된 메모리가 해제됩니다.

#### Stack vs Heap
스택영역은 CPU가 효율적으로 메모리를 구성하기에 최적화가 되어있어 속도가 빠르고 메모리를 직접 해제하지 않아도 되는 반면 힙 영역은 메모리를 개발자가 직접 관리해야하므로 메모리 누수의 위험성이 있고 할당/해제 작업으로 인해 속도가 스택에 비해 느립니다.
또 스택영역은 메모리 크기에 대한 제한이 있지만 힙 영역은 컴퓨터에 남아있는 메모리 내에서는 메모리 크기에 대한 제한이 없습니다.


<br>

Swift의 Type은 크게 Value Type(값 타입)과 Reference Type(참조 타입)으로 나뉩니다. 값 타입은 변수 또는 상수에 할당될 때나 함수로 전달될 때 복사 되는 값인 타입이고 참조 타입은 복사본 대신에 존재하는 같은 인스턴스에 대한 참조가 사용됩니다.

구조체는 Value Type(값 타입), 클래스는 Reference Type(참조 타입)입니다. 
이를 메모리 구조 관련해서 얘기를 해보면 값 타입인 구조체의 인스턴스(그 중 프로퍼티. 메서드는 데이터 영역에 저장된다.)는 Stack 영역에 데이터를 복사해서 메모리를 할당하고, 참조 타입인 클래스의 인스턴스는 Heap 영역에 메모리를 할당하고 함수를 실행하게 되면 Heap의 주소를 전달해줍니다. 구조체나 클래스 자체는 데이터 영역에 저장돼요.

> Value Type 값 -> Stack에 메모리 할당
Reference Type 값 -> Heap에 메모리 할당

~~추가) Value Type이 Heap에, Reference Type 이 Stack에 저장되는 상황도 있다고 합니다. [참고](https://sujinnaljin.medium.com/ios-swift%EC%9D%98-type%EA%B3%BC-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%A0%80%EC%9E%A5-%EA%B3%B5%EA%B0%84-25555c69ccff)~~
```
class Person {
    var name = "사람"
}



struct Animal {
    var name = "동물"
}




var p = Person()    // x1234  <- Heap의 주소를 가리킴
p.name              // 사람

var a = Animal()
a.name              // 동물



var p2 = p       // (클래스)     // x1234 <- 메모리 주소가 복사되서 담김


p.name = "혜리"

p.name          // 해리
p2.name         // 해리
    




var a2 = a       // (구조체)
a.name = "강아지"

a.name          // 강아지
a2.name         // 동물

```
이런 차이가 인스턴스 선언시 상수(let)나 변수(var)로 생성할 때에도 영향을 미칩니다. 

> - 인스턴스 상수(let)으로 선언시
구조체 -> 저장 속성이 전부 상수로 선언됨
클래스 -> 가르키는 인스턴스 고정(저장 속성은 각 let/var 선언에 따름)
- 인스턴스 변수(var)로 선언시에는 구조체도 저장 속성의 각 let/var 선언에 따른다.

그리고 상속에서도 차이를 보입니다. 구조체는 상속이 불가능한 반면에 클래스는 상속이 가능합니다. 클래스만이 상속 가능합니다. 
상속에 대해서는 다음에 또 자세히 다뤄볼게요.

## 언제 구조체를 사용하고 언제 클래스를 사용하나!
- 연관된 데이터들을 단순히 묶는 것이 목적일 때는 **구조체**가 낫습니다.
- 구조체에 저장된 저장 속성들이 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때 **구조체**를 사용합니다.

- 해당 모델을 serialize해서 전송하거나 파일로 저장할 경우가 발생하면 **클래스**를 사용합니다.
- Objectice-C와 상호 운용성을 원할 때는 **클래스**를 사용합니다.
	- Objective-C에서 지원하는 API를 사용해야 할 때는 Objective-C의 클래스를 상속받아 사용해야 할 수 있습니다. 이런 경우에는 클래스를 사용해야 합니다.
- 고유한 값을 제어해야 할 때는 **클래스**를 사용하세요.
	- 클래스는 참조 타입이기 때문에 스위프트에서 고유한 값으로 사용됩니다. 참조 타입을 사용하면 앱의 여러 곳에서 사용하더라도 한 영역에서 적용한 수정이 다른 영역에서도 적용됩니다. 주로 파일 관리나 네트워크 연결과 같은 작업을 다룰 때 클래스를 사용합니다.


[참고]
- https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html
- https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes
- https://sihyungyou.github.io/iOS-Memory-Architecture/
- https://sujinnaljin.medium.com/ios-swift%EC%9D%98-type%EA%B3%BC-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%A0%80%EC%9E%A5-%EA%B3%B5%EA%B0%84-25555c69ccff
