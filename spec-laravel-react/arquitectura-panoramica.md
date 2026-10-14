# Arquitectura en una página — Manager Library

El stack real es React + Laravel/PHP; Python y .NET son referencias funcionales.

~~~mermaid
flowchart TB
    U["Bibliotecario / Admin"] --> APP["app / React"]
    V["Visitante"] --> PAGE["page / sitio público"]
    APP -->|"HTTP /api/v1"| API["api / Laravel PHP"]
    API --> PG[("PostgreSQL / library")]
    API --> COV[("Portadas / almacenamiento")]
    TOOL["tool / sembrador"] -->|"usa la API"| API
    INFRA["infra / contenedores"] -.-> PG
    INFRA -.-> API
~~~

| Repo | Papel | Evidencia |
|---|---|---|
| docs | Especificación y decisiones | Documentos coherentes con el stack |
| api | Dominio, aplicación y API Laravel | PHPUnit, integración y contrato |
| app | Pantallas React | Recorrido login → préstamo → reporte |
| page | Presentación pública estática | Funciona sin API |
| infra | Contenedores y PostgreSQL vacío | Arranque reproducible |
| tool | Datos vía API | Catálogo y préstamos de muestra |

1. El préstamo es un hecho inmutable; solo cambia returned_at.
2. Descontar y devolver ejemplares ocurre en la operación que crea o cierra la línea.
3. El estado se calcula: open, overdue o closed.
4. Los valores congelados no siguen al catálogo.
5. PostgreSQL es la última barrera de integridad.
