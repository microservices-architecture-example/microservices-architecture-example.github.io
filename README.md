# Microservices Architecture Example Store

[![Java](https://img.shields.io/badge/Java-17%2B-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://www.java.com/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.0-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Python](https://img.shields.io/badge/Python-3.10-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.95-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![AWS](https://img.shields.io/badge/AWS-Cloud-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)

A high-performance, polyglot microservices architecture for a modern e-commerce backend. Designed with scalability, security, and maintainability in mind, this project demonstrates a production-ready implementation of the **Database-per-Service** pattern.

[📚 **Read the Full Documentation**](https://microservices-architecture-example.github.io/)

---

## 🚀 Architecture Overview

The system is composed of loosely coupled services communicating via REST APIs, with a centralized **API Gateway** handling routing and security.

```mermaid
graph TD
    user((User)) -->|HTTPS| gateway[API Gateway]
    
    subgraph "Trusted Layer (Private Subnet)"
        gateway -->|Auth Check| auth[Auth Service]
        gateway -->|/account| account[Account Service]
        gateway -->|/product| product[Product Service]
        gateway -->|/order| order[Order Service]
        gateway -->|/exchange| exchange[Exchange Service]
        
        order -->|Feign| product
        exchange -->|HTTP| external[External Rates API]
    end
    
    account --> db1[(PostgreSQL)]
    product --> db2[(PostgreSQL)]
    order --> db3[(PostgreSQL)]
```

## 🛠️ Services

| Service | Tech Stack | Description |
| :--- | :--- | :--- |
| **Gateway** | Java, Spring Cloud Gateway | Edge server acting as the single entry point. Handles **JWT validation**, **CORS**, and **Rate Limiting**. |
| **Auth** | Java, Spring Boot, JWT | Centralized identity provider. Issues **HMAC256** signed tokens. |
| **Account** | Java, Spring Boot | Manages user profiles and authentication data. |
| **Product** | Java, Spring Boot | Product catalog management with **Flyway** migrations. |
| **Order** | Java, Spring Boot | Orchestrates order processing, validating stock and calculating totals via inter-service communication. |
| **Exchange** | **Python, FastAPI** | Real-time currency conversion service, demonstrating a **polyglot architecture**. |

## ✨ Key Features

*   **Polyglot Architecture**: Seamless integration of Java (Spring Boot) and Python (FastAPI) services.
*   **Centralized Security**: Stateless authentication using JWT, enforced at the Gateway level.
*   **Infrastructure as Code**: Fully containerized with Docker and orchestrated via Kubernetes (EKS).
*   **CI/CD**: Automated pipelines using Jenkins and GitHub Actions.
*   **Database Isolation**: Each service owns its data schema, ensuring loose coupling.

---

## 📦 Documentation Setup

This repository contains the source code for the project documentation, built with **MkDocs**.

### Prerequisites
*   Python 3.10+

### Running Locally

1.  Create a virtual environment:
    ```shell
    python3 -m venv env
    source ./env/bin/activate
    ```

2.  Install dependencies:
    ```shell
    python3 -m pip install -r requirements.txt --upgrade
    ```

3.  Start the development server:
    ```shell
    mkdocs serve
    ```

4.  Access the docs at `http://127.0.0.1:8000`.