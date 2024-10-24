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

## Designing a GraphQL API in Java

Designing a GraphQL API in Java can be accomplished using libraries like **Spring Boot** with **GraphQL Java**. Below is a step-by-step guide to creating a simple GraphQL API for a blog application.

### Step 1: Set Up Your Project

You can create a new Spring Boot project using Spring Initializr (https://start.spring.io/) with the following dependencies:

- Spring Web
- Spring Boot DevTools
- Spring Data JPA
- H2 Database (for an in-memory database)
- GraphQL Spring Boot Starter

### Step 2: Define Your Data Model

Create entities for `User`, `Post`, and `Comment`. Here’s a simple example:

```java
import javax.persistence.*;
import java.util.List;

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;

    @OneToMany(mappedBy = "author")
    private List<Post> posts;

    // Getters and Setters
}

@Entity
public class Post {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String content;

    @ManyToOne
    @JoinColumn(name = "user_id")
    private User author;

    @OneToMany(mappedBy = "post")
    private List<Comment> comments;

    // Getters and Setters
}

@Entity
public class Comment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String content;

    @ManyToOne
    @JoinColumn(name = "user_id")
    private User author;

    @ManyToOne
    @JoinColumn(name = "post_id")
    private Post post;

    // Getters and Setters
}
```

### Step 3: Create Repositories

Define Spring Data JPA repositories for each entity:

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {}
public interface PostRepository extends JpaRepository<Post, Long> {}
public interface CommentRepository extends JpaRepository<Comment, Long> {}
```

### Step 4: Define the GraphQL Schema

Create a GraphQL schema file (`schema.graphqls`) in the `src/main/resources` directory:

```graphql
type User {
    id: ID!
    name: String!
    email: String!
    posts: [Post]
}

type Post {
    id: ID!
    title: String!
    content: String!
    author: User!
    comments: [Comment]
}

type Comment {
    id: ID!
    content: String!
    author: User!
    post: Post!
}

type Query {
    users: [User]
    user(id: ID!): User
    posts: [Post]
    post(id: ID!): Post
}

type Mutation {
    createUser(name: String!, email: String!): User
    createPost(title: String!, content: String!, authorId: ID!): Post
    createComment(content: String!, authorId: ID!, postId: ID!): Comment
}
```

### Step 5: Implement Resolvers

Create a GraphQL resolver to handle queries and mutations:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import graphql.kickstart.tools.GraphQLMutationResolver;
import graphql.kickstart.tools.GraphQLQueryResolver;

import java.util.List;

@Component
public class BlogResolver implements GraphQLQueryResolver, GraphQLMutationResolver {

    @Autowired
    private UserRepository userRepository;
    @Autowired
    private PostRepository postRepository;
    @Autowired
    private CommentRepository commentRepository;

    // Queries
    public List<User> getUsers() {
        return userRepository.findAll();
    }

    public User getUser(Long id) {
        return userRepository.findById(id).orElse(null);
    }

    public List<Post> getPosts() {
        return postRepository.findAll();
    }

    public Post getPost(Long id) {
        return postRepository.findById(id).orElse(null);
    }

    // Mutations
    public User createUser(String name, String email) {
        User user = new User();
        user.setName(name);
        user.setEmail(email);
        return userRepository.save(user);
    }

    public Post createPost(String title, String content, Long authorId) {
        Post post = new Post();
        post.setTitle(title);
        post.setContent(content);
        post.setAuthor(userRepository.findById(authorId).orElse(null));
        return postRepository.save(post);
    }

    public Comment createComment(String content, Long authorId, Long postId) {
        Comment comment = new Comment();
        comment.setContent(content);
        comment.setAuthor(userRepository.findById(authorId).orElse(null));
        comment.setPost(postRepository.findById(postId).orElse(null));
        return commentRepository.save(comment);
    }
}
```

### Step 6: Application Properties

Configure your application in `src/main/resources/application.properties` for the H2 database:

```properties
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
```

### Step 7: Run Your Application

Run your Spring Boot application. You should be able to access the GraphQL endpoint at `http://localhost:8080/graphql`.

### Step 8: Example Queries and Mutations

1. **Query all users:**

```graphql
query {
  users {
    id
    name
    email
  }
}
```

2. **Fetch a specific post:**

```graphql
query {
  post(id: 1) {
    title
    content
    author {
      name
    }
    comments {
      content
      author {
        name
      }
    }
  }
}
```

3. **Create a new user:**

```graphql
mutation {
  createUser(name: "Charlie", email: "charlie@example.com") {
    id
    name
  }
}
```

### Summary

This example demonstrates how to create a simple GraphQL API in Java using Spring Boot. You can define your data model, set up repositories, create a GraphQL schema, and implement resolvers to handle queries and mutations. With this setup, you can efficiently manage data for your application using GraphQL.


