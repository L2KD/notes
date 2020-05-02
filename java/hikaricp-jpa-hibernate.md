## Deps

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>3.4.1</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-hikaricp</artifactId>
    <version>5.4.10.Final</version>
</dependency>
```

## Add bean

```xml
<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
    <property name="poolName" value="POOL_HIS_MNG" />
    <property name="connectionTestQuery" value="SELECT 1 from dual" />
    <property name="jdbcUrl" value="${db.url}"/>
    <property name="username" value="his_manager"/>
    <property name="password" value="${db.pass}"/>
    <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
</bean>

<bean id="dataSourceMNGH" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
    <constructor-arg ref="hikariConfig" />
</bean>
```

Then

```xml
<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
    <property name="dataSource" ref="dataSourceMNGH" />
    <property name="persistenceUnitName" value="myPersistenceUnit" />
    <property name="packagesToScan" >
        <list>
            <value>vn.vnpthis</value>
        </list>
    </property>
    <property name="jpaVendorAdapter" ref="jpaVendorAdapter"/>
</bean>
<bean id="jpaVendorAdapter" class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
    <property name="showSql" value="true"/>
    <property name="generateDdl" value="false"/>
    <property name="databasePlatform" value="org.hibernate.dialect.Oracle12cDialect"/>
</bean>
<jpa:repositories base-package="vn.vnpthis"
                    entity-manager-factory-ref="entityManagerFactory"
                    transaction-manager-ref="theProperTxManager"/>

<bean id="theProperTxManager" class="org.springframework.orm.jpa.JpaTransactionManager">
    <property name="dataSource" ref="dataSourceMNGH"/>
    <property name="entityManagerFactory" ref="entityManagerFactory" />
</bean>
```
