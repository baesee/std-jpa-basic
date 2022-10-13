컬렉션(Set,List,Map) 을 이용한 1:1, 1:N(@Embeddable) 매핑
=============

> 컬렉션 테이블을 이용한 매핑의 경우 아래 어노테이션 중 이용하여 가능
> @ElementCollection, @CollectionTable, @MapKeyColumn

> > Set 이용 (1:1)
```java
@Entity
@Table(name = "role")
public class Role {
    @Id
    private String id;
    private String name;

    @ElementCollection
    @CollectionTable(
            name = "role_perm",
            joinColumns = @JoinColumn(name = "role_id")
    )
    @Column(name = "perm")
    private Set<String> permissions = new HashSet<>();
...
```

> > Set 이용 (1:N)
```java

@Embeddable
public class GrantedPermission {
    @Column(name = "perm")
    private String permission;
    private String grantor;

    protected GrantedPermission() {}

    public GrantedPermission(String permission, String grantor) {
        this.permission = permission;
        this.grantor = grantor;
    }
    ...
    
@Entity
@Table(name = "role")
public class Role2 {
    @Id
    private String id;
    private String name;

    @ElementCollection
    @CollectionTable(
            name = "role_perm",
            joinColumns = @JoinColumn(name = "role_id")
    )
    private Set<GrantedPermission> permissions = new HashSet<>();
    ...
```

> > List 이용 (1:1)
```java
@Entity
@Table(name = "question")
public class Question {
    @Id
    private String id;
    private String text;

    @ElementCollection(fetch = FetchType.EAGER)
    @CollectionTable(
            name = "question_choice",
            joinColumns = @JoinColumn(name = "question_id")
    )
    @OrderColumn(name = "idx")
    @Column(name = "text")
    private List<String> choices = new ArrayList<>();

    protected Question() {}
    ...
```

> > List 이용 (1:N)
```java
@Embeddable
@Access(AccessType.FIELD)
public class Choice {
    private String text;
    private boolean input;

    protected Choice() {}

    public Choice(String text, boolean input) {
        this.text = text;
        this.input = input;
    }
    ...
    
@Entity
@Table(name = "question")
public class Question2 {
    @Id
    private String id;
    private String text;

    @ElementCollection(fetch = FetchType.EAGER)
    @CollectionTable(
            name = "question_choice",
            joinColumns = @JoinColumn(name = "question_id")
    )
    @OrderColumn(name = "idx")
    private List<Choice> choices = new ArrayList<>();

    protected Question2() {}
  ...
    
```

> > Map 이용 (1:1)
```java
@Entity
@Table(name = "doc")
public class Document {
    @Id
    private String id;
    private String title;
    private String content;
    @ElementCollection
    @CollectionTable(
            name = "doc_prop",
            joinColumns = @JoinColumn(name = "doc_id")
    )
    @MapKeyColumn(name = "name")
    @Column(name = "value")
    private Map<String, String> props = new HashMap<>();
...
```

> > Map 이용 (1:N)
```java

@Embeddable
@Access(AccessType.FIELD)
public class PropValue {
    private String value;
    private boolean enabled;

    protected PropValue() {}

    public PropValue(String value, boolean enabled) {
        this.value = value;
        this.enabled = enabled;
    }
    ...

@Entity
@Table(name = "doc")
public class Document2 {
    @Id
    private String id;
    private String title;
    private String content;
    @ElementCollection
    @CollectionTable(
            name = "doc_prop",
            joinColumns = @JoinColumn(name = "doc_id")
    )
    @MapKeyColumn(name = "name")
    private Map<String, PropValue> props = new HashMap<>();
    ...
    
``` 
