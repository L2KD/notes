# Dùng gradle hoặc maven để build và run app

Gradle hay Maven là dep management và build tool cho các ngôn ngữ lập trình, trong đó có java.

Maven sử dụng `pom.xml` để cấu hình, còn Gradle thì dùng `build.gradle`

Cách cài đặt bin của 2 thằng này vui lòng tra cứu trên google.

Nếu ko muốn cài đặt có thể xài wrapper, thực ra là một package jar được đóng gói lại kèm theo 1 vài config và executable jar đó có thể thay thế vai trò của binary installed.

Dùng wrapper: `./gradlew` hoặc `./mvnw`

Khoogn dùng wrapper: `mvn` hoặc `gradle`

## Build

gradle -Pprod bootJar

Lệnh trên sẽ đóng gói app vào 1 fat jar (một file jar chứa tất cả deps cần thiết để có thể chạy ngay bằng lệnh `java`), sử dụng profile Prod (profile mặc định khi run). Nếu muốn nhiều profile thì ngăn cách bằng dấu `,`. Vd: `gradle -Pprod,swagger bootJar`.

    mvn package

Tương tự vậy, lệnh trên sẽ đóng gói lại thành jar hay war thì tùy vào file pom.xml định nghĩa.

mvn install

Lệnh trên sẽ package sau đó copy file đã được đóng gói vào thư mục `.m2` của user.

## Run

Sau khi có được file jar, nếu build bằng gradle thì sẽ nằm trong `build/libs`, còn mvn thì `target`.

Chạy bằng lệnh:

    java -jar build/libs/some-app.0.0.1-SNAPSHOT.jar

Lệnh tren sẽ chạy file jar một cách trực tiếp.

Nếu muốn truyền profile vào thì dùng:

    java -Dspring.profiles.active=prod,no-liquibase,swagger -jar some-app.jar

Có thể chạy trực tiếp executable jar

    ./some-app.jar

Lúc này nếu muốn profile phải truyền vào như args.

# war và jar

Jar được sử dụng như một executable standalone binary (chạy với `java` hoặc chạy 1 mình.

War cũng có thể dùng như executable binary, nhưng có thể lựa chọn khác là sử dụng bên trong 1 web container (vd `glassfish`).

Vì vậy khi deploy, có thể chạy `java -jar app.jar` bên trong đó có sẵn web service (nếu đang làm spring boot) (dùng war hoặc jar), hoặc deploy app bên trong tomcat/glassfish (dùng war)

# Spring Framework Security OAuth2

Để xử lý đăng nhập (chứng thực) người dùng.

Có nhiều cách để chứng thực:

- `@EnableOAuth2Sso`: Chứng thực nhờ vào 1 Authentication Server của bên khác (VD Facebook, Google).
- `auth-server`: Tự dùng Auth server, có thể tự cấp token.

Ví dụ hiện tại ta đang có 1 service, cần protect các endpoint trên service này. Khi này service (our app) được xem như là một Resource Server, tạo mọt configuration như sau:

    @Configuration

Ví dụ hiện tại ta đang có 1 service, cần protect các endpoint trên service này. Khi này service (our app) được xem như là một Resource Server, tạo mọt configuration như sau:

```Java
@Configuration
@EnableResourceServer
protected static class ResourceServerConfiguration extends ResourceServerConfigurerAdapter {
  @Override
  public void configure(HttpSecurity http) throws Exception {
    http
      .antMatcher("/me")
      .authorizeRequests().anyRequest().authenticated();
  }
}
```

@EnableAuthorizationServer để cấu hình app hiện tại thành một OAuth2 server (thay vì xài 1 bên khác như FB hay google).

@EnableResourceServer để cấu hình app hiện tại như một ResourceServer, bên trong đó có các resource endpoint, có thể được bảo vệ (auth required)

---

Phân quyền user bằng Spring Sec (ROLE & AUTHORITY)

Đầu tiên phải hiểu về Oauth2 (AuthenticationServer, ResourceServer)

Flow:

    1. Authentication Client phải request token (thường là /oauth/token_), sau đó dùng token đó, hoặc là nhét vào url hoặc là nhét vào header của request.
    2. Token này có expired time.
    3. Có 2 loại là token & reload token.
    4. Grant type là `password` hoặc `client_credentials`.
    5. Nếu grant type password thì lúc request token, truyền vào username/pwd & client id & secret. Nếu là client_credentials thì nhập vào client id và secret.

\* Để xác định client nào có ROLE gì, custom trong `configure()` của `AuthorizationServerConfigurerAdapter`

Ví dụ:

    clients.inMemory().withClient(x).secret(y).authorities("ROLE_ADMIN");

Trong đó x là client id, y là client secret, 2 cái này khai báo trong file `application.yml`.

Sau đó, auth client mới dùng id & secret này để xin token (từ `/oauth/token`). Thường thì nó nằm config trong `oauth2.web-client-configuration`.

\* Về phía service, để protect 1 resource endpoint. Custom lại hàm `configure()` của `ResourceServerConfigurerAdapter`.

Ví dụ:

    http.authorizeRequests().antMatchers("/api/**").hasAuthority(AuthoritiesConstants.ADMIN)

Như vậy tất cả endpoint `/api/**` đều phải có Authority là ADMIN mới có thể truy cập được.

---

## Hibernate + jpa and nativeQuery

VD

```Java
@Query(value = "select distinct k.sovaovien from KhamBenhNoiTru k" +
        " where trunc(k.ngayLap) BETWEEN trunc(:ngayVao) and trunc(:ngayRa) and k.dvtt = :dvtt")
List<Integer> findAllByDvtt(
        @Param("dvtt") String dvtt,
        @Param("ngayVao") Date ngayVao,
        @Param("ngayRa") Date ngayRa,
        Pageable pageable);
```

Danh sách trả về là 1 list Integer --> OK

VD2:

```Java
@Query(value = "select distinct k.SOVAOVIEN from HIS_MANAGER.NOITRU_DIEUTRI k" +
        " where TRUNC(NGAYLAP) BETWEEN TRUNC(:ngayVao) and TRUNC(:ngayRa) and k.DVTT = :dvtt", nativeQuery = true)
List<Integer> findAllByDvtt(
        @Param("dvtt") String dvtt,
        @Param("ngayVao") Date ngayVao,
        @Param("ngayRa") Date ngayRa,
        Pageable pageable);
```

Danh sách trả về là List\<BigDecimal\>

Có vẻ như HQL nó sẽ parse giá trị của ResultSet về lại đúng type khai báo trong entity có dính liếu, còn nativeQuery nó sẽ tự đánh giá chuyện đó thông qua "hình hài" của resultSet. Trong vd trên, SOVAOVIEN là Number(11).

---

## Spring data rest

Spring data rest hỗ trợ export các endpoint theo RESTful, từ các Entity của Hibernate, kết hợp tốt với JPA.

Để có thể sử dụng, add `dep` sau:

```Groovy
// Gradle
compile group: 'org.springframework.boot', name: 'spring-boot-starter-data-rest', version: '2.1.5.RELEASE'
```

Trong file `application.xml` (hoặc `application.properties`, tùy dự án), cấu hình base path:

```Yaml
spring:
  data:
    rest:
      base-path: /api
```

Như vậy, tất cả sẽ được export ra từ cái base endpoint là `/api`.

Tiếp theo, trong 1 repository, cần annotate:

```Java
@RepositoryRestResource(path = "structureCustom", collectionResourceRel = "structure")
public interface StructureRepo extends JpaRepository<Structure, Integer> { }
```

Theo đó, `@RepositoryRestResource` thực ra là optional, xuất hiện để có thể remap endpoint từ cái mặc định là `/structure` (automatically set theo JpaRepo) thành `/structureCustom`.

Ví dụ cơ bản CRUD: Theo quy ước RESTful thì các mỗi thao tác sẽ có động từ thích hợp. Ví dụ GET là lấy danh sách, hoặc nếu truyền id thì lấy chính xác theo id. POST là create. PUT là update, DELETE là ... uh, delete.

VD cách create new entity:

```http
POST http://localhost:10001/api/structure
Content-Type: application/json;charset=UTF-8

{ "desc": "Lorem ipsum", "level": 1 }
```

Sau request đó sẽ trả về `201` --> `Created`.

Kiểm tra lại bằng cách GET tới đó

```http
GET http://localhost:10001/api/structure
```

--> Ra 1 cái structure của spring data rest hỗ trợ cả phân trang và các entity có liên quan...

Trường hợp các entity có relationship với nhau thì sao??

```http
POST http://localhost:10001/api/unit
Content-Type: application/json;charset=UTF-8

{ "code": "K2", "desc": "Kệ số 3", "structureInfo": "http://localhost:10001/api/structure/1" }
```

Response:

```http
HTTP/1.1 201 Created
Expires: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
X-XSS-Protection: 1; mode=block
Pragma: no-cache
Location: http://localhost:10001/api/unit/5
Date: Fri, 07 Jun 2019 04:21:27 GMT
Connection: keep-alive
X-Content-Type-Options: nosniff
Content-Length: 0

<Response body is empty>

Response code: 201 (Created); Time: 61ms; Content length: 0 bytes
```

Cool, right?

### Dùng `projection` để ẩn, hoặc hiện field ra output của API

Mặc định, đối với Spring data rest thì những primitive valued fields mới hiện ra trong output của API. Các field có relationship thường sẽ hiện ra theo kiểu:

    manager: "http://localhost/api/employee/2"

thay vì

```json
manager: {
  "name": "John Doe",
  "dob": "",
  ...
}
```

Cách thực hiện:

1.  Đầu tiên viết 1 Interface kiểu:

    ```Java
    @Projection(
        name = "defaultEmployee",
        types = { Employee.class }
    )
    public interface DefaultEmployee {
      Integer getId();
      String getName();
      Employee getManager();
    }
    ```

    Trong đó có annotation `@Projection` với name là đặt lại cái tên cho dễ theo dõi (vd hiện tất cả fields, hoặc chỉ hiện password, hoặc trừ passwd ra, hoặc hiện cả vợ bé ra...), `types` thì ref về cái Class mà mình muốn xử.

2.  Kế tiếp, ở repo đang extends về JpaRepo, thêm annotation sau:

    ```java
    @RepositoryRestResource(
        ...
        excerptProjection = DefaultEmployee.class
    )
    ```

    Như vậy, 1 cách mặc định, repo này sẽ trả về cái hiển thị theo `interface` kia.

---

## Self-reference

Quan hệ tự thân, vd lớp Employee có 1 prop là `manager` cũng thuộc kiểu là `Employee`, cho biết người quản lý của anh B là sếp A. Ngoài ra cũng còn có 1 prop khác là `subordinates` thuộc kiểu `Set<Employee>`, cho biết các hạ cấp của sếp A là anh B, C, D...

Ví dụ

```java
@Entity
public class DonViLuuTru {
    @Id
    private Integer id;
    @Column(name = "id_cha")
    private Integer parentId;
    private Integer dvtt;
    private String kyHieu;
    private String moTa;

    @OneToMany
    @JoinColumn(name = "id_cha")
    private Set<DonViLuuTru> children = new HashSet<>();
}
```

Trong đó, parentId chính là id cha của instance hiện tại. Entity hiện tại có quan hệ `@OneToMany` với chính nó, join qua column là `id_cha`.

Để hiện 1 cách đệ quy children thì dùng `Projection` (xem phần trên)

```java
@Projection(...)
public interface ABC {
  ...;
  Set<ABC> getChildren();
}
```

Quan trọng là chỗ `Set<ABC>` để projection dựng lại đàn children (thông qua getter là `getChildren()`), sau đó ép kiểu về chính cái interface hiện tại.

---

## Recursive với self-reference entity

Nêu vấn đề: Bên trên đã có cấu hình các entity quan hệ với chính nó. Tuy vậy yêu cầu đặt ra là lấy được cái field custom như:

    # Pseudo code
    this.fullName = this.parent.name + this.name

    # Output mong muốn:
    Kệ 1 - Ngăn 2 - Chồng 1

    Trong đó Kệ 1 là this.parent.parent.name
    Ngăn 2 là this.parent.name
    Chồng 1 là this.name

    v.v...

Như vậy, theo suy nghĩ thông thường thì mình sẽ trả về

    this.name nếu như this.parentId null
    hoặc
    đệ quy this.name = this.parent.name + this.name

Vấn đề là phải móc được cái repo vào mới `getOne()` được parent trong DB.

Cách xử lý:

Vì để code thêm các xử lý này, ta phải tạo 1 cái class & interface khác để cái main repo implement luôn về cái class này

```java
public interface DonViLuuTruRepo extends JpaRepository<DonViLuuTru, Integer>, CustomDonViRep { ... }
```

Trong đó `CustomDonViRep` là cái đang được đề cập tới.

```java
public interface CustomDonViRep {
    String getFullName(DonViLuuTru donViLuuTru, DonViLuuTruRepo donViLuuTruRepo);
}

public class CustomDonViRepImpl implements CustomDonViRep {
    @Override
    public String getFullName(DonViLuuTru donViLuuTru, DonViLuuTruRepo donViLuuTruRepo) {
        String currentName = String.format("(%s) %s", donViLuuTru.getKyHieu(), donViLuuTru.getMoTa());

        if (donViLuuTru.getParentId() == null) {
            return currentName;
        } else {
            return String.format("%s - %s", this.getFullName(
                donViLuuTruRepo.getOne(donViLuuTru.getParentId()),
                donViLuuTruRepo
            ), currentName);
        }
    }
}
```

Projection:

```java
@Projection(
    name = "Projection don gian cho phan hien thi Luu tru hsba",
    types = DonViLuuTru.class
)
public interface BasicProjectionDonViLuuTru {
    Integer getId();
//    String getMoTa();
//    String getKyHieu();

    @Value("#{@donViLuuTruRepo.getFullName(target, @donViLuuTruRepo)}")
    String getFullDisplayName();
}
```

## Advanced search cho Spring data REST

Thường thì Spring data rest sẽ expose cái `/search` kèm theo 1 số method mình định lại bên trong cái repo extending JpaRepo (hoặc PagingAndSort...)

Ví dụ:

Tìm theo tên nhân viên:

    List<NhanVien> findByName(String name);

Tìm theo giới tính:

    List<NhanVien> findBySex(Integer sex);

Thế bây giờ muốn vừa tìm theo tên vừa tìm theo sex thì phải có thêm 1 method nữa sao? (`findByNameAndSex`). Cái này sẽ phát sinh theo một tỉ lệ rất khó đoán nếu như số lượng props của Entity nhiều, chưa kể mình phải check null và build cái query theo input vào của API nữa.

Để giải quyết, ta phải dùng `Example`.

Ví dụ:

    NhanVien nv = new NhanVien();
    nv.setName("NVA");
    nv.setSex(1);
    Example<NhanVien> e = Example.of(nv);

    repo.findAll(e);

Đoạn trên sẽ lấy tất cả nhân viên có tên NVA và có giới tính là 1.

Đoạn trên sẽ nằm trong controller, bên trong controller có autowire repo vào để chèn đoạn code trên vào trong 1 cái Mapping nào đó.

Nhưng vấn đề là ta cần output theo HAL để cho nó đúng RESTful.

Để output ra được HAL, ta phải annotate controller là `@RepositoryRestController`.

Có điều `@RepositoryRestController` lại không có EntityBody.

Ta phải inject `PagedResourcesAssembler` vào method để assemble resource:

```java
@GetMapping public Object getByCriteria(@RequestParam...,
  PagedResourcesAssembler pagedResourcesAssembler) {
    return ResponseEntity.ok(pagedResourcesAssembler.toResource(repo).findAll(e, PageRequest.of(p, s))));
}
```

Khi này, request vào endpoint:

    http://localhost/nhan-vien?name=NVA&sex=1

Result:

```
{
  _embedded: {...},
  _links: {
  self: {
    href: "http://localhost/nhan-vien?name=NVA&sex=1"
  }
  },
  page: {
    size: 10,
    totalElements: 1,
    totalPages: 1,
    number: 0
  }
}
```

**Vấn đề:** Khi này sẽ không tận dụng được `projection` để hiện thị thông tin như khi truy cập trực tiếp qua spring data rest. Do bên kia được annotation excerpt project cho repo, còn ở đây lấy instance của repo ra và gọi `findAll` thì thực ra nó trả về `Page` của type NhanVien (không có projection)

Cách xử lý: (https://stackoverflow.com/questions/29376090/how-to-apply-spring-data-projections-in-a-spring-mvc-controllers/29386907#29386907)

```java
return ResponseEntity.ok(pagedResourcesAssembler.toResource(
            luuTruHsbaRepo.findAll(e, PageRequest.of(p, s))
                .map(l -> projectionFactory.createProjection(DefaultProjectionLuuTruHsba.class, l))
            )
        );
```

Cấu hình theo SO chỉ

---

## Avoid stackoverflowerror for entity's ManyToOne relationships...

Thường thì là do `toString()` hoặc/và `hashCode()` của `@Data` (Lombok) tự động sinh ra.

Exclude bằng cách:

    // On the top of Entity
    @EqualsAndHashCode(exclude = {"parent", "children"})

    // Top of ManyToOne/OneToMany that we don't want Lombok to automatically generate the method.
    @ToString.Exclude

---

## Đọc file từ `resources` folder

Vd có file `text.txt` trong

    src/main/resources/text.txt

Để lấy file đó, bình thường sẽ dùng

```java
File theFile = new File("src/main/resources/text.txt");
```

Vấn đề là khi run trực tiếp trong IDE thì cái này chạy ngon lành, nhưng khi run bằng executable jar thì lại báo lỗi `File not found` do nó không tìm ra folder `src`.

Tuy vậy các resources vẫn được nhét vào jar bt.

**Giải pháp:**

```java
ClassPathResource cpr = new ClassPathResource("/text.txt");
InputStream inputStream = cpr.getInputStream();
File reportFile = File.createTempFile("text", "txt");
FileUtils.copyInputStreamToFile(inputStream, reportFile);
```

Ref: https://stackoverflow.com/a/31579156/3844992

---

## Zuul

`Zuul` là một Proxy, đóng vai trò là App gateway, một phần tất yếu trong kiến trúc microservice. Ngoài ra `zuul` cũng là một server-side load balancer.

### Include Zuul vào bằng cách

Lấy dep: group ID of `org.springframework.cloud`, artifact ID of `spring-cloud-starter-netflix-zuul`.

To enable it, annotate a Spring Boot main class with `@EnableZuulProxy`.

Theo quy ước, service có ID `users` sẽ nhận requests từ proxy có tại `/users`.

## Ribbon

Ribbon là client-size load balancer.

---

## Spring Boot Unit Test với Spring Security JWT tokens

Đối với một số API, việc cần phải có lớp bảo vệ cho các private endpoints là chuyện rất bình thường. VD:

```java
@Configuration
@EnableResourceServer
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfiguration extends ResourceServerConfigurerAdapter {
  @Override
  public void configure(HttpSecurity http) throws Exception {
      http
          .csrf()
          .disable()
          .headers()
          .frameOptions()
          .disable()
      .and()
          .sessionManagement()
          .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
      .and()
          .authorizeRequests()
          .antMatchers("/api/**").authenticated()
          ...;

  }
  @Bean
  public TokenStore tokenStore(JwtAccessTokenConverter jwtAccessTokenConverter) {
      return new JwtTokenStore(jwtAccessTokenConverter);
  }

  @Bean
  public JwtAccessTokenConverter jwtAccessTokenConverter(OAuth2SignatureVerifierClient signatureVerifierClient) {
      return new OAuth2JwtAccessTokenConverter(oAuth2Properties, signatureVerifierClient);
  }
```

Nhưng lúc này khi test được run, khúc trên sẽ check các request được gởi tới (thông qua mockmvc perform...) và kiểm tra xem có được authenticated không (authorization bằng jwt chẳng hạn). Để bypass chuyện này, cần phải khai báo một `@Configuration` khác, và nói với TestClass rằng lấy cái config kia mà xài.

```java
@Configuration
public class SecurityBeanOverrideConfiguration {
  @Bean
  @Primary
  public TokenStore tokenStore() {
      return null;
  }

  @Bean
  @Primary
  public JwtAccessTokenConverter jwtAccessTokenConverter() {
      return null;
  }
}
```
