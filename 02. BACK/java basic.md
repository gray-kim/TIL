# JAVA

## JAVA Stream

### java stream filter predicate 
Java 8 이상에서 제공하는 Stream API에서 filter() 메소드는 Predicate를 인자로 받습니다.

Predicate는 입력값을 받아서 boolean 값을 리턴하는 함수형 인터페이스입니다. 즉, 조건식을 표현할 수 있는 함수입니다.

Stream의 filter() 메소드에서는 Predicate를 사용하여 조건에 맞는 요소만을 선택할 수 있습니다. 예를 들어, 다음과 같은 코드에서는 짝수인 요소만 선택합니다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
List<Integer> evenNumbers = numbers.stream()
                                   .filter(n -> n % 2 == 0)
                                   .collect(Collectors.toList());
```

위 코드에서 `filter(n -> n % 2 == 0)` 부분이 Predicate입니다. 이 Predicate는 입력값이 짝수인지 아닌지를 검사하여 true/false 값을 반환합니다. `filter()` 메소드는 이 Predicate를 이용하여 Stream에서 짝수인 요소만 선택합니다.

Predicate는 람다식을 이용하여 구현할 수 있으며, `test()` 메소드를 이용하여 입력값을 검사하여 boolean 값을 반환합니다. 예를 들어, 다음과 같은 코드에서는 문자열의 길이가 5 이상인지를 검사하는 Predicate를 구현합니다.

```java
Predicate<String> lengthPredicate = s -> s.length() >= 5;
```

위 코드에서 `s -> s.length() >= 5` 부분이 Predicate입니다. 이 Predicate는 입력값이 문자열의 길이가 5 이상인지 아닌지를 검사하여 true/false 값을 반환합니다. `test()` 메소드를 이용하여 입력값을 검사할 수 있습니다.

```java
String str = "Hello, world!";
if (lengthPredicate.test(str)) {
    System.out.println("The length of the string is greater than or equal to 5.");
}
```

위 코드에서 `test(str)` 부분은 입력값 `str`을 검사하여 문자열의 길이가 5 이상인지 아닌지를 판단합니다. 이 결과가 true이면 "The length of the string is greater than or equal to 5."을 출력합니다.


- 혼합 예제  
아래처럼 Predicate를 사용해서 filter에 여러가지 조건을 넣을 수 있음.
```java
private SearchResult findSearchFilterPredicate(String standDt, long searchCnt){
        Predicate<SearchDto> cond1=entity->standDt.compareTo(entity.getBeginDt())>=0 && standDt.compareTo(entity.getEndDt())<=0;
        Predicate<SearchDto> cond2=entity->searchCnt>=entity.getOverCnt() && searchCnt<=entity.getBelowCnt();
        return listSearchResult
        .stream()
        .filter(cond1.and(cond2))
        .findFirst()
        .orElse(null);
}
```
