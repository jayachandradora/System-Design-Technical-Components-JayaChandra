# Api Gate Way

Details Of API Gate way

1. Clien sends the request to API gateway. It could be tipically http rest or http graphQL.
2. Validate the http request.
3. Api gateway checks callers IP address and other http headers to allow or denail of lists of requests.
4. Then it pass the request to Identity manager to identity provider for authentication or authorization.
5. then pass the request to rate limit check to allow(below limit) or denail(over limit) of request.
6. With help of both Dynamic routing and service discovery it locates the resources of a service with path matching.
7. Http protocol communication happens. it could be gRpc, http etc.
8. Additionally, API gateway could do logging, Monitoring, circut breaker, analytics & caching etc.

![image](https://user-images.githubusercontent.com/115500959/201331038-32eafbd9-347d-49bf-9385-2e905080b1e2.png)

# API GateWay + Service Registry & Service Discovery

![image](https://user-images.githubusercontent.com/115500959/202827056-13979bb4-78be-4b39-ab41-07c6cbaf7a02.png)

![image](https://user-images.githubusercontent.com/115500959/203079620-b6725c61-0d16-4f3e-805b-1ef817968607.png)

![image](https://user-images.githubusercontent.com/115500959/203079850-bb5ea9b7-efd7-4036-b29e-168bb6803baa.png)

![image](https://user-images.githubusercontent.com/115500959/203082508-adb73dff-cc22-4fff-a10f-240dbf45e241.png)


# graphQL 

GraphQL is a query language for API developed by Meta. It provides a schema of the data in the API and gives clients the power to ask for exactly what they need.

In this video, we talk about:
- What is GraphQL
- When to use it
- Trade-offs
- GraphQL vs. REST

The diagram below shows the quick comparison between REST and GraphQL.

🔹GraphQL is a query language for APIs developed by Meta. It provides a complete description of the data in the API and gives clients the power to ask for exactly what they need.

🔹GraphQL servers sit in between the client and the backend services.

🔹GraphQL can aggregate multiple REST requests into one query. GraphQL server organizes the resources in a graph.

🔹GraphQL supports queries, mutations (applying data modifications to resources), and subscriptions (receiving notifications on schema modifications).

Watch the video now: https://lnkd.in/ekHvFCxS

![image](https://user-images.githubusercontent.com/115500959/202827939-58818a48-d2ea-46da-a639-7e753571ea21.png)

Following is a quick summary of differences between the two.

1. REST is architectural concept. GraphQL is a specification.
2. REST defines multiple endpoints. GraphQL defines a single endpoint.
3. REST supports multiple HTTP methods (GET, POST, PUT, DELETE, PATCH). GraphQL only allows GET and POST.
4. REST GET URLs can be cached, stored in browser history, bookmarked. GraphQL GET cannot be bookmarked.
5, REST allows creation of a dedicated endpoint (/health) for monitoring. In case of GraphQL, the monitoring tool needs the ability to use query parameter or read request body to check for health status.
6. REST uses a server driven architecture, GraphQL has client driven architecture.
7. REST does not provide type safety, GraphQL provides type safety.
8. In REST the HTTP status code 200 denotes success and the remaining codes denote different types of errors. In GraphQL, irrespective of success/failure the status code is 200, the response body needs to be interpreted to determine success/failure and in case of failures, failure details.
9. REST uses API versioning, GraphQL does not need it.
10. In REST retrieving information of multiple resources, requires multiple HTTP requests and the interaction is chatty. In GraphQL all resources can be requested in a single request and the response will provide all the information.

So when do we use GraphQL
- In bandwidth sensitive use cases like mobile devices
- For resource information consolidation use cases
- Front ending multiple microservices API to provide a consolidated response.


