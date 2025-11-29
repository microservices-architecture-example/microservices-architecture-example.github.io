# Microservices Architecture Example

**A Cloud-Native E-Commerce Microservices Architecture built with Spring Boot and Kubernetes.**

This project is a practical and robust example of a microservices architecture, demonstrating how to build a scalable, resilient, and modern application. It serves as a reference for design patterns, system integration, and DevOps practices.

---

## 🚀 Tech Stack

This project leverages a modern stack focused on performance, scalability, and cloud-native deployment:

### Backend & Frameworks
![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white)
![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-6DB33F?style=for-the-badge&logo=Spring&logoColor=white)

### Data & Persistence
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![Flyway](https://img.shields.io/badge/Flyway-CC0200?style=for-the-badge&logo=flyway&logoColor=white)

### DevOps & Cloud
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326CE5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Jenkins](https://img.shields.io/badge/jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white)

---

## ☁️ AWS Integration

This architecture is designed to run natively on **Amazon Web Services (AWS)**, leveraging managed services for high availability and minimal operational overhead.

*   **EKS (Elastic Kubernetes Service)**: Orchestrates the microservices containers.
*   **ALB/NLB (Load Balancers)**: Manages external traffic ingress to the API Gateway.
*   **RDS (Relational Database Service)**: (Optional) Can be configured to host the PostgreSQL databases.
*   **ElastiCache**: (Optional) Managed Redis for caching.

---

## 🏗️ Architecture & Design

The architecture follows the **API Gateway** pattern with isolated databases per service (**Database per Service**), ensuring decoupling and autonomy.

### Component Diagram

``` mermaid
flowchart LR
    subgraph api [Subnet API]
        direction TB
        gateway --> account
        gateway --> auth:::red
        gateway --> product
        gateway --> order
        gateway --> exchange
        auth --> account
        order --> product
        account --> db@{ shape: cyl, label: "Database" }
        product --> db
        order --> db
    end
    exchange e3@==> 3partyapi:::green@{label: "3rd-party API"}
    internet e2@==> |request| gateway:::orange
    e2@{ animate: true }
    e3@{ animate: true }
    classDef green fill:#cfc
    classDef orange fill:#FCBE3E
```

### Services

The system is composed of the following microservices, each with a unique responsibility:

| Service | Responsibility | Interface | Implementation |
| :--- | :--- | :--- | :--- |
| **Gateway Service** | Single entry point, routing, and security. | - | [gateway-service](https://github.com/microservices-architecture-example/gateway.service) |
| **Auth Service** | Authentication and authorization (JWT). | [auth](https://github.com/microservices-architecture-example/auth) | [auth-service](https://github.com/microservices-architecture-example/auth.service) |
| **Account Service** | User and account management. | [account](https://github.com/microservices-architecture-example/account) | [account-service](https://github.com/microservices-architecture-example/account.service) |
| **Product Service** | Product catalog and management. | [product](https://github.com/microservices-architecture-example/product) | [product-service](https://github.com/microservices-architecture-example/product.service) |
| **Order Service** | Order management and purchase flow. | [order](https://github.com/microservices-architecture-example/order) | [order-service](https://github.com/microservices-architecture-example/order.service) |
| **Exchange Service** | Currency exchange rates (external integration). | - | [exchange-service](https://github.com/microservices-architecture-example/exchange.service) |

---

## 🌟 Key Features

*   **Secure Authentication**: Stateless implementation with JWT.
*   **Externalized Configuration**: Centralized configuration management.
*   **Resilience**: Fault tolerance patterns in inter-service communication.
*   **Automated CI/CD**: Build and deploy pipelines with Jenkins.
*   **Infrastructure as Code**: Organized Kubernetes manifests for EKS deployment.

---

## 🔗 Main Repository

To access the complete source code and service orchestration:
[**microservices-architecture-example/all**](https://github.com/microservices-architecture-example/all)