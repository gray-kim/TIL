# Transactional Annotation

> api서버에서는 api송신 성공, 실패 여부 상관없이 로그에 잘 남는데 web프로젝트에서는 실패 시 로그가 남지 않는 부분 임규태D에게 문의하였더니 api호출하는 상위 메소드에 트랜젝션이 걸려있는 경우 롤백이 되어서 로그에 남기는 부분까지 롤백되기 때문에 새로운 트랜젝션을 생성해주면 실패 시에도 로그 남길 수 있다고 알려주심

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
```


