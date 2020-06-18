# microservices-eureka-server
Eureka server and Zuul gateway.

In case you'd like to add new service to this application - go to properties and add new rout

*************************< About an application >*************************

There are 4 Spring Boot applications.

1. Eureka-server (https://github.com/KatyshevAlex/microservices-eureka-server)

    MAIN ISSUE:
       
        allows other services to work

    This part of the application make 2 kind of work:
  
        a) registers and maintains all services by using Spring Cloud Eureka
      
        b) receives all requests on { port 8761 } and redirect it in different parts by Spring cloud - Zuul
        
        
      
2. RESTful app - Login service (https://github.com/KatyshevAlex/microservices-login)

     MAIN ISSUE:
      
          This service has to make JWT tokens
          This service has to work with DAO-service as ROLE_APPLICATION
          

    This part allows:
    
        a) to get logged in. << localhost:8761/zuul-prefix/login-service/login/signin >>
        
        b) to get logged out << not implemented >>
        
        c) to get User by JWT token << localhost:8761/zuul-prefix/login-service/login/get-user-by-token >>
        
    You will find in this project things like:
    
        a) { Spring Security } 
        
        b) { JWT builder } 
        
        c) { Spring cloud Feign } 
        
        d) @CrossOrigin  - allows to receive requests from certain sources 
        
        e) Custom annotation @LogExecutionTime by { Aspect oriented programming }
        
        f) @ConfigurationProperties - configs from properties to bean that you can @Autowired
        
        g) { regular expressions } in class PatternConstants
        
        
        
        
  3. RESTful app - DAO service (https://github.com/KatyshevAlex/microservices-dao)
     
     MAIN ISSUE:
      
          This service has to work only for users with ROLE_APPLICATION that login-service has and use 

     This part allows:
      
         a) DAO functions, but the app is protected by Spring Security and only users with ROLE_APPLICATION is allowed
          
         b) Queries,JpaRepositories and so on
          
         c) Custom answers class CustomBasicAuthenticationEntryPoint
              
      Login-service passes Log/Pass with ROLE_APPLICATION by using FeignConfiguration from properties.
      
      Login-service can send requests during execution its own operations.
      
      
      
      
 4. RESTful app - Product service (https://github.com/KatyshevAlex/microservices-product)
 
      MAIN ISSUE:
      
          This service has to work only through JWT authorization. 
          
          It should validate each JWT in login-service and get users by token
          
      This part allows:     
      
          a) to get access to product DB
          b) to use custom OncePerRequestFilter
         


MAIN LOGIC THAT WAS CHECKED:

    1) Get JWT token:
    
        localhost:8761/zuul-prefix/login-service/login/signin
        
        with JSON { "login": "eureka", "password": "password" }
        
    2) JWT that we've got in response  we send in header "Authorization-jwt" with prefix "bearer "
    
       like - "bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOi..." at
        
       http://localhost:8761/zuul-prefix/product-service/product/all-quizzes
       
    3) We should get Quizzes that was added automatically during start.
    
        
        
        
    
    
    
    
    
    


  
  

