# Tareas — Manager Library (React + Laravel)

Cada tarea termina cuando su evidencia existe y se puede ejecutar. T-01 a T-04 ya tienen implementación en api; las demás permanecen pendientes.

| # | Tarea | Depende de | Evidencia |
|---|---|---|---|
| T-01 | Esqueleto Laravel y GET /health | — | GET /api/v1/health responde {"status":"ok"} |
| T-02 | Migración Laravel: seis tablas, FK, índices y categorías | T-01 | php artisan migrate; consultas de data-model §10 |
| T-03 | Dominio: Isbn, Quantity, DateRange | T-01 | PHPUnit: ISBN 13 dígitos, cantidad > 0, fin ≥ inicio |
| T-04 | Dominio: Book (withdraw, restock, baja lógica) | T-03 | PHPUnit: disponibles nunca < 0 ni > total |
| T-05 | Dominio: Loan (addItem, returnItem, estado, 14 días, máx. 3) | T-04 | PHPUnit con Clock fake: open, overdue, closed |
| T-06 | Dominio: Member y User | T-03 | Nombre normalizado; rol inválido rechazado |
| T-07 | Puertos y casos de uso de autenticación | T-06 | HU-01 y HU-02 con fakes |
| T-08 | Puertos y casos de uso de libros | T-04 | HU-03/HU-04; baja bloqueada con préstamos abiertos |
| T-09 | Caso de uso prestar (LendBooksService) | T-05, T-08 | HU-06: límites, vencidos y disponibilidad |
| T-10 | Devolver y vencidos | T-09 | HU-07/HU-09; doble devolución → ITEM_ALREADY_RETURNED |
| T-11 | Reporte de más prestados | T-09 | HU-10; título congelado tras renombrar |
| T-12 | Repositorios de libro, socio y usuario | T-02, T-08 | Integración contra PostgreSQL real |
| T-13 | Repositorio de préstamos y concurrencia | T-12, T-09 | Último ejemplar: uno 201, otro 409 |
| T-14 | Seguridad: hash y tokens | T-07 | Secretos ausentes de logs y respuestas |
| T-15 | Adaptador de portadas | T-08 | PNG 2 MB; 3 MB → 422; sin portada → null |
| T-16 | Adaptador HTTP y autorización | T-10, T-11, T-14, T-15 | Contrato completo de api-contract.md |
| T-17 | Composición y arranque | T-16 | API inicia con base vacía e inicia sesión el admin |
| T-18 | Invariantes pendientes en PostgreSQL | T-12 | Violaciones de checks fallan |
| T-19 | Pruebas de arquitectura | T-17 | Dominio sin imports Laravel; bindings centralizados |
| T-20 | infra: PostgreSQL, API y frontend | T-17 | docker compose up arranca todo |
| T-21 | tool: sembrador por API | T-17 | Datos de demostración y vencidos |
| T-22 | app: pantallas React | T-16 | Recorrido manual completo |
| T-23 | page: sitio público | — | Correcto en móvil y escritorio |
