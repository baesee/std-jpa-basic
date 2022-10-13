JPA, JPQL ( JPA Query Language )
=============

> JPQL 특징
- 테이블 대신 엔티티 이름, 속성을 사용한다.
> > Entity
```java
@Entity
public class Review {
    @Id
    @Column(name = "review_id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    @Column(name = "hotel_id")
    private String hotelId;
    private int mark;
    @Column(name = "writer_name")
    private String writerName;
    private String comment;
    private LocalDateTime created;
```
```java
TypedQuery<Review> query = em.createQuery(
                    "select r from Review r where r.hotelId = :hotelId order by r.id desc",
                    Review.class);
            query.setParameter("hotelId", "H-001");
            // 페이징 처리 
            query.setFirstResult(8); // 0부터 시작, 시작행
            query.setMaxResults(4); // 최대 결과 개수
            List<Review> reviews = query.getResultList();
```
- 기본구조
  - SELECT 별칭 FROM 엔티티명 별칭
    - select r from Review r
    - select r from Review as r
  - 쿼리 생성
    - TypedQuery<Review> query = em.createQuery("select r from Review r where r.hotelId = :hotelId order by r.id desc", Review.class); <br/>List<Review> reviews = query.getResultList(); // getResultList 시점에 쿼리 실행
  - 검색 조건
    - WHERE, AND, OR , 괄호 등 기본 SQL문법과 거의 동일
      - select r from Review r where r.hotelId = :hotelId
      - select r from Review r where r.hotelId = ?
      - select r from Review r where r.hotelId = :hotelId and r.mark > : minMark
    - 파라미터
      - 이름 사용 : querySetParameter("hotelId","H-001");
      - 인덱스 기반 : querySetParameter(0,"H-001");
  ```java
  query.setParameter("hotelId", "H-001");
  ```
    - 정렬 ( order by asc, desc)
  
- 주의사항 (다음 경우는 JPQL말고 일반쿼리 사용 고려)
   - 여러 테이블 조인
   - DBMS 특화된 쿼리 필요시 
   - 서브 쿼리 필요시
   - 통계 및 대량 데이터 조회/처리 시
    
