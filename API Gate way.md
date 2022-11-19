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

![image](https://user-images.githubusercontent.com/115500959/202827056-13979bb4-78be-4b39-ab41-07c6cbaf7a02.png)

