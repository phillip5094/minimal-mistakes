# 2장 스위프트 처음 시작하기
기본적으로 문장 끝에 세미콜론은 붙이지 않아도 상관없습니다.
<br>
한 줄 끝을 명시적으로 보여주기 때문에 세미콜론 붙이는 방식을 좋아합니다.

## 문자열 보간법
문자열 + 연산 대신에 더욱 편리하게 문자열을 출력할 수 있는 방법
```
var name1:String = "Alice";
var name2:String = "Bob";
print("My name is " + name1 + ". Nice to meet you.");
print("My name is \(name2). Nice to meet you.");
```

## 변수 상수
* var : 변수
* let : 상수
* Javascript처럼 var을 생략해서 사용할 수 있지만, 저는 변수라고 명시적으로 보여주는 것을 더 선호하기 때문에 붙입니다.

# 3장 데이터 타입 기본
스위프트의 모든 데이터 타입 이름은 Upper Camel Case를 사용합니다.

## Int vs UInt
* Int : 부호를 포함한 정수 (Int8, Int16, Int32, Int64)
* UInt : unsigned int (UInt8, UInt16, UInt32, UInt64)
