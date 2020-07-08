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
