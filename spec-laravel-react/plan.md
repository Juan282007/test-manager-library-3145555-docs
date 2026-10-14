# Plan técnico — Manager Library (React + Laravel)

Este plan traduce el producto de spec.md a React, PHP con Laravel y PostgreSQL. El esquema vive en data-model.md y la separación de capas en architecture.md.

## 1. Decisiones técnicas

| # | Decisión | Elección | Por qué |
|---|---|---|---|
| D-01 | Frontend | React con Vite | Interfaz de operación separada de la API |
| D-02 | Backend | PHP 8.3 + Laravel 13 | Rutas versionadas, inyección, validación y PHPUnit |
| D-03 | Persistencia | PostgreSQL + migraciones Laravel | Un solo dueño del esquema (ADR-001) |
| D-04 | Dominio | Clases PHP puras en app/Domain | Pruebas sin framework ni base |
| D-05 | API | JSON versionado bajo /api/v1 | Contrato común para React y herramientas |
| D-06 | Concurrencia | Testigo de versión y CHECK de ejemplares (ADR-002) | Protege el último ejemplar |
| D-07 | Fechas | Instantes UTC; reloj por puerto | Vencimientos determinísticos |
| D-08 | Reporte | Consulta agregada detrás de un puerto de lectura | Sin acoplar dominio a SQL |
| D-09 | Portadas | Binario externo; PostgreSQL guarda cover_key | La clave es opaca |
| D-10 | Autenticación | Hash seguro y tokens por entorno | Secretos fuera del repositorio |
| D-11 | Pruebas | PHPUnit, integración PostgreSQL y contrato HTTP | Evidencia proporcional al riesgo |

## 2. Configuración por entorno

| Variable | Obligatoria | Ejemplo |
|---|---|---|
| DATABASE_URL | sí | postgresql://library:library@db:5432/library |
| DB_HOST, DB_PORT, DB_DATABASE, DB_USERNAME, DB_PASSWORD | sí fuera de Docker | valores locales de PostgreSQL |
| JWT_SECRET | cuando se habilite autenticación | cadena larga aleatoria |
| CORS_ORIGIN | sí | http://localhost:5173 |

## 3. Estrategia de pruebas

| Nivel | Qué prueba | Contra qué |
|---|---|---|
| Dominio | Invariantes y transiciones | Clases PHP puras |
| Aplicación | Casos de uso | Fakes de puertos |
| Integración | Migraciones y repositorios | PostgreSQL real |
| Contrato | JSON, códigos y autorización | API Laravel |
| Arquitectura | Dependencias hacia dentro | PHPUnit y revisión de namespaces |
| E2E | Recorridos de operación | React + API |

## 4. Orden de construcción

Esqueleto y salud → migración inicial → objetos de valor → agregados → casos de uso con fakes → repositorios y adaptadores → API → React → infraestructura y datos de demostración. El detalle está en tasks.md.
