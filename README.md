# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. 
Explain based on your understanding of Observer design patterns, 
do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

**ANS:** In the Observer pattern diagram, Subscriber is implemented as an interface.
With this, the purpose of reducing the dependency of the Publisher on concrete Subscriber classes will be achieved. 
Also, by using interface it will help to enhance flexibility and maintainability of BambangShop's program.
This allows for creating alternative implementations of concrete Subscribers without needing to modify existing code. 

2. id in Program and url in Subscriber is intended to be unique. 
Explain based on your understanding, 
is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

**ANS:** Using DashMap (map/dictionary) is better than using Vec (list.)
This is because: When using Vec, ensuring the uniqueness of an id entails an inefficient iteration through the entire list (with a complexity of O(N)). 
Conversely, DashMap allows for direct access, guaranteeing the uniqueness of ids (with a complexity of O(1)).

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. 
In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. 
Explain based on your understanding of design patterns, 
do we still need DashMap or we can implement Singleton pattern instead?

**ANS:** Dashmap is a better choice because it ensures that operations on the static variable SUBSCRIBERS can be safely performed by multiple threads concurrently. 
On the other hand, the Singleton Pattern does not always guarantee thread safety, especially in cases of concurrency. 

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”.
Model in MVC covers both data storage and business logic.
Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

**ANS:** In the traditional MVC pattern, the Model typically encompasses both data storage and business logic. 
However, as applications grow in complexity, it becomes beneficial to separate concerns further by introducing the concepts of "Service" and "Repository". Here's why:

- Separation of Concerns: By separating the Model into Service and Repository layers, we adhere more closely to the principle of separation of concerns. The Service layer handles the application's business logic, while the Repository layer deals with data access and persistence. This separation makes the codebase easier to understand, maintain, and scale.
- Flexibility and Reusability: Separating the Model into Service and Repository layers makes it easier to swap out implementations. 
- Testing: Separating concerns makes it easier to unit test different parts of the application in isolation. 

2. What happens if we only use the Model?
Explain your imagination on how the interactions between each model (Program, Subscriber, Notification)
affect the code complexity for each model?

**ANS:** If we only use the Model without separating it into Service and Repository layers, the interactions between different models (such as Program, Subscriber, and Notification) would likely become tightly coupled.
This could lead to increased code complexity and decreased maintainability as the application grows. For example:

- Without a clear separation of concerns, business logic related to Programs, Subscribers, and Notifications might become intertwined within the Model layer.
- As the application expands and new features are added, managing the interactions and dependencies between different models could become challenging.
- Code related to data access and manipulation might also clutter the Model layer, making it harder to understand and maintain.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work.
You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

**ANS:** Postman is a powerful tool for testing APIs and collaborating on API development. Postman provides a user-friendly interface for sending requests to APIs and inspecting responses.
Usually, I use Postman to send HTTP requests to test server endpoints and verify if the responses align with expectations.
Postman allows users to organize requests into collections and define environments with variables. 
This makes it easy to manage and execute sets of related requests, streamlining the testing process.

#### Reflection Publisher-3
