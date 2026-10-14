# Arquitectura — Manager Library (React + Laravel)

La solución usa arquitectura hexagonal dentro de Laravel: el dominio no conoce el framework, la aplicación define puertos y los adaptadores conectan HTTP, PostgreSQL y almacenamiento.

## 1. Capas y dependencias

~~~mermaid
flowchart LR
    UI["app / React"] -->|"HTTP /api/v1"| HTTP["api / rutas Laravel"]
    subgraph API["api / Laravel"]
      HTTP --> APP["Application / casos de uso + puertos"]
      APP --> DOM["Domain / agregados + objetos de valor"]
      DB["Infrastructure / Eloquent y PostgreSQL"] -. implementa .-> APP
      STO["Infrastructure / almacenamiento"] -. implementa .-> APP
      SEC["Infrastructure / seguridad"] -. implementa .-> APP
    end
    DB --> PG[("PostgreSQL / library")]
~~~

Las dependencias del negocio apuntan hacia dentro. Laravel pertenece a adaptadores y arranque, nunca a app/Domain.

## 2. Estructura del backend

api/app/
├── Domain/                 # agregados y objetos de valor
├── Application/
│   ├── Contracts/           # repositorios, Clock, CoverStorage, TokenIssuer
│   └── Services/            # casos de uso
├── Http/
│   ├── Controllers/         # adaptadores HTTP
│   ├── Requests/            # validación de entrada
│   └── Resources/           # representación JSON
├── Infrastructure/
│   ├── Persistence/         # Eloquent y repositorios
│   ├── Storage/             # portadas
│   └── Security/            # hash y tokens
└── Providers/               # composición y bindings

El frontend vive en el repo app como React y consume únicamente api/v1.

## 3. Agregados

| Raíz | Contiene | Invariantes |
|---|---|---|
| Book | Isbn, copias totales/disponibles, baja lógica | disponibles 0..total; total ≥ 1; título no vacío |
| Loan | colección de LoanItem | 1..3 líneas; sin libro repetido; vencimiento 14 días |
| Member | documento | documento obligatorio y único |
| User | rol, hash | rol admin o librarian |

Loan.AddItem llama a Book.withdraw() antes de añadir la línea. Loan.ReturnItem llama a Book.restock() una sola vez. La persistencia se confirma en una transacción.

## 4. Errores

El dominio y la aplicación lanzan excepciones propias. El adaptador HTTP las traduce al cuerpo y código de api-contract.md. Una excepción de Laravel no llega directamente al cliente.

## 5. Qué no hay

No hay eventos, colas, caché, microservicios ni multi-tenencia hasta que un requisito lo pida.
