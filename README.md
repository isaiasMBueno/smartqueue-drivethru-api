# SmartQueue Drive-Thru API

A clean and educational ASP.NET Core Web API that simulates a real-world fast-food order flow across **counter**, **drive-thru**, and **delivery** channels.

This project was built as a laboratory assignment, but it goes beyond a basic CRUD by modeling queue behavior, operational constraints, and order lifecycle transitions using clear service-layer business rules.

## Why this project is valuable

Even as a demonstrative project, it explores practical backend engineering concepts:

- **RESTful API design** with semantic routes and proper HTTP verbs.
- **Layered architecture** (Controller + Service + Model + Enums) to keep concerns separated.
- **Dependency Injection** via scoped service registration.
- **Domain-oriented workflow modeling**, representing the lifecycle of an order.
- **Queue prioritization logic** for different origins (counter, drive-thru, delivery).
- **Operational constraints simulation**, such as kitchen concurrency limits and delivery batch thresholds.
- **Input validation and defensive error handling** with clear status code mapping.
- **In-memory state management** for rapid prototyping and learning.

---

## Project overview

The API manages orders (`Pedido`) from creation to completion, including:

1. Creating a new order with an origin.
2. Updating queue position of waiting orders.
3. Moving orders from waiting to preparation.
4. Finishing prepared orders.
5. Delivering delivery orders in batches.
6. Completing in-person pickup.
7. Querying all orders with optional filters.

### Order origins

- `balcao` (counter)
- `drivethru`
- `delivery`

### Order statuses

- `aguardando` (waiting)
- `fazendo` (in preparation)
- `pronto` (ready)
- `entregue` (delivered/completed)

---

## Business rules highlighted

The service layer implements domain behavior that makes the simulation realistic:

- **Kitchen capacity limit:** maximum of 3 orders simultaneously in preparation.
- **Dynamic queue selection:** next order can be selected by origin-based balancing rules.
- **Delivery batching:** delivery dispatch is only performed when exactly 3 ready delivery orders are available.
- **Pickup by ticket (`senha`):** ready orders can be completed individually by identifier.
- **Queue reordering:** waiting orders can be moved to the end of the queue.

---

## Tech stack

- **.NET / ASP.NET Core Web API**
- **C#**
- **Swagger / OpenAPI** (enabled in Development)
- **Postman collection** included (`EndPoints-trabalho`)

---

## API endpoints

Base route: `api/pedidos`

| Method | Route | Description |
|---|---|---|
| `GET` | `/api/pedidos` | List all orders (optional filters: `status`, `origem`) |
| `POST` | `/api/pedidos/{origem}` | Create a new order |
| `PUT` | `/api/pedidos/{senha}` | Move waiting order to the end of the queue |
| `PATCH` | `/api/pedidos/preparar` | Move next eligible order to preparation |
| `GET` | `/api/pedidos/finalizar` | Finish the first order currently being prepared |
| `GET` | `/api/pedidos/entrega` | Deliver ready `delivery` orders in batch |
| `DELETE` | `/api/pedidos/{senha}` | Complete pickup/delivery for a specific ready order |

---

## Getting started

### Prerequisites

- .NET SDK 5.0+ (recommended: latest LTS)
- Any API client (Swagger UI, Postman, Insomnia)
- IDE/editor (Visual Studio or VS Code)

### Run locally

```bash
dotnet restore
dotnet run
```

When running in Development, Swagger UI is available automatically.

---

## Project structure

```text
Controllers/
  PedidosController.cs   # HTTP contract and response mapping
Services/
  IPedidosService.cs     # Application service contract
  PedidosService.cs      # Core business rules and queue orchestration
Models/
  Pedido.cs              # Order entity representation
Enums/
  eOrigemPedido.cs       # Order origin types
  eStatusPedido.cs       # Order lifecycle statuses
Program.cs               # App bootstrap and DI configuration
```

---

## Suggested repository name

**Recommended:** `smartqueue-drivethru-api`

Other good options:

- `orderflow-drivethru-simulator`
- `fastlane-orders-api`
- `restaurant-queue-workflow-api`

---

## Educational outcomes

This project is especially useful for practicing:

- API lifecycle thinking instead of isolated endpoints.
- Translating real business rules into deterministic code.
- Modeling state transitions with enums and controlled operations.
- Structuring medium-sized backend code for maintainability.

---

## Future enhancements

Potential next steps to evolve this demo into production-ready architecture:

- Persist data with Entity Framework Core + relational database.
- Add automated tests (unit + integration).
- Implement structured logging and observability.
- Version the API and improve contract documentation.
- Introduce authentication/authorization.
- Containerize with Docker and CI pipeline.

---

## License

Educational use project. Feel free to study, adapt, and extend.
