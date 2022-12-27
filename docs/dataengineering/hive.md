# Hives

## Setup Hive Metastore with PostgreSQL

[Article](https://normanlimxk.com/2021/06/30/set-up-spark-and-hive-for-data-warehousing-and-processing/)

### Unable to connect to Postgres DB due to the authentication type 10 is not supported

It's necessary configure encryption (Recommended: scram-sha-256)

[StackOverflow](https://stackoverflow.com/questions/64210167/unable-to-connect-to-postgres-db-due-to-the-authentication-type-10-is-not-suppor)

For testing purposes, can do:

```shell
# Remove Line This Line from file /var/lib/postgresql/config/data/pg_hba.conf
host all all all scram-sha-256

# Command to Remove
sed '100d' pg_hba.conf > pg_hba.conf.new
mv pg_hba.conf.new pg_hba.conf
```

GRANT ALL ON SCHEMA public TO hive;

### ERROR: permission denied for schema public

```shell
# Give the following permissions
ALTER DATABASE metastore OWNER TO hive;
```

## hive-site.xml

```xml
<configuration>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:postgresql://localhost:6432/metastore</value>
    <description>PostgreSQL JDBC driver connection URL</description>
  </property>

  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>org.postgresql.Driver</value>
    <description>PostgreSQL metastore driver class name</description>
  </property>

  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>hive</value>
    <description>the username for the DB instance</description>
  </property>

  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>super_secure_password</value>
    <description>the password for the DB instance</description>
  </property>

  <property>
    <name>hive.metastore.schema.verification</name>
    <value>false</value>
  </property>
    
</configuration>
```


