# Constitución — Manager Library (React + Laravel)

Principios innegociables del proyecto. El negocio se implementa con React en el frontend y PHP con Laravel en el backend. Python y .NET son únicamente referencias de origen.

## Artículo I — El dominio no depende de Laravel

La carpeta api/app/Domain/ no importa Laravel, Eloquent, controladores, HTTP ni infraestructura. Los agregados, objetos de valor y errores de dominio usan PHP estándar únicamente.

**Verificación:** las pruebas unitarias de api/tests/Unit/Domain/ ejecutan sin base de datos; una revisión impide imports de Illuminate en el dominio.

## Artículo II — La aplicación define puertos; los adaptadores los implementan

La capa de aplicación expresa mediante contratos todo lo que necesita del exterior: repositorios, reloj, hash de claves, tokens y almacenamiento de portadas. Los contratos no exponen Eloquent ni tipos HTTP.

**Verificación:** los casos de uso se prueban con fakes; la aplicación no importa modelos Eloquent ni adaptadores concretos.

## Artículo III — Un único punto de composición

Las implementaciones de puertos se registran en los service providers de Laravel. Los controladores y casos de uso reciben dependencias por inyección; no se construyen adaptadores con new dentro del dominio o la aplicación.

**Verificación:** los bindings viven en api/app/Providers/; el dominio y la aplicación no llaman al contenedor global.

## Artículo IV — SOLID es verificable

| Principio | Aplicación concreta | Verificación |
|---|---|---|
| SRP | Un servicio de aplicación representa un caso de uso | Cada servicio tiene una razón principal para cambiar |
| OCP | Cambiar disco por S3 no toca dominio ni aplicación | Se sustituye el adaptador mediante un binding |
| LSP | Los fakes sustituyen adaptadores reales | El mismo test pasa con fake y adaptador |
| ISP | Puertos pequeños: Clock, BookRepository, CoverStorage | Ningún implementador deja métodos sin usar |
| DIP | El contrato lo define quien lo consume | Los puertos viven en la aplicación |

## Artículo V — El esquema es propiedad de las migraciones de Laravel

El repositorio infra levanta PostgreSQL vacío. Todo el DDL, índices, restricciones y semilla vive en las migraciones de api/database/migrations/. Eloquent no crea el esquema al arrancar.

**Verificación:** infra no contiene CREATE TABLE; php artisan migrate es la única ruta de instalación del esquema.

## Artículo VI — La base impone sus invariantes

Toda invariante expresable como CHECK, UNIQUE o FK se declara en PostgreSQL. Las reglas exclusivas del dominio se mantienen en los agregados y se identifican como pendientes cuando aún no tienen barrera de base de datos.

**Verificación:** las consultas de data-model.md §10 coinciden con la migración real.

## Artículo VII — Lo derivado se calcula

El estado de un préstamo (open, overdue, closed) y los días de retraso no se almacenan como columnas.

## Artículo VIII — Los valores congelados no siguen al catálogo

La línea de préstamo guarda título, ISBN y categoría del momento del préstamo. Los reportes no leen esos datos desde el libro vivo.

## Artículo IX — Ningún secreto entra al repositorio ni a un log

Contraseñas, hashes precalculados y secretos JWT vienen del entorno. El archivo .env real está ignorado; solo se versiona .env.example. Las respuestas nunca exponen credenciales.

## Artículo X — Gana el motor

Si el documento de datos y PostgreSQL discrepan, se corrige el documento o la migración; no se acepta una tercera interpretación.

## Artículo XI — Idioma

Código, columnas, rutas y mensajes de error de la API usan inglés. La documentación y los mensajes visibles al usuario usan español.

## Artículo XII — Nada se da por hecho sin evidencia

Una tarea de tasks.md está terminada cuando su evidencia existe, se ejecuta y corresponde al stack React + Laravel.
