# Exchange Service

The **Exchange Service** provides real-time currency conversion rates for the `store` domain. Unlike other services in this architecture which are built with Java/Spring Boot, this service is implemented using **Python** and **FastAPI**, demonstrating the polyglot nature of the microservices architecture.

!!! info "Polyglot Architecture"
    This service showcases how different technologies can coexist in the same ecosystem, communicating via standard REST APIs.

---

## 🏗️ Architecture

The service acts as a proxy to external currency providers, adding a business layer (spread application) on top of the raw rates.

```mermaid
classDiagram
    class ExchangeService {
        +get_exchange_rate(base, target): ExchangeResponse
    }
    
    class RatesClient {
        +get_rate(base): dict
    }
    
    class ExchangeResponse {
        +str base
        +str target
        +float rate
        +float buy
        +float sell
    }

    ExchangeService --> RatesClient : fetches raw data
    RatesClient ..> External_API : https://open.er-api.com
```

---

## 🔌 API Reference

### Endpoints

| Method | Path | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `GET` | `/exchange/{base}/{target}` | Get the exchange rate between two currencies. | ✅ Yes |

### Data Models

#### `ExchangeResponse`
```json
{
  "base": "USD",
  "target": "BRL",
  "rate": 5.15,
  "buy": 5.253,
  "sell": 5.047
}
```

---

## 🧠 Business Logic

The core logic involves fetching the market rate and applying a configurable spread to determine the "Buy" and "Sell" prices for the store.

### 1. External Integration
The service queries `https://open.er-api.com/v6/latest/{CURRENCY}` to get the latest market rates.

### 2. Spread Calculation
A spread (profit margin) is applied to the market rate.
*   **Buy Rate**: `Market Rate * (1 + Spread)`
*   **Sell Rate**: `Market Rate * (1 - Spread)`

```python
# Snippet from rates.py
rate = data["rates"][target.upper()]
spread = settings.spread  # e.g., 0.02 (2%)

return {
    "base": base.upper(),
    "target": target.upper(),
    "rate": rate,
    "buy": round(rate * (1 + spread), 6),
    "sell": round(rate * (1 - spread), 6),
}
```

---

## ⚙️ Configuration

The service uses **Pydantic** for configuration management, reading from environment variables.

```python
# config.py
class Settings(BaseSettings):
    port: int = 8083
    rates_base_url: str = "https://open.er-api.com"
    spread: float = 0.02  # 2% Spread
    jwt_algorithm: str = "HS256"
```

---

## 📂 Project Structure

The project follows a standard Python/FastAPI structure.

```tree
api/
└── exchange.service/
    ├── app/
    │   ├── __init__.py
    │   ├── main.py             # FastAPI Application
    │   ├── auth.py             # JWT Validation
    │   ├── config.py           # Settings
    │   ├── models.py           # Pydantic Models
    │   └── rates.py            # Business Logic
    ├── requirements.txt        # Python Dependencies
    ├── Dockerfile
    └── k8s/                    # Kubernetes Manifests
```