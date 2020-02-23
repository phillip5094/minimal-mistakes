---
title: "Java-Stream"
categories: 
  - 공부
last_modified_at: 2020-02-23T13:00:00+09:00

toc: true
---
2020.02.23

## java.util.stream
stream의 요소를 람다식으로 처리한다.

```
int sum = widgets.stream()
                 .filter(b -> b.getColor() == RED)
                 .mapToInt(b -> b.getWeight())
                 .sum();
```

## Stream과 Collection의 차이점
- Stream은 element를 저장하는 구조가 아닌, 파이프라인을 통해 element를 전달한다.
- Stream 연산이 원본을 수정하지 않는다.
- Stream 연산은 lazy하게 처리된다. (불필요한 연산을 피할 수 있다)
- 동일한 element를 다시 방문하고 싶다면, 새로운 Stream을 생성해야 한다.

## Stream pipeline 3단계
Stream 생성, 중간 처리, 최종 처리

```
int sum = widgets.stream()                          // Stream 생성
                 .filter(b -> b.getColor() == RED)  // 중간 처리
                 .mapToInt(b -> b.getWeight())      // 중간 처리
                 .sum();                            // 최종 처리
```

### Stream 생성
Stream은 순차적 또는 병렬적으로 처리가 가능하다.

```
Stream stream1 = widgets.stream();          // Serial Stream
Stream stream2 = widgets.parallelStream();  // Parallel Stream
```

Stream은 4종류가 있다.
- Stream
- IntStream
- LongStream
- DoubleStream

### 중간 처리
전달받은 Stream을 중간 처리하고 처리된 Stream을 반환한다.<br>
<img src="https://github.com/phillip5094/phillip5094.github.io/blob/master/imgs/stream_pipeline.png" width="70%"/><br>
대표적인 중간 처리 연산을 소개하겠습니다.<br>

- distinct<br>
Object.equals(Object) == true 인 경우, 중복을 제거한다.
- filter(Predicate)<br>
Predicate == true 인 element를 추출한다.

```
list.stream().filter(n -> n.startsWith("a"));   // a로 시작하는 element 필터링
```

- flatMap(Function)<br>
element를 Function을 통해서 새로 구성한다.<br>
하나의 element가 여러 개의 element로 바뀔 수 있다.

```
list.stream().flatMap(n -> Arrays.stream(n.split(' ')));       // element를 split하고 새로운 stream 생성 
```

- map(Function)<br>
element를 Function을 통해서 새로 구성한다.<br>

```
public class Person{
    String name;
}
persons.stream().map(person -> person.getName());
```

- sorted<br>
Stream element를 정렬한다. 오름차순 정렬은 하는 방법은 아래 3가지가 있다.

```
public class Person implements Comparable<Person>{
    int age;

    @Override
    public int compareTo(Person p){
        return Integer.compare(age, p.age);
    }
}

persons.stream().sorted();
persons.stream().sorted((a, b) -> a.compareTo(b));
persons.stream().sorted(Comparator.naturalOrder());
```

- peek<br>
각 element에 대해서 동작을 수행한다. 디버깅 용도로 많이 사용한다.

```
persons.stream()                                    // Stream 생성
       .map(person -> person.getName())             // person element를 name으로 바꿈
       .peek(name -> System.out.println(name))      // element 출력
       .collect(Collectors.toList());               // 결과 stream을 list에 저장
```

### 최종 처리
- allMatch(Predicate) / anyMatch(Predicate) / noneMatch(Predicate) <br>
allMatch : 모든 element가 Predicate를 만족하는지 검사하고 true/false를 반환한다.<br>
anyMatch : 적어도 1개의 element가 Predicate를 만족하는지 검사하고 true/false를 반환한다.<br>
noneMatch : 모든 element가 Predicate를 만족하지 않는지 검사하고 true/false를 반환한다. 

```
int[] arr = {2, 3, 4, 5};
boolean result1 = Arrays.stream(arr).allMatch(a -> a >= 2);         // result1 : true
boolean result2 = Arrays.stream(arr).anyMatch(a -> a % 3 == 0);     // result2 : true
boolean result3 = Arrays.stream(arr).noneMatch(a -> a % 3 == 0);    // result3 : false     
```

- forEach<br>
Stream의 각 element에 대해서 연산을 수행한다.

```
list.stream()                                   // Stream 생성
    .filter(n -> n % 2 == 0)                    // element 중 짝수 추출                  
    .forEach(n -> System.out.println(n));       // 추출된 element 출력
```

<참고자료 및 서적><br>
https://docs.oracle.com/javase/8/docs/api/index.html?java/util/stream/package-summary.html<br>
https://palpit.tistory.com/648<br>
신용권,이것이 자바다,한빛미디어,2015
