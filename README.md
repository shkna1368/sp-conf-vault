# sp-conf-vault
download from : https://www.vaultproject.io/downloads
tutprial:https://www.techgeeknext.com/spring-boot/spring-cloud-vault
create vaultconfig.hcl in vault folder:
-----------------
storage "file" {
	path= "./vault-data"
}


listener "tcp" {
  address = "127.0.0.1:8200"
  tls_disable =1
}
api_addr = "http://127.0.0.1:8200"
cluster_addr = "https://127.0.0.1:8201"
ui = true

disable_mlock=true

---------------

vault server -config ./vaultconfig.hcl

in new cmd:
 vault operator init
 copy token from : Initial Root Token: s.LqgTdZ5JkfiOXbFuFE8dA0tH
set VAULT_TOKEN=s.LqgTdZ5JkfiOXbFuFE8dA0tH
set VAULT_ADDR=http://localhost:8200
unstael keys until 3 otems :vault operator unseal oeZU/5V3gicAq15dPx3hcv4ltMJLkiNpVi/xeZeprLc1

vault secrets enable -path=secret/ kv

vault kv put secret/shababApp dbusername=postgres  dbpassword=Shkna1368

in code:
pom.xml:
 <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-vault-config</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bootstrap</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-vault-config-databases</artifactId>
        </dependency>


bootstrap.yml
spring:
  application:
    name: shababApp
  cloud:
    vault:
      host: localhost
      port: 8200
      scheme: http
      token: s.LqgTdZ5JkfiOXbFuFE8dA0tH


in application.yml:

server:
  port: 8070
spring:
  jpa:
    hibernate:
      ddl-auto: update
    database-platform: org.hibernate.dialect.PostgreSQLDialect
  datasource:
    url: "jdbc:postgresql://localhost:5432/carmanagment"
    username: ${dbusername}
    password: ${dbpassword}



