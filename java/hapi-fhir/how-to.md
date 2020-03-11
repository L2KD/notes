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

## Module của FHIR

- (1) Foundation: Chịu trách nhiệm mọi thứ về cơ sở hạ tầng tổng thể cho FHIR specification.
- (2) Conformance
- (2) Exchange
- (2) Terminology
- (2) Security & Privacy
- (2) Implementation Support
- (3) Administration
- (4) Clinical
- (4) Diagnostics
- (4) Medications
- (4) Workflow
- (4) Financial
- (5) Clinical Reasoning

## [Conformance module (tức là sự phù hợp)](http://hl7.org/fhir/conformance-module.html)

Những thành phần cơ bản của một phần mềm dựa từ FHIR.

Đối với HAPI thì những thứ này có thể xem khi request tới `/{baseEndpoints}/metadata`

### Conformance (CapabilityStatement & StructureDefinition)

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

Ngoài ra validator còn load StructureDefinition trong `/hapi-fhir-validation-resources-r4/src/main/resources/org/hl7/fhir/r4/model/extension/extension-definitions.xml`.

### [Compartment definitions](http://hl7.org/fhir/2016Sep/compartmentdefinition.html)

Compartment là một logical grouping các resources có liên quan với nhau.

Khai báo để đạt được các mục đích sau:

- Hoạt động như một cơ chế để có thể tìm kiếm các resources có liên quan một cách nhanh nhất.
- Cung cấp khai báo cơ bản để áp dụng access control trên các resources.

### [SearchParam definitions](http://hl7.org/fhir/2016Sep/searchparameter.html)

Khai báo thêm các search param (nằm ngoài chuẩn FHIR cơ bản) mà phần mềm có dùng đến.

### ValueSet

ValueSet định ra một set những code dùng code systems, sử dụng trong các ngữ cảnh đặc biệt. Value sets link between CodeSystem definitions and their use in coded elements (terminologies).

HAPI có embed ValueSet của SNOMED CT, và LOINC. (file `/hapi-fhir-validation-resources-r4/src/main/resources/org/hl7/fhir/r4/model/valueset/valuesets.xml`)

### Operation definitions

## Chức năng của HAPI

### DB improvements

Clusterd

```
2020-03-03 14:15:47.443  INFO 2854 --- [           main] org.hibernate.Version                    : HHH000412: Hibernate Core {5.3.7.Final}
2020-03-03 14:15:47.444  INFO 2854 --- [           main] org.hibernate.cfg.Environment            : HHH000206: hibernate.properties not found
2020-03-03 14:15:47.552  INFO 2854 --- [           main] o.hibernate.annotations.common.Version   : HCANN000001: Hibernate Commons Annotations {5.0.4.Final}
2020-03-03 14:15:47.737  INFO 2854 --- [           main] org.hibernate.dialect.Dialect            : HHH000400: Using dialect: org.hibernate.dialect.MySQL5InnoDBDialect
2020-03-03 14:15:48.262  INFO 2854 --- [           main] org.hibernate.search.engine.Version      : HSEARCH000034: Hibernate Search 5.11.3.Final
2020-03-03 14:15:48.989  INFO 2854 --- [           main] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2020-03-03 14:15:49.223  INFO 2854 --- [           main] o.h.h.i.QueryTranslatorFactoryInitiator  : HHH000397: Using ASTQueryTranslatorFactory
2020-03-03 14:15:49.482  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Creating Local Scheduler
2020-03-03 14:15:49.499  INFO 2854 --- [           main] org.quartz.impl.StdSchedulerFactory      : Using default implementation for ThreadExecutor
2020-03-03 14:15:49.508  INFO 2854 --- [           main] org.quartz.core.SchedulerSignalerImpl    : Initialized Scheduler Signaller of type: class org.quartz.core.SchedulerSignalerImpl
2020-03-03 14:15:49.508  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : Quartz Scheduler v.2.3.0 created.
2020-03-03 14:15:49.509  INFO 2854 --- [           main] org.quartz.simpl.RAMJobStore             : RAMJobStore initialized.
2020-03-03 14:15:49.510  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : Scheduler meta-data: Quartz Scheduler (v2.3.0) 'local-0' with instanceId 'NON_CLUSTERED'
  Scheduler class: 'org.quartz.core.QuartzScheduler' - running locally.
  NOT STARTED.
  Currently in standby mode.
  Number of jobs executed: 0
  Using thread pool 'org.quartz.simpl.SimpleThreadPool' - with 4 threads.
  Using job-store 'org.quartz.simpl.RAMJobStore' - which does not support persistence. and is not clustered.

2020-03-03 14:15:49.510  INFO 2854 --- [           main] org.quartz.impl.StdSchedulerFactory      : Quartz scheduler 'local-0' initialized from an externally provided properties instance.
2020-03-03 14:15:49.510  INFO 2854 --- [           main] org.quartz.impl.StdSchedulerFactory      : Quartz scheduler version: 2.3.0
2020-03-03 14:15:49.510  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : JobFactory set to: ca.uhn.fhir.jpa.sched.AutowiringSpringBeanJobFactory@4e74adb
2020-03-03 14:15:49.510  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : Scheduler local-0_$_NON_CLUSTERED paused.
2020-03-03 14:15:49.510  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Creating Clustered Scheduler
2020-03-03 14:15:49.511  INFO 2854 --- [           main] org.quartz.impl.StdSchedulerFactory      : Using default implementation for ThreadExecutor
2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.core.SchedulerSignalerImpl    : Initialized Scheduler Signaller of type: class org.quartz.core.SchedulerSignalerImpl
2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : Quartz Scheduler v.2.3.0 created.
2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.simpl.RAMJobStore             : RAMJobStore initialized.
2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : Scheduler meta-data: Quartz Scheduler (v2.3.0) 'clustered-1' with instanceId 'NON_CLUSTERED'
  Scheduler class: 'org.quartz.core.QuartzScheduler' - running locally.
  NOT STARTED.
  Currently in standby mode.
  Number of jobs executed: 0
  Using thread pool 'org.quartz.simpl.SimpleThreadPool' - with 4 threads.
  Using job-store 'org.quartz.simpl.RAMJobStore' - which does not support persistence. and is not clustered.

2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.impl.StdSchedulerFactory      : Quartz scheduler 'clustered-1' initialized from an externally provided properties instance.
2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.impl.StdSchedulerFactory      : Quartz scheduler version: 2.3.0
2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : JobFactory set to: ca.uhn.fhir.jpa.sched.AutowiringSpringBeanJobFactory@4e74adb
2020-03-03 14:15:49.512  INFO 2854 --- [           main] org.quartz.core.QuartzScheduler          : Scheduler clustered-1_$_NON_CLUSTERED paused.
2020-03-03 14:15:49.516  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Scheduling local job ca.uhn.fhir.jpa.searchparam.registry.SearchParamRegistryImpl with interval 00:00:10.000
2020-03-03 14:15:50.720  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Scheduling local job ca.uhn.fhir.jpa.term.TermDeferredStorageSvcImpl$Job with interval 5000ms
2020-03-03 14:15:51.113  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Scheduling clustered job ca.uhn.fhir.jpa.search.reindex.ResourceReindexingSvcImpl with interval 00:00:10.000
2020-03-03 14:15:58.233  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Scheduling local job ca.uhn.fhir.jpa.util.ResourceCountCache with interval 00:10:00
2020-03-03 14:16:00.007  INFO 2854 --- [           main] .f.r.h.c.DefaultProfileValidationSupport : Loading structure definitions from classpath: /org/hl7/fhir/r4/model/profile/profiles-resources.xml
2020-03-03 14:16:00.022  INFO 2854 --- [           main] ca.uhn.fhir.util.XmlUtil                 : FHIR XML procesing will use StAX implementation 'Woodstox XML-processor' version '4.4.1'
2020-03-03 14:16:00.463  INFO 2854 --- [           main] .f.r.h.c.DefaultProfileValidationSupport : Loading structure definitions from classpath: /org/hl7/fhir/r4/model/profile/profiles-types.xml
2020-03-03 14:16:00.496  INFO 2854 --- [           main] .f.r.h.c.DefaultProfileValidationSupport : Loading structure definitions from classpath: /org/hl7/fhir/r4/model/profile/profiles-others.xml
2020-03-03 14:16:00.638  INFO 2854 --- [           main] .f.r.h.c.DefaultProfileValidationSupport : Loading structure definitions from classpath: /org/hl7/fhir/r4/model/extension/extension-definitions.xml
2020-03-03 14:16:00.838  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Scheduling clustered job ca.uhn.fhir.jpa.term.TermReadSvcR4 with interval 00:10:00
2020-03-03 14:16:01.614  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Scheduling local job ca.uhn.fhir.jpa.subscription.SubscriptionTriggeringSvcImpl with interval 5000ms
2020-03-03 14:16:01.704  WARN 2854 --- [           main] c.n.c.sources.URLConfigurationSource     : No URLs will be polled as dynamic configuration sources.
2020-03-03 14:16:01.704  INFO 2854 --- [           main] c.n.c.sources.URLConfigurationSource     : To enable URLs as dynamic configuration sources, define System property archaius.configurationSource.additionalUrls or make config.properties available on classpath.
2020-03-03 14:16:01.708  INFO 2854 --- [           main] c.netflix.config.DynamicPropertyFactory  : DynamicPropertyFactory is initialized with configuration sources: com.netflix.config.ConcurrentCompositeConfiguration@24be9868
2020-03-03 14:16:02.155  INFO 2854 --- [           main] c.u.f.j.sched.BaseSchedulerServiceImpl   : Scheduling clustered job ca.uhn.fhir.jpa.search.warm.CacheWarmingSvcImpl with interval 00:00:10.000
2020-03-03 14:16:02.166  INFO 2854 --- [           main] o.s.s.c.ScheduledExecutorFactoryBean     : Initializing ExecutorService
2020-03-03 14:16:02.166  INFO 2854 --- [           main] o.s.s.c.ScheduledExecutorFactoryBean     : Initializing ExecutorService 'scheduledExecutorService'
2020-03-03 14:16:02.242  INFO 2854 --- [           main] .s.s.UserDetailsServiceAutoConfiguration :

Using generated security password: 865ec77d-c58e-4066-8378-862238df9d6e

```

### Scheduler

Tự động chạy một số nhiệm vụ. Được viết dựa trên thư viện quartz scheduler org.

Trong HAPI có 2 cái scheduler là `Local` và `Clustered`. Tức 2 classes trên extend lại abstract class `BaseHapiScheduler` (`LocalHapiScheduler` & `ClusteredHapiScheduler`)

Sau đó sẽ có service `scheduler` để quản lý 2 ông scheduler trên (`ISchedulerService`).

> Local scheduler sẽ thực thi chỉ 1 task xuyên suốt cluster (khi chạy trong môi trường cluster)
> Còn Clustered sẽ thực thi locally (và chạy trên tất cả các node của cluster nếu đang chạy chế độ cluster)

Sau đó các task nào cần scheduler sẽ Autowire `SchedulerService` vào.

VD

```java
public class TermReindexingSvcImpl implements ITermReindexingSvc {
	@Autowired
	private ISchedulerService mySchedulerService;
}
```

Tiếp theo Task này được khai báo `Bean` ở phần `BaseConfig`.

**Các module sử dụng Scheduler**

- TermReindexing
- SubscriptionLoader <- pub/sub?
- BulkDataExport
- BaseTermRead (abstract)
- StaleSearchDeleting
- CacheWarming
- TermDeferredStorage
- ResourceReindexing
- SubscriptionTriggering
- AnalyticsInterceptor

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

## fhir-downloader

Bulk data access

View more at https://github.com/smart-on-fhir/sample-apps-stu3/tree/master/fhir-downloader, https://chat.fhir.org/#narrow/stream/179250-bulk-data/,

Dùng docker

```
$ docker pull node:7.9-alpine
$ git clone https://github.com/smart-on-fhir/sample-apps-stu3.git
$ docker run -it --rm -v /where-you-put-it/sample-apps-stu3:/tmp/fhir node:alpine /bin/sh

# inside the container

# cd /tmp/fhir/fhir-downloader
# copy config.example.json config.json
Adapt config by our server and token info if needed
# npm i
# node . -d /dev/null

```

Well, failed... bởi vì cái \$import nó ở STU3. Server hiện tại ở R4.
