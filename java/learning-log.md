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

@EnableAuthorizationServer để cấu hình app hiện tại thành một OAuth2 server (thay vì xài 1 bên khác như FB hay google).

@EnableResourceServer để cấu hình app hiện tại như một ResourceServer, bên trong đó có các resource endpoint, có thể được bảo vệ (auth required)
