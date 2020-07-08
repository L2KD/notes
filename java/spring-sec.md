Secure một java app

Thêm đoạn sau vào web.xml

```
<filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<listener>
        <listener-class>org.springframework.security.web.session.HttpSessionEventPublisher</listener-class>
</listener>
```

Đoạn đó để Ủy thác cho spring sec để proxy các request.

## Spring sec

Thêm dep và thêm class

```
@Configuration
@EnableWebSecurity
public class OAuth2ResourceServer extends WebSecurityConfigurerAdapter {
  @Override
  protected void configure(HttpSecurity http) throws Exception {

    http
      .authorizeRequests(authorizeRequests ->
        authorizeRequests.anyRequest().authenticated()
      )
      .authorizeRequests().anyRequest().authenticated().and()
      .oauth2Login(Customizer.withDefaults())
      ;

  }

  @Bean
  public ClientRegistrationRepository clientRegistrationRepository() {
    List<ClientRegistration> registrations = clients.stream()
      .map(
        this::getRegistration
      )
      .filter(Objects::nonNull)
      .collect(Collectors.toList());
    return new InMemoryClientRegistrationRepository(this.keycloakClientRegistration());
  }

    private ClientRegistration keycloakClientRegistration() {
    return ClientRegistration
        .withRegistrationId("keycloak")
        .clientId("fhir-service")
        .clientSecret("3288b358-57fa-4673-9e18-6634f76528e5")
        .scope("read")
        .clientAuthenticationMethod(ClientAuthenticationMethod.BASIC)
        .clientName("fhir-service")
        .authorizationUri("http:10.82.33.6:9080/auth/realms/bsgd/protocol/openid-connect/auth")
        .redirectUriTemplate("{baseUrl}/hapi-fhir-jpaserver/fhir/")
        .redirectUriTemplate("{baseUrl}/")
        .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
        .tokenUri("http:10.82.33.6:9080/auth/realms/bsgd/protocol/openid-connect/token")
        .userInfoUri("http:10.82.33.6:9080/auth/realms/bsgd/protocol/openid-connect/userinfo")
        .userNameAttributeName("preferred_username")
        .build()
        ;
```

Annotation `@EnableWebSecurity` sẽ kích hoạt SPring sec. Class này phải được extend class `WebSecurityConfigurerAdapter`. Lưu ý, phiên bản Spring Sec 5 trở về sau có sự thay đổi này do Spring đã bỏ Authentication Server ra khỏi framework.

## Keycloak
