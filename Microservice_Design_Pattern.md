## Microservice Architecture and Design Patterns for Microservices
Src: https://medium.com/@madhukaudantha/microservice-architecture-and-design-patterns-for-microservices-e0e5013fd58a
src: https://microservices.io/patterns/decomposition/decompose-by-subdomain.html
![image](https://user-images.githubusercontent.com/69948118/182506406-96eda3d8-bab5-4f10-a1ac-130278a524fd.png)
![image](https://user-images.githubusercontent.com/69948118/182509521-b4072042-f2dd-4939-bb2a-061e6ebb51a5.png)


### Decomposition Patterns
1. Decompose by Business Capability
![image](https://user-images.githubusercontent.com/69948118/182508629-56a04f9d-3803-4953-bac3-03f2aae10ee6.png)

3. Decompose by Subdomain
 ![image](https://user-images.githubusercontent.com/69948118/182508724-f606d5c6-7163-4dea-a00a-1474860e17ca.png)
 
4. Decompose by Transactions / Two-phase commit (2pc) pattern
5. Strangler Pattern
![image](https://user-images.githubusercontent.com/69948118/182509322-f5d1c1ee-4f6e-47c6-8bbb-0560c0bca94a.png)

### Integration Patterns
1. API Gateway Pattern
![image](https://user-images.githubusercontent.com/69948118/182509684-692c6637-6e3a-4d08-ab43-7dafbe95f9ad.png)

2. Aggregator Pattern
![image](https://user-images.githubusercontent.com/69948118/182510048-031041d1-b96b-4cdb-8314-08aaf5c0d605.png)

3. Proxy Pattern
![image](https://user-images.githubusercontent.com/69948118/182510147-2bff4fad-27cb-442c-ad5f-8fe7b473a793.png)

4. Gateway Routing Pattern
The API gateway is responsible for request routing. An API gateway implements some API operations by routing requests to the corresponding service.

5. Chained Microservice Pattern
![image](https://user-images.githubusercontent.com/69948118/182510324-55c3ad5b-6603-420b-a0f7-ae9f9149ffc7.png)

6. Branch Pattern
![image](https://user-images.githubusercontent.com/69948118/182510524-67eb0616-26c1-4d46-b16b-24134d8e89a1.png)

7. Client-Side UI Composition Pattern
![image](https://user-images.githubusercontent.com/69948118/182510663-9c4e31cc-dd52-436c-866f-19fbc03160e9.png)

### Database Patterns
1. Database per Service
![image](https://user-images.githubusercontent.com/69948118/182510939-2679822e-8902-4916-a05d-885a6dac4f7e.png)

2.Shared Database per Service

3. Command Query Responsibility Segregation (CQRS)
![image](https://user-images.githubusercontent.com/69948118/182511427-e434b8ec-9345-4be4-9d6a-b21a2526b483.png)

4. Event Sourcing

5.Saga Pattern
![image](https://user-images.githubusercontent.com/69948118/182512068-aa2b3396-1fdb-4f84-9842-9f69cdf85a2f.png)


### Observability Patterns
1.Log Aggregation
2.Performance Metrics
3.Distributed Tracing
![image](https://user-images.githubusercontent.com/69948118/182512712-d4d664df-4422-4067-a598-e439add3d37e.png)

4. Health Check
![image](https://user-images.githubusercontent.com/69948118/182512787-e3be595d-dba2-4e46-a6a9-3b4e4592db3d.png)


### Cross-Cutting Concern Patterns
1.External Configuration
![image](https://user-images.githubusercontent.com/69948118/182512896-aa6f95c4-a7ae-4a9c-809f-3a1c99599e8a.png)

2.Service Discovery Pattern
![image](https://user-images.githubusercontent.com/69948118/182513079-0fee67dc-b22a-4f72-a273-088938d21148.png)

3.Circuit Breaker Pattern
![image](https://user-images.githubusercontent.com/69948118/182513301-a7214669-00ef-4fdd-8aca-80e1798ad384.png)

4. Blue-Green Deployment Pattern

![image](https://user-images.githubusercontent.com/69948118/182513450-f8d6cea6-003f-4c3d-aa59-377ac21e2a96.png)

5.










