**프로젝트명: 묵찌빠 게임**

**프로젝트 설명:** 유저로부터 console 입력을 받아 컴퓨터와 묵찌빠 게임을 실행하는 프로그램

**진행기간: 2021/03/01 ~ 2021/03/05 (1주)**

**프로젝트 진행인원: 2인**

## 주요 학습 내용

- Optional Chaining의 사용법
- 변수와 함수의 naming
- Enum의 활용법
- class를 통한 기능과 역할의 분리
- commit시에 앞에 키워드를 붙여 어떤 작업을 진행했는지 명확해지도록 작업



## 프로젝트 진행 과정

**Step1: 가위바위보 클래스 구현**

- 사용자 입력에 따라 게임이 계속해서 진행되도록 하는 `startGame()` 구현
- switch문을 이용해 GameResult를 반환해주는 `returnResultOfGame()` 구현
- 사용자 입력을 받고 그 입력이 유효한지 확인해주는 `getUserInput()`구현
- enum을 활용하여
  - 게임 결과를 저장하는 `GameResult`구현
  - 사용자와 컴퓨터의 손 모양을 저장하는 `Hand`구현
  - 에러를 처리하기 위해 필요한 `GameError` Error 프로토콜 구현

**Step2: 묵찌빠 클래스 구현**

- enum을 활용하여
  - 현재 진행중인 게임에서 누가 공격권을 가지고 있는지를 관리하는 `GameTurn` 구현
  - nested enum을 활용하여 지역성을 부여
- 게임의 종료 조건의 체크와 인터페이스 표시를 위한 메서드 구현



## 주요 피드백과 개선내역

**Step1 PR 링크:** https://github.com/yagom-academy/ios-rock-paper-scissors/pull/26

**Step2 PR 링크:** https://github.com/yagom-academy/ios-rock-paper-scissors/pull/38

<br/> 

> 피드백 1: 함수명이 조금 아쉬운 것 같습니다! 양쪽 손을 비교한다는 의미는 전달이 됐지만, 보통 compare라 하면 대소를 비교하는데 많이 사용하고, GameResult라는 타입을 반환하기 때문에 조금 더 명확하게 바뀌면 좋을 것 같아요.

`compareHands(_ user: Int, with computer: Int) -> GameResult` 메서드의 메서드명이 아쉽다는 피드백이었다.

`compareHands` 메서드는 유저가 낸 가위, 바위, 보 중 하나와 컴퓨터가 낸 가위, 바위, 보 중 하나를 비교하여 누가 이겼는지를  GameResult로 반환하는 메서드이다.

게임의 결과를 반환하는 메서드이니 차라리 `gameResult`와 같은 직관적인 메서드명을 붙여주는게 어땠을까 하는 생각이 든다.

<br/> 

> 피드백 2: `Int`로 선언된 이 변수들은 1, 2, 3이 아닌 다른 값이 들어가도 괜찮은 걸까요?
> 두 프로퍼티 모두 0, 1, 2, 3 이라는 숫자밖에 갖지 않는데 Int 타입을 사용하는게 적합한지 의문이 들어서요! ㅎ

```swift
var handOfComputer = 0
var handOfUser = 0
```

기존에는 handOfComputer, handOfUser를 단순 Int값으로 저장하여 이용했었다. 하지만 enum에 대해 더 잘 이해하게 되자 한정된 값들의 집합은 enum으로 관리하는 것이 안전하고 효율적이다라고 생각하게되어 enum을 활용해 수정을 진행했다.

<br/> 

> 피드백 3: 무조건 동사로 시작해야한다는 생각을 조금 덜어내고 명사형 함수 네이밍도 도전해보는건 어떨까요?
> 스위프트 공식문서 참고해서 네이밍에 조금 더 고민해보면 좋을 것 같아요!

학교에서는 배우기 어려웠던 메서드의 네이밍에 대해서 더 깊게 고민해볼 수 있는 피드백이었다. 어떤 것을 반환해야 한다면 명사형으로 메서드명을 정해도 괜찮다는 것에 대해 알게되었다.

<br/> 

> 피드백 4: 이 turn의 String을 바로 이용하고 싶었던 거라면 enum의 CustomStringConvertible 프로토콜에 대해 공부해보면 좋을 것 같네요!

protocol에 대해 아직 생소하지만 enum, struct, class등의 객체는 프로토콜을 채택하여 특정 기능을 가지게 하는 효과를 볼 수 있다는 것에 대해 알게되었다. `CustomStringConvertible`을 채택함으로써 각 case가 description이라는 연산 프로퍼티를 통해 string 값을 반환하도록 할 수 있었다. 단순히 rawValue에 String을 담아놓고 그것을 사용하는 것보다 description을 통해 값을 사용함으로써 안전성과 신뢰성을 높일 수 있었다.



## 프로젝트를 진행하며 느낀점

**이번 프로젝트에 대해 피드백을 받으며 변수명 함수명을 더 자연스럽게 짓게 되었다.**

```swift
class RockPaperScissors {
  enum HandOfUserAndComputer {
    case scissor
    case rock
    case paper
  }
// ...
  private func rockPaperScissorsGameResult() {
    //...
  }
}
```

기존에는 위의 코드처럼 최대한 구체적으로 naming을 하려는 시도를 했었는데, `RockPaperScissors`라는 class 내부에 있는 메서드인 것이 분명하므로 접두사를 떼고 `gameResult()`로 사용해도 문맥상 어색하지 않다는 것을 알게되었다.

또, 함수명을 항상 동사로만 짓던 습관에서 벗어나 필요한 경우에는 명사형 네이밍을 사용하여 더 의미전달 측면에서 명확한 코드를 작성할 수 있게 되었다.

<br/> 

**enum의 활용에 익숙해졌다**.

enum을 활용하는 방법을 잘 몰랐기에 Error 프로토콜을 채택하여 에러 케이스들을 관리하는 용도로써만 사용해왔다.

하지만 이번 프로젝트를 통해 `CaseIterable`, `CustomStringConvertible`, `Comparable` 등의 다양한 프로토콜들에 대해 공부해보고 사용해보며 enum을 활용할 수 있는 다양한 방법들에 대해 알아보았다.

<br/> 

**Optional을 해제하는 여러 방법들에 익숙해졌다**.

Optional을 해제하는 과정이 필요하단 것은 알고 있었지만 이를 실제로 많이 써본적은 없었다. 

이번 프로젝트에선 optional chaining을 통한 여러 optional 값들의 해제, map의 활용을 통한 optional 해제 로직의 단순화 등 다양한 장소에서 여러 방법으로 작업을 진행해보며 optional에 대해 더 익숙해졌다.

<br/> 

**Force Unwrapping도 하나의 의사전달 수단이 될 수 있다는 것을 알게되었다.**

이번 프로젝트에서 피드백을 받기 이전에는 강제 언래핑에 대한 나의 인식은 절대 사용해서 안되는 최후의 수단에 가까웠다.

하지만 어쩔 수 없이 강제 언래핑을 진행해야되는 상황이나 명백하게 강제 언래핑이 가능한 상황에서는 강제 언래핑을 해주는 것이 의미전달 측면에서 더 좋을 수 있다는 점에 대해 알게되었다.

만약 코드에 nil coalescing(??의 활용)이 사용되었다면 다른 프로그래머는 이를 보고 

>  "정말 nil값이 나올 가능성이 있기 때문에 nil coalescing를 통해 default 값을 지정해주었구나"

와 같이 생각할 수 있기 때문에 nil이 아님을 보장하는 로직이 짧고 명확하다면 강제 언래핑을 사용해주는 것도 괜찮다는 토론이 이루어졌다.

일례로 CaseIterable을 채택한 enum의 값들을 allCases를 통해 배열로 만들어준 뒤, randomElement() 메서드를 통해 랜덤한 값을 구한다면

allCases를 통해 빈 배열이 아님을 확인할 수 있으므로 randomElement()의 결과값은 강제 언래핑을 진행해주어도 문제가 발생하지 않는다. 오히려 반드시 값이 존재한다는 확신을 줄 수 있다.



