@Embeddable
=============

> 테이블에서는 addres1 , addres2, zipcode와 같이 세부적으로 나눠저있는 경우
> 엔티티 클래스에서 해당 연관 객체를 생성하여 사용 할 수 있다 이때 해당 어노테이션(@Embeddable)를 사용한다.
> >```java
> >
> >@Embeddable
> >public class Address {
> >    @Column(name = "addr1")
> >    private String address1;
> >    @Column(name = "addr2")
> >    private String address2;
> >    @Column(name = "zipcode")
> >    private String zipcode;
> >
> >    protected Address() {
> >    }
> >
> >    public Address(String address1, String address2, String zipcode) {
> >        this.address1 = address1;
> >        this.address2 = address2;
> >        this.zipcode = zipcode;
> >    }
> >    ...
> >}
> >```


> > 해당 아래 구문을 확인</br>
> > <span style="color:red">
__private Address address;__
    </span>
> >```java
> >@Entity
> >@Table(name = "hotel_info")
> >public class Hotel {
> >    @Id
> >    @Column(name = "hotel_id")
> >    private String id;
> >
> >    @Column(name = "nm")
> >    private String name;
> >
> >    private int year;
> >
> >    @Enumerated(EnumType.STRING)
> >    private Grade grade;
> >
> >    @Embedded
> >    private Address address;
> >
> >    private LocalDateTime created;
> >
> >    @Column(name = "modified")
> >    private LocalDateTime lastModified;
> >
> >    protected Hotel() {
> >    }
> >
> >    public Hotel(String id, String name, int year, Grade grade, Address address) {
> >        this.id = id;
> >        this.name = name;
> >        this.year = year;
> >        this.grade = grade;
> >        this.address = address;
> >        this.created = LocalDateTime.now();
> >        this.lastModified = LocalDateTime.now();
> >    }
> >
> >}
> >```
