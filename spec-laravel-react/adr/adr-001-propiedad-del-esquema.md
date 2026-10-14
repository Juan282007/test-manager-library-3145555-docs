# ADR-001 · El esquema es propiedad de las migraciones de Laravel

- **Estado:** aceptada
- **Contexto:** infra levanta PostgreSQL. Si el DDL también vive en scripts de arranque o en Eloquent, aparecen varias fuentes de verdad.
- **Decisión:** todo el DDL, índices, restricciones y semilla vive en api/database/migrations/. infra solo crea el motor vacío, el usuario y el volumen.
- **Alternativas descartadas:** scripts SQL en infra/db/init/; Schema::create() al arrancar; sincronización automática del ORM.
- **Consecuencias:** una extensión, índice o restricción nueva se crea en la migración que la necesita. Un cambio manual en producción es un defecto.
- **Cómo se verifica:** infra no contiene CREATE TABLE; php artisan migrate crea el esquema y data-model §10 coincide con él.
