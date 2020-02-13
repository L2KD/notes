# Learning log

## Dependencies

Để sử dụng được, cần add tối thiểu các deps sau

- `hapi-fhir-base`
- `hapi-fhir-structures-{version}`

Trong đó version có thể là DSTU2, hay R4... tùy vào lựa chọn và nhu cầu.

Nếu sử dụng client trong app, thêm dep:

- `hapi-fhir-client`

Cụ thể

```
<properties>
    <hapi.version>4.2.0-SNAPSHOT</hapi.version>
</properties>

<dependency>
    <groupId>ca.uhn.hapi.fhir</groupId>
    <artifactId>hapi-fhir-base</artifactId>
    <version>${hapi.version}</version>
</dependency>
<dependency>
    <groupId>ca.uhn.hapi.fhir</groupId>
    <artifactId>hapi-fhir-structures-r4</artifactId>
    <version>${hapi.version}</version>
</dependency>
<dependency>
    <groupId>ca.uhn.hapi.fhir</groupId>
    <artifactId>hapi-fhir-client</artifactId>
    <version>${hapi.version}</version>
</dependency>
<repositories>
    <repository>
        <id>oss-snapshot</id>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </repository>
</repositories>
```

## Client

Client có 2 loại, 1 là generic, 2 là annotation.

### Cách tạo generic client

```java
FhirContext context = FhirContext.forR4();
IGenericClient genericClient = context.newRestfulGenericClient(serverUrl);
```

### Cách sử dụng

Tạo một patient
