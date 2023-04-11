# JPA, QueryDSL

### Q클래스는 연결된 db에 있는 테이블을 알아서 만들어 주는건지?
- 그레이들에 설정되어 있어서 build하면 생성한 Entity클래스로부터 Q클래스를 알아서 생성해줌

### saveAndFlush
- save는 해당 트렌젝션이 끝날때 실제 commit이 되는데 saveAndFlush는 바로 저장이 됨.

### Projections.fields()
- Projection 은 엔터티를 그냥 그대로 가지고 오는게 아니라 필요한 필드만 가지고 오는 걸 말한다. 
- Querydsl 에서는 프로젝션 대상이 하나면 명확한 타입을 지정할 수 있지만 프로젝션 대상이 둘 이상이라면 Tuple 이나 DTO 로 조회해야 한다.

```java
public void test() {
    //given
    QMember member1 = new QMember("M");
    //when
    List<jpaStudy.ex.dto.MemberDto> result = factory.select(Projections.fields(jpaStudy.ex.dto.MemberDto.class, QMember.member.name,
            ExpressionUtils.as(
                    JPAExpressions.select(member1.age.max()).from(member1), "age"
            )
            )).from(QMember.member).fetch();
    }		
```
  
### ExpressionUtils - 서브쿼리 작성 시 사용하는 클래스
- ExpressionUtils은 Querydsl 내부에서 새로운 Expression을 사용할 수 있도록 지원합니다.
- 예전에는 JPASubQuery를 통해 만들었지만, 버전업이 되어 현재는 JPAExpressions를 통해 서브쿼리를 생성


### Sub Query 서브쿼리
1. select Sub Query
    ```java
    @Override
    public List<StudentCount> findAllStudentCount() {
        return queryFactory
                .select(Projections.fields(StudentCount.class,
                        academy.name.as("academyName"),
                        ExpressionUtils.as(
                                JPAExpressions.select(count(student.id))
                                        .from(student)
                                        .where(student.academy.eq(academy)),
                                "studentCount")
                ))
                .from(academy)
                .fetch();
    }
    ```

2. where 절 Sub Query
    ```java
    @Override
    public List<Academy> findAllByStudentId(long studentId) {
        return queryFactory
                .selectFrom(academy)
                .where(academy.id.in(
                        JPAExpressions
                                .select(student.academy.id)
                                .from(student)
                                .where(student.id.eq(studentId))))
                .fetch();
    }
    ```

### Optional 클래스와 ifPresent 메소드의 기능, OptionalLong
- Optional 클래스는 java8부터 제공. 값이 존재할 수도 있고, 존재하지 않을 수도 있는 객체를 명시할 때 사용
  1. Optional 클래스는 제네릭으로 구현되어 있습니다. T는 포함된 값의 타입을 나타낸다.
  2. Optional은 final로 선언되어있는 불변객체이다. 따라서 한번 선언하면 변경이 불가능하다.
  3. 값이 없는 데이터는 empty() 값이 있는 데이터는 of() 메서드를 사용하여 객체를 생성한다.
  4. orElse(),orElseGet(),orElseThrow(),isPresent()같은 메서드로 null값을 안전하게 처리한다.

- Optional.isPresent()
    - Optional 객체가 값을 포함하고 있는지 여부를 반환한다.
    ```java
    Optional<String> isPresentExample= Optional.of("isPresent");
    if (isPresentExample.isPresent()) {
        System.out.println("Value is present.");
    }
    ```

- Optional.orElseThrow(Supplier<? extends X> exceptionSupplier)
    - Optional 객체가 값을 포함하고있으면 값을 반환하고,  
    그렇지 않으면 NoSuchElementException또는 직접 설정한 Exception을 반환 한다.
    ```java
    Optional<String> orElseThrowExample= Optional.empty();
    String value = orElseThrowExample.orElseThrow(() -> new CustomException("Value not present"));
    ```        

- OptinalInt, OptionalLong, OptionalDouble
    - IntStream 과 같이 Optional도 기본형을 값으로 하는 것들이 있다.
    - 반환타입이 Optional<T.>가 아닌것을 제외 하고는 Stream에 정의된 것과 비슷하다.
        > Optional<T> ->T get()  
         OptionalInt -> getAsInt()  
         OptionalLong -> getAsLong()  
         OptionalDouble -> getAsDouble()  



### 페이징 처리 스프링프레임워크 도메인의 Page, Pageable
- https://velog.io/@devmizz/Spring-Page-%EA%B0%9D%EC%B2%B4-%EC%9D%B4%EB%AA%A8%EC%A0%80%EB%AA%A8
