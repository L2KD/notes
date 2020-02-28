# Learning log

## FHIR là gì và tại sao nên dùng FHIR.

(Dịch từ https://fhir-france.blogspot.com/)

0. FHIR (Fast Health Interoperability Resources) là chuẩn liên thông hệ thống mới nhất do HL7 định ra.

1. Được thiết kế để cho phép trao đổi thông tin để hỗ trợ việc cung cấp dịch vụ chăm sóc sức khỏe trong nhiều môi trường khác nhau. Đặc tả được xây dựng và điều chỉnh các thực hành RESTful hiện đại, được sử dụng rộng rãi để cho phép cung cấp dịch vụ chăm sóc sức khỏe tích hợp trên một loạt các nhóm và tổ chức.

1. Phạm vi dự định của FHIR là rộng, bao gồm con người và thú y, chăm sóc lâm sàng, y tế công cộng, thử nghiệm lâm sàng, quản trị và các khía cạnh tài chính

1. Các khái niệm

   - Một số các đặc tính (properties) mà 80% các hệ thống hiện đang sử dụng. Mỗi property có một kiểu dữ liệu (data type) riêng biệt (mặc dù một vài resource cho phép có thể có nhiều hơn 1 data type cho property)
   - Cơ chế chuẩn mở rộng (extension standard) cho phép các dev có thể thêm properties mới một cách chắc chắn an toàn.
   - Một định danh (identity) để nhờ đó có thể lưu trữ, tìm hoặc xem. (xem thêm sửa xóa - crud)
   - Mỗi resource có một validation schema.
   - Interoperability: Sự tương kết, liên thông giữa các hệ thống
   - Resource: Đơn vị cơ bản của sự liên thông, thứ nhỏ nhất hay được nhắc đến. Vd: một bệnh nhân, một chẩn đoán hay y bác sĩ.
   - Mỗi resource có một vài đặc điểm chung:
   - Các resource có thể được tái sử dụng trong hệ biến hóa liên thông (paradigm). Tức là chúng ta có thể nhận (hoặc lưu) một resource thông qua RESTful service, sau đó tích hợp vào một tin nhắn (message) hoặc đính vào một tài liệu.
   - FHIR được xây dựng trong một hệ thống có phiên bản (versioning). Mỗi resource có thể có nhiều phiên bản, và có các cơ chế để lấy lịch sử thay đổi của một resource nhất định.
   - FHIR có thể được thể hiện dưới dạng JSON hoặc XML.

1. Mỗi resource có các ví dụ chỉ ra cách sử dụng resource như thế nào.
1. FHIR dưới dạng JSON rất lý tưởng để dùng cho việc dev mobile.
1. Hiện đã có nhiều clients open-source để biến chuẩn FHIR này trở nên dễ sử dụng và dễ test.
1. Cũng tồn tại nhiều server để test.

## [Conformance (tức là sự phù hợp)](http://hl7.org/fhir/conformance-module.html)

Do FHIR là một quy chuẩn (standard), nên các phần mềm khác implement nó phải khai báo một số meta data, nhằm thể hiện các ý như:

- Các quy tắc về resouce nào được sử dụng, resources nào không, các resource nào được thêm vào mà không phải là có sẵn (base definition).
- Quy tắc về RESTful API, gởi tin...
- Quy tắc về các terminologies được sử dụng.
- Mô tả Resouce map (map từ resource fhir vào model nào của phần mềm).

Conformance resources gồm có 2 resources thường được sử dụng để khai báo đầy đủ các thứ trên: ImplementationGuide và CapabilityStatement.

Đối với HAPI thì

- CapabilityStatement nằm trong `/hapi-fhir-validation-resources-r4/src/main/resources/org/hl7/fhir/r4/model/profile/profiles-resources.xml` (có 2 cái)
- StructureDefinition nằm trong file trên và `/hapi-fhir-validation-resources-r4/src/main/resources/org/hl7/fhir/r4/model/profile/profiles-others.xml` và `/hapi-fhir-validation-resources-r4/src/main/resources/org/hl7/fhir/r4/model/profile/profiles-types.xml`.

Bằng chứng là lúc run time của HAPI (jpa server), sẽ có các profile validator load các file trên.

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

## Security

AuthN

AuthZ
