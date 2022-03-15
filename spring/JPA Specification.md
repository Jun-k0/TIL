# JPA Specification란?
JPA Specification은 DB 쿼리의 조건을 간단히 Spec으로 작성하여 날릴 수 있게 해줍니다.

하나의 검색을 위한 @GetMapping이 있다고 할때, 시간이 흐를수록 점점 조건이 추가될수록 코드가 덧붙여지며 유지보수가 힘들어지게 됩니다.

이때, Spec으로 관리하게 되면 코드가 깔끔해지며 유지보수가 좀 더 용이해집니다.

# Specification 적용

Specification을 적용하기 위해서 우선 ```JpaSpecificationExecutor<T>```를 추가로 상속받아야 합니다.
```java
public interface StockRepository extends JpaRepository<Stock, Long>, JpaSpecificationExecutor<Stock> {
}
```

원하는 쿼리를 Spec에 작성해봅시다.

```java
public class StockSpecification {
	
    // like 쿼리 (포함)
    public static Specification<Stock> likeName(String name){
        return new Specification<Stock>() {
            @Override
            public Predicate toPredicate(Root<Stock> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder) {
                return criteriaBuilder.like(root.get("name"), "%" + name + "%");
            }
        };
    }
	
    // between 쿼리 (사이값)
    public static Specification<Stock> betweenDate(LocalDate startDate, LocalDate endDate){
        return new Specification<Stock>() {
            @Override
            public Predicate toPredicate(Root<Stock> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder) {
                return criteriaBuilder.between(root.get("date"), startDate, endDate);
            }
        };
    }
    
    // equal 쿼리 (일치)
    public static Specification<Stock> equalName(Long todoId) {
        return new Specification<Stock>() {
            @Override
            public Predicate toPredicate(Root<Stock> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder) {
                return criteriaBuilder.equal(root.get("name"), name);
            }
        };
    }
}
```
작성한 Spec을 사용해봅시다.
```java
@RequiredArgsConstructor
@Service
@Transactional
public class StockService {
    private final StockRepository stockRepository;

    public List<Stock> searchStockList(String name, LocalDate startDate, LocalDate endDate){
        Specification<Stock> spec = Specification.where(StockSpecification.likeName(name));
        spec = spec.and(StockSpecification.betweenDay(startDate, endDate));

        return stockRepository.findAll(spec);
    }
}
```
```java
@RequiredArgsConstructor
@RestController
public class StockApiController {

    private final StockService Stock;

    @GetMapping("/api/v1/stock")
    public List<Stock> search(@RequestParam String name,
                              @RequestParam String startDate,
						      @RequestParam String endDate){
        LocalDate start = LocalDate.parse(startDate, DateTimeFormatter.ISO_DATE);
        LocalDate end = LocalDate.parse(endDate, DateTimeFormatter.ISO_DATE);
        return Stock.searchStockList(name, start, end);
    }
}
```

# vs QueryDSL
갓영한님 말씀에 의하면 실무에서 JpaSpecificationExcutor를 사용하는 것은 권장하지 않는다고 한다.. 

JpaSpecification 자체가 결국 JPA가 제공하는 Criteria로 이루어지는데, **JPA의 Criteria는 조금만 복잡해져도 실무에서 사용이 정말 어려워 지기 때문.**

JPA Criteria(JpaSpecificationExecutor 포함)에 비해 편리하고 실용적인 QueryDSL을 쓰자.

# Reference
https://velog.io/@hellozin/JPA-Specification%EC%9C%BC%EB%A1%9C-%EC%BF%BC%EB%A6%AC-%EC%A1%B0%EA%B1%B4-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0
https://www.inflearn.com/questions/16685
