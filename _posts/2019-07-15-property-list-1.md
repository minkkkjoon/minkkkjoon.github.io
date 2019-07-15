---
title: "데이터 저장 - 프로퍼티 리스트 (1)"
elements:
  - content
  - css
  - formatting
  - html
  - markup
categories:
  - Swift
tags:
  - Swift
  - Data
  - Propertylist
  - UserDefaults



---





### 1.1 프로퍼티 리스트(Property List)란?

프로퍼티 리스트는 애플의 주요 소프트웨어 프레임워크에 이용되는 **객체 직렬화를 위한 XML 형식의 파일**이다. 

애플에서는 **간단한 데이터 계층을 표현하기 위한 추상화**라고 정의하고 있기도 하다.

쉽게 말해 비교적 **간단한 데이터를 XML 포맷에 맞추어 키(Key) - 값(Value) 형식으로 저장**하는 것이라고 할 수 있다.

<br>

프로퍼티 리스트는 대부분 앱의 공통 데이터나 주요 설정, 환경 설정 정보를 저장하는 용도로 사용된다. iOS 기반의 프로젝트에서는 항상 Info.plist 파일을 찾아볼 수 있는데, 이는 앱의 빌드와 실행에 필요한 환경 설정값을 저장하는 대표적인 시스템 프로퍼티 리스트 파일입니다.

<br>

![info-plist](/images/info-plist.png)

------

*프로젝트에 포함된 Info.plist 파일*

<br>

프로퍼티 리스트는 데이터의 타입을 추상화하여 저장한다. 여기서 추상화는 정확하고 구체적인 형태를 가리키는 구체화의 반대말로서, **일체의 개별적인 특성을 배제하고 공통성을 띠는 것**을 말한다.



우리가 프로그래밍 코드에서 문자열을 다룰 때 사용하는 데이터 타입은 대부부 구체화된 타입이다.

스위프트에서 제공하는 String, 파운데이션 프레임워크에서 제공하는 NSString, 코어 파운데이션 프레임워크에서 제공하는 CFString 타입이 있는데 각 타입은 모두 자신만의 독특한 특성이나 속성을 지니고 있다.



예를 들어, **String 타입**은 개별 문자열을 Character 타입으로 변환할 때는 **.character 속성**이 있고 문자열의 길이를 구할 때에는 **.character.count 속성**이 있지만 **NSString 타입**은 Character 타입으로 변환할 수 있는 **속성이 없고** 문자열의 길이를 구할 때에는 **.length 속성**을 사용한다. 이처럼 구체적인 타입들 각각의 특성이 모두 다르다.



하지만 이와 같은 각 타입의 문자열 데이터가 프로퍼티 리스트에 저장될 떄에는 모두 "문자열" 이라는 공통의 타입으로 추상화 된다. 정확히는 <string> 타입이지만 스위프트의 String 과는 이름만 같을 뿐, 의미적으로는 다른 타입이다.



즉, 각각의 타입이 갖는 개별적이고 구체적인 특성은 모두 배제한 채 "문자열"이라는 공통된 특성만 추출(**추상화**)하여 <string> 타입으로 바꾸었기 때문에 String 타입으로 저장을 했어도 .character 속성을 사용할 수 없고 NSString 타입으로 저장을 했어도 .length  속성을 사용할 수 없다.



하지만 덕분에 프로퍼티 리스트에 저장된 데이터는 연관된 어느 타입으로든 읽어 올 수 있다.

String 타입의 값을 저장했더라도 이를 NSString 타입의 형태로 읽어올 수 있고, CFString 으로 읽어올 수도 있다.

<br>

![plist-data-abstract](/images/plist-data-abstract.png)

<br>

주의할 것은 프로퍼티 리스트가 데이터의 타입을 추상화하는 것이지, 결코 데이터 자체를 추상화하는 것은 아니라는 점입니다. 데이터 자체는 추상화할 수 없을 뿐더러, 추상화해서도 안 됩니다.

추상화하는 순간 값의 유실이 발생하기 때문입니다. 데이터는 그대로 보존하되 타입만 추상화하는 것이 프로퍼티 리스트에서 데이터를 저장하는 메커니즘의 핵심이라고 할 수 있습니다.

<br>

<br>

### 1.2 프로퍼티 리스트와 데이터 타입

<br>

프로퍼티 리스트에 저장할 수 있는 데이터 타입은 크게 두 가지로 구분할 수 있다.

하나는 **원시 타입(Primitive Data Type)**으로, 스위프트에서 제공하는 String, Int, Float, Double, Bool 등이 이에 해당한다. 이 타입으로 선언된 데이터들은 모두 프로퍼티 리스트에 저장할 수 있다.

또 다른 하나는 **레퍼런스 타입(Reference Data Type)**으로, 파운데이션 프레임워크에서 제공하는 NSString, NSNumber, NSDate, NSData 등이 여기에 속하며 코어 파운데이션 프레임워크에서 제공하는 CFString, CFNumber, CFDate, CFData 등도 역시 이 분류에 속한다.

프로퍼티 리스트에는 컨테이너 형태의 집합 자료형도 저장할 수 있다. 스위프트에서 제공하는 **Array, Dictionary** 파운데이션 프레임워크에서 제공하는 **NSArray, NSDictionary** 코어파운데이션에서 제공하는 **CFArray, CFDictionary** 등이 대표적인 집합 자료형들이다.

<br>

| <center>타입</center> | <center>XML 엘리먼트</center> | <center>스위프트</center> | <center>파운데이션</center> | <center>코어파운데이션</center> |
| :-------------------: | :---------------------------- | :------------------------ | --------------------------- | ------------------------------- |
|         배열          | <array>                       | **Array**                 | NSArray                     | CFArray                         |
|       딕셔너리        | <dict>                        | **Dictionary**            | NSDictionary                | CFDictionary                    |
|        문자열         | <string>                      | **String**                | NSString                    | CFString                        |
|         날짜          | <date>                        | -                         | NSDate                      | CFDate                          |
|        Base64         | <data>                        | -                         | NSData                      | CFData                          |
|         정수          | <integer>                     | Int, UInt                 | NSNumber                    | CFNumber                        |
|         실수          | <real>                        | Float, Double             | NSNumber                    | CFNumber                        |
|        논리형         | <true/>,<false/>              | Bool                      | NSNumber                    | CFNumber                        |

*프로퍼티 리스트이 XML 엘리먼트와 데이터 타입*

<br>
