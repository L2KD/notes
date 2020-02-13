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

```java
@RequestMapping(value = "/add-patient")
public ResponseEntity testFhir() {
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("dd/MM/yyyy");
    Patient pat1 = new Patient();
    Bundle resp = new Bundle();
    Bundle bundle = new Bundle();

    try {
        pat1.addIdentifier()
                .setSystem("http://vnpthis.vn/mabn")
                .setValue("12345");
        pat1.setId(IdType.newRandomUuid());
        pat1
                .setGender(Enumerations.AdministrativeGender.MALE)
                .setBirthDate(simpleDateFormat.parse("10/10/2010"))
                .addName().setFamily("Đoàn").addGiven("Đoàn Văn Hậu");

        bundle.setType(Bundle.BundleType.TRANSACTION);

        bundle.addEntry()
                .setFullUrl(pat1.getIdElement().getValue())
                .setResource(pat1)
                .getRequest()
                .setUrl("Patient")
                .setIfNoneExist("identifier=http://vnpthis.vn/mabn|12345")
                .setMethod(Bundle.HTTPVerb.POST);
        IGenericClient client = hl7FhirUtils.getClient();
        MethodOutcome outcome = client.create()
                .resource(pat1)
                .prettyPrint()
                .encodedJson()
                .execute();
        IIdType id = outcome.getId();
        System.out.println("Got ID: " + id.getValue());
        return ResponseEntity.ok(id.getValue());

    } catch (ParseException e) {
        e.printStackTrace();
    }
    return ResponseEntity.ok("");
}
```

### Search resource

```java
@RequestMapping(value = "/fhir-list-patients")
public ResponseEntity listPatients() {
    IGenericClient client = hl7FhirUtils.getClient();
    // Perform a search
    Bundle results = client
            .search()
            .forResource(Patient.class)
            .where(Patient.FAMILY.matches().value("Đoàn"))
            .returnBundle(Bundle.class)
            .execute();

    return ResponseEntity.ok(hl7FhirUtils.getContext().newJsonParser().setPrettyPrint(true).encodeResourceToString(results));
}
```
