Description
This Assignement shows the User which are stored in the In-Memory H2 Database. Using the following endpoints, different operations can be achieved:

1> /login (Synchronous, just send the username and password in the post body. User will be authenticated and can perform the below operations.
2> /profile (Synchronous rest call between Authorization and Profile Service, user will be creating their own profile info if they are authenticated.
3> /profile (Event based) (If user is authenticated they can update their own profile only.
4> /profile (Event based) (If user is authenticated they can delete their own profile only.
------------------------------------------------------------------------------------------------------------------------------------------------------------
to Open Authorization-service project database on Google Chrome use 
http://localhost:8023/h2-console 
	 {Enter Driver Class:org.h2.Driver
		 JDBC URL:jdbc:h2:mem:user
		 User Name:datta
		 Password:1234
	 }
this mention is application.properties in Authorization-web service.

create manually user to write Sql Query
insert into user values('datta','123');

then hit first operation for login
localhost:9090/assignement/login
	in Body write followin JSON as enter in database
	{
	"username" : "datta",
	"password":"123"
	}

this will return JWT token and we use for this for Authorization

then go for 2 POST operation i.e profile creation with Authentication
then hit following url on postman
localhost:9090/assignement/profile
In header 
Auth : Bearer [past JWT token here]
in body
	{
		"username":"datta",
		"address":"Aurangabad",
		"phoneNumber":"12345678"
	}
---------------------------------------------------------------------------------------------------------------------------------------------------
After this we go with Kafka for PUT and Delete operation
 Run zookeeper server
1> .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
 Run Kafka Server
2>  kafka-server-start.bat server.properties	
	Generate topics only first time in system
3> kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 2 --topic Profile-Updater-IN
start kafka consumer
4> kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic Profile-Updater-IN --from-beginning

after above 4 steps we perform put and delete operation

open postman

PUT -> localhost:9090/assignement/profile/datta
Auth : Bearer [past JWT token here]
{
"username":"datta",
"address":"Pune",
"phoneNumber":"12345678"
}

See data in kafka consumer for put operation

then check this in H2 database for Profile service Database
open with following URL 
	http://localhost:8024/h2-console
write query to check data
	select * from user_profile;

then for delete operation
DELETE -> localhost:9090/assignement/profile/datta
Auth : Bearer [past JWT token here]

see deleted data in kafka consumer

-------------------------------------------------------------------------------------
Libraries used
Spring Boot
Spring Configuration
Spring REST Controller
Spring JPA
H2
Spring Security
Spring kakfa
Spring Web
Spring-boot-Eureka 
Zuul-service

Development Tools
STS 4.6.0
kafka_2.13-2.4.0

