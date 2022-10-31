## JPA with Hibernate
- **Entity State Transaction**: https://vladmihalcea.com/a-beginners-guide-to-jpa-hibernate-entity-state-transitions/
- **JPA persist, merge and Hibernate save, update, saveOrUpdate methods**: https://vladmihalcea.com/jpa-persist-merge-hibernate-save-update-saveorupdate/
- **One to One Relation**: https://vladmihalcea.com/the-best-way-to-map-a-onetoone-relationship-with-jpa-and-hibernate/
- **One to Many Relation**: https://vladmihalcea.com/the-best-way-to-map-a-onetomany-association-with-jpa-and-hibernate/
- **Many to Many Relation**: 
    - https://vladmihalcea.com/the-best-way-to-use-the-manytomany-annotation-with-jpa-and-hibernate/ 
    - https://www.baeldung.com/jpa-many-to-many
- **JPA Cascade**: https://vladmihalcea.com/a-beginners-guide-to-jpa-and-hibernate-cascade-types/ 
- **Optimistic vs. Pessimistic Locking**: https://vladmihalcea.com/optimistic-vs-pessimistic-locking/
- **Optimistic locking with JPA and Hibernate**: https://vladmihalcea.com/optimistic-locking-version-property-jpa-hibernate/
- **Eiger/Lazy loading**: https://www.baeldung.com/hibernate-lazy-eager-loading 

## Repositories
- **Custom Repository**: 
    -  https://vladmihalcea.com/custom-spring-data-repository/
    -  https://www.baeldung.com/spring-data-entitymanager
 
 
## Liquibase and Database Migration
Liquibase best practice: https://www.liquibase.org/get-started/best-practices 
Turn off liquibase on spring boot application start: Add `spring.liquibase.enabled=false` to **application.properties** file.

**1. Automatically Generate migration script**

Use `JPA Buddy` IntelliJ plugin that conveniently generates changelog/migration files. However, you have to pay for advance feature.

**2. Update/Rollback database**

Add **`liquibase-maven-plugin`** plugin in `pom.xml`.

The **`liquibase-maven-plugin`** plugin reads `liquibase.properties` file which contains database and changelog settings to update/rollback database. 

liquibase.properties file:
```
url=jdbc:postgresql://localhost:5432/bookstore
username=root
password=12345
driver=org.postgresql.Driver
changeLogFile=src/main/resources/db/changelog/changelog-master.xml
```
`liquibase-maven-plugin` file:
```
            <plugin>
                <groupId>org.liquibase</groupId>
                <artifactId>liquibase-maven-plugin</artifactId>
                <configuration>
                    <propertyFile>src/main/resources/liquibase.properties</propertyFile>
                </configuration>
            </plugin>
```
>**Note:** We don't need this properties for production and no need to worry to hide this credential, as migration file generally get created local/dev environment.

**Commands:**

- To update database: `mvn liquibase:update`
- To rollback changes: `mvn liquibase:rollback -Dliquibase.rollbackTag=1.0` or `mvn liquibase:rollback -Dliquibase.rollbackCount=3` or `mvn liquibase:rollback "-Dliquibase.rollbackDate=Jun 03, 2017"`. [More](https://docs.liquibase.com/tools-integrations/maven/commands/maven-rollback.html)
- To update from parent module - `mvn -pl <module name> liquibase:update`
>Note: In powershell you have use double qoute to run `-Dliquibase` section. e.g. `mvn liquibase:rollback "-Dliquibase.rollbackTag=1.0"`.
>
***>NOTE***: Make sure you run `mvn clean install` to generate target files correctly to perform liquibase operations specially If you are working with multi-module maven projects.

**Maven Liquibase Commands:**

https://docs.liquibase.com/tools-integrations/maven/commands/home.html 

*To run a liquibase maven goal with multiple attributes, use sample example below:* 
```
mvn liquibase:generateChangeLog "-Dliquibase.outputChangeLogFile=src/main/resources/db/changelog/generate-tables.xml" "-Dliquibase.clearCheckSums=true"
```

