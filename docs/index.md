# Microservices Architecture Example

**A Cloud-Native E-Commerce Microservices Architecture built with Spring Boot and Kubernetes.**

Este projeto é um exemplo prático e robusto de uma arquitetura de microserviços, demonstrando como construir uma aplicação escalável, resiliente e moderna. O objetivo é fornecer uma referência para padrões de design, integração de sistemas e práticas de DevOps.

---

## 🚀 Tech Stack

Este projeto utiliza uma stack moderna focada em performance e escalabilidade:

*   **Backend**: Java 17+, Spring Boot 3, Spring Cloud (Gateway, OpenFeign).
*   **Data**: PostgreSQL, Redis, Flyway (Migrations).
*   **DevOps**: Docker, Kubernetes (EKS), Jenkins (CI/CD).

---

## 🏗️ Arquitetura e Design

A arquitetura segue o padrão de **API Gateway** com bancos de dados isolados por serviço (**Database per Service**), garantindo desacoplamento e autonomia.

### Diagrama de Componentes

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

### Serviços

O sistema é composto pelos seguintes microserviços, cada um com responsabilidade única:

| Serviço | Responsabilidade | Interface | Implementação |
| :--- | :--- | :--- | :--- |
| **Gateway Service** | Ponto de entrada único, roteamento e segurança. | - | [gateway-service](https://github.com/microservices-architecture-example/gateway.service) |
| **Auth Service** | Autenticação e autorização (JWT). | [auth](https://github.com/microservices-architecture-example/auth) | [auth-service](https://github.com/microservices-architecture-example/auth.service) |
| **Account Service** | Gestão de usuários e contas. | [account](https://github.com/microservices-architecture-example/account) | [account-service](https://github.com/microservices-architecture-example/account.service) |
| **Product Service** | Catálogo e gestão de produtos. | [product](https://github.com/microservices-architecture-example/product) | [product-service](https://github.com/microservices-architecture-example/product.service) |
| **Order Service** | Gestão de pedidos e fluxo de compra. | [order](https://github.com/microservices-architecture-example/order) | [order-service](https://github.com/microservices-architecture-example/order.service) |
| **Exchange Service** | Cotação de moedas (integração externa). | - | [exchange-service](https://github.com/microservices-architecture-example/exchange.service) |

---

## 🌟 Principais Funcionalidades

*   **Autenticação Segura**: Implementação stateless com JWT.
*   **Configuração Externalizada**: Gestão de configuração centralizada.
*   **Resiliência**: Padrões de tolerância a falhas na comunicação entre serviços.
*   **CI/CD Automatizado**: Pipelines de build e deploy com Jenkins.
*   **Infraestrutura como Código**: Manifests Kubernetes organizados para deploy no EKS.

---

## 🔗 Repositório Principal

Para acessar o código fonte completo e a orquestração dos serviços:
[**microservices-architecture-example/all**](https://github.com/microservices-architecture-example/all)