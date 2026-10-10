# Actividad 2: Hosting de bases de datos a largo plazo

## 1. Objetivo

Investigar si existe un servicio donde pueda subir mi base de datos y que **siga funcionando durante 10 o 20 años (o de por vida) sin que yo tenga que estar entrando a mantenerla viva**, y si no existe, encontrar lo más cercano.

**Fecha de la investigación:** 9 de octubre de 2026.

## 2. Cómo hice la investigación

1. Busqué proveedores que ofrecen bases de datos gratuitas en cuatro categorías: SQL administrado, SQLite en la nube, NoSQL de las nubes grandes y máquinas virtuales gratuitas donde yo mismo instalo la base.
2. Para cada candidato revisé la **documentación oficial** y no solo blogs, y busqué específicamente la letra chica: qué pasa si la base **no se usa** (se pausa, se apaga, se archiva o se borra).
3. Busqué casos de proveedores que **ya eliminaron** su plan gratuito, para ver qué tan confiable es la promesa de "gratis para siempre".
4. Clasifiqué las opciones según si hay que entrar periódicamente para que la base no se pierda.

## 3. Respuesta corta

**No existe ningún hosting de bases de datos que garantice por contrato 20 años de servicio, ni gratis ni de pago.** Lo más parecido son planes que el proveedor describe como permanentes ("always free", "para toda la vida de la suscripción"), pero eso es una política que la empresa puede cambiar.

Lo que sí hay son tres niveles de "qué tan cerca estoy":

| Nivel | Qué es | Ejemplos encontrados |
|---|---|---|
| 1. Gratis, pero hay que mantenerla viva | La base se detiene, se pausa o se archiva sola si no la uso, y en algunos casos se puede borrar | Oracle Autonomous Database, Supabase gratis, MongoDB Atlas gratis, Aiven gratis, Turso gratis |
| 2. Gratis y permanente, sin regla de inactividad que yo haya encontrado | El proveedor describe el plan como sin límite de tiempo | Cloudflare D1, Azure SQL Database (oferta gratuita), Oracle HeatWave Always Free, DynamoDB |
| 3. De pago mínimo, sin pausas automáticas | Es la única que implica una relación comercial mientras se pague | Aiven Developer (5 USD al mes), Turso Developer (4.99 USD al mes), planes de pago de Supabase |

## 4. La promesa de "gratis para siempre" no es confiable

Casos reales de planes gratuitos que desaparecieron o cambiaron:

| Proveedor | Qué pasó |
|---|---|
| Heroku Postgres | Eliminó todos sus planes gratuitos en noviembre de 2022 |
| ElephantSQL | Cerró el servicio por completo, incluido su plan gratuito de 20 MB |
| PlanetScale | Retiró su plan gratuito (Hobby) el 8 de abril de 2024 |
| **CockroachDB** | **El 15 de septiembre de 2026 dejó de ofrecer su plan gratuito Basic (10 GiB) a nuevas implementaciones**; ahora los clientes nuevos reciben una prueba de 30 días con crédito |
| Render | Sus bases de datos PostgreSQL gratuitas se eliminan después de unos 30 días |
| Fly.io | Ya no ofrece plan gratuito a usuarios nuevos |
| AWS | En julio de 2025 cambió su nivel gratuito: las cuentas nuevas reciben un plan de 6 meses con créditos y la cuenta se cierra si no se pasa a un plan de pago |
| Supabase | Cambió su política de pausa: en 2024 las bases pausadas se podían restaurar solo 90 días, y en la documentación actual la ventana es de 1 año |
| Oracle (reportado) | Una fuente reporta que en junio de 2026 recortó a la mitad las máquinas gratuitas Ampere A1, sin anuncio público. No pude confirmarlo en la documentación oficial |

Esto muestra que las reglas cambian con los años (a veces a favor y a veces en contra) y que una base pensada para durar 20 años no puede depender de un solo proveedor.

## 5. Criterios de evaluación

| Criterio | Pregunta |
|---|---|
| Permanencia | ¿El plan gratuito expira o es permanente? |
| Inactividad | ¿Se pausa, se apaga, se archiva o se borra si no se usa? |
| Mantenimiento | ¿Tengo que entrar periódicamente para que no se pierda? |
| Capacidad | ¿Alcanza para una base de clase (decenas de MB)? |
| Conexión | ¿Puedo conectarme con un cliente estándar (psql, mysql…) o solo por una API? |
| Cuenta | ¿Necesita tarjeta o una suscripción? |

## 6. Candidatos analizados

**Comprobado** = confirmado en documentación oficial del proveedor. **Por verificar** = viene de comparativas o de fuentes secundarias y conviene revisarlo en la página oficial.

### 6.1 SQL administrado

| Proveedor | Motor | Qué ofrece gratis | ¿Qué pasa si no la uso? | Verificación |
|---|---|---|---|---|
| **Azure SQL Database (oferta gratuita)** | SQL Server | Hasta 10 bases por suscripción; cada una con 100,000 vCore-segundos de cómputo al mes, 32 GB de datos y 32 GB de respaldo, "por la vida de la suscripción" | Se pausa solo cuando se acaba la cuota mensual de cómputo y se reanuda el mes siguiente. No encontré una regla de borrado por inactividad. Requiere una suscripción de Azure activa y **no es compatible con "Azure for Students Starter"** | Comprobado |
| **Oracle Cloud: HeatWave Always Free** | MySQL | Una base por cuenta, en la región de origen: 50 GB de almacenamiento y 50 GB de respaldos, sin límite de tiempo | No encontré una regla de inactividad en la documentación consultada. Sin SLA y sin soporte oficial | Comprobado (inactividad: por verificar) |
| **Oracle Cloud: Autonomous Database Always Free** | Oracle Database | Hasta 2 instancias de unos 20 GB | Se **detiene sola a los 7 días sin conexiones** (conserva los datos) y puede **borrarse de forma permanente** si acumula 90 días detenida | Comprobado |
| **Neon** | PostgreSQL | 100 proyectos; por proyecto 1 GB de almacenamiento (20 GB en total) y 100 CU-hours de cómputo al mes. Antes eran 0.5 GB: el límite cambió | El cómputo se suspende a los 5 minutos de inactividad (no se puede desactivar en el plan gratis) pero los datos se conservan. Una fuente secundaria reporta que en tres regiones de Azure los proyectos gratuitos inactivos 90+ días pueden borrarse desde el 5 de octubre de 2026 | Comprobado (Azure: por verificar) |
| **Supabase (plan gratuito)** | PostgreSQL | Proyectos gratuitos con 500 MB de base de datos | Se **pausa si hay poca actividad durante 7 días**; se puede restaurar hasta 1 año después según la documentación actual. Los planes de pago no se pausan | Comprobado |
| **Aiven (plan gratuito)** | PostgreSQL / MySQL | 1 CPU, 1 GB de RAM y 1 GB de disco, sin tarjeta y sin límite de tiempo | Aiven puede **apagar el servicio por falta de uso** (avisa antes y se puede volver a encender) y se reserva el derecho de cambiar región o plan | Comprobado |
| **Aiven (plan Developer)** | PostgreSQL / MySQL | De pago, desde 5 USD al mes: 1 CPU, 1 GB de RAM y hasta 8 GB de disco | **No se apaga automáticamente** por inactividad | Comprobado |
| **CockroachDB (Basic)** | Compatible con PostgreSQL | Tuvo 10 GiB gratis, pero **ya no está disponible para clientes nuevos** desde el 15 de septiembre de 2026. En su documentación, los clústeres gratuitos se podían borrar tras 6 meses sin actividad | Descartada | Comprobado (cierre: por verificar) |

### 6.2 SQLite en la nube

| Proveedor | Motor | Qué ofrece gratis | ¿Qué pasa si no la uso? | Verificación |
|---|---|---|---|---|
| **Cloudflare D1** | SQLite | Plan gratuito con 5 GB de almacenamiento en total (máximo 500 MB por base), 5 millones de filas leídas al día y 100,000 escritas al día, sin tarjeta. Su documentación responde que el plan gratuito de Workers "siempre" incluirá la posibilidad de usar D1 gratis | No encontré una regla de inactividad. **Limitación:** solo se accede desde Cloudflare Workers o por API HTTP, no con un cliente SQL normal | Comprobado |
| **Turso** | SQLite (libSQL) | Plan gratuito con 100 bases, 5 GB en total, 500 millones de filas leídas y 10 millones escritas al mes, sin tarjeta | En el plan gratuito las bases se **archivan tras 10 días de inactividad** y hay que reactivarlas manualmente con la CLI o la API (los datos no se borran según la documentación). El plan Developer cuesta 4.99 USD al mes | Comprobado |

### 6.3 NoSQL

| Proveedor | Qué ofrece gratis | ¿Qué pasa si no la uso? | Verificación |
|---|---|---|---|
| **Amazon DynamoDB** | 25 GB de almacenamiento y 25 unidades de lectura y 25 de escritura al mes en el nivel "Always Free", que no expira a los 12 meses | Sin regla de inactividad encontrada. **Ojo:** las cuentas nuevas desde julio de 2025 empiezan en un "Free plan" que se cierra a los 6 meses; para conservar la cuenta hay que pasar al plan de pago (los 25 GB siguen siendo gratis dentro del límite) | Por verificar |
| **Google Cloud Firestore** | 1 GiB de datos y cuotas diarias de lecturas y escrituras | Sin regla de inactividad encontrada | Por verificar |
| **MongoDB Atlas (M0)** | Un clúster gratuito de 512 MB, sin fecha de expiración | Se **pausa tras un periodo de inactividad sin conexiones** (la documentación actual menciona 30 días) y se puede reanudar | Comprobado |

### 6.4 Montar mi propia base en una máquina virtual gratuita

| Proveedor | Qué ofrece gratis | ¿Qué pasa si no la uso? | Verificación |
|---|---|---|---|
| **Oracle Cloud: máquinas Ampere A1** | Máquinas virtuales gratuitas donde puedo instalar MySQL o PostgreSQL con control total | Oracle puede **reclamar las máquinas inactivas**: las considera inactivas si en 7 días el uso de CPU (percentil 95) es menor a 20%, y lo mismo para red y memoria. Es la opción que más trabajo requiere y la menos "para olvidarse" | Comprobado |

### 6.5 Ya no sirven para esta meta

Heroku Postgres, ElephantSQL, PlanetScale (plan Hobby), Render (BD gratuita), Fly.io y CockroachDB Basic para clientes nuevos (ver sección 4).

## 7. ¿Cuál sirve si no quiero estar entrando?

| Opción | ¿Hay que entrar para mantenerla viva? |
|---|---|
| Oracle Autonomous Database | **Sí**: menos de cada 7 días para evitar que se detenga, y siempre antes de los 90 días detenida |
| Supabase gratis | **Sí**: unas consultas por día durante la semana, o se pausa |
| Turso gratis | **Sí**: el archivado ocurre a los 10 días y hay que reactivarla a mano |
| MongoDB Atlas gratis | **Sí**: una conexión de vez en cuando |
| Aiven gratis | **Sí**: si se apaga por falta de uso, hay que volver a encenderla |
| Oracle Ampere A1 (base propia) | **Sí, en la práctica**: necesita mantener un uso mínimo para no ser reclamada |
| Cloudflare D1 | **No se encontró esa exigencia** |
| Azure SQL (oferta gratuita) | **No se encontró esa exigencia**, pero necesita una suscripción activa |
| Oracle HeatWave Always Free | **No se encontró esa exigencia**, conviene confirmarlo en la consola al crearla |
| DynamoDB | **No se encontró esa exigencia**, pero la cuenta de AWS debe mantenerse activa |
| Neon | **No para conservar los datos**: se suspende pero no se borra (salvo la excepción reportada en regiones Azure) |
| Aiven Developer / Turso Developer / Supabase de pago | **No**: a cambio de un pago mensual |

## 8. Hallazgos clave

- **"De por vida" es una promesa, no un contrato.** Hasta CockroachDB, un proveedor serio, cerró su plan gratuito para clientes nuevos hace menos de un mes.
- **La única que lo dice explícitamente es Cloudflare:** su documentación afirma que el plan gratuito siempre incluirá D1. Pero es SQLite y no se conecta con un cliente SQL normal, así que sirve sobre todo para aplicaciones alojadas en Cloudflare.
- **Las que más se acercan a "gratis, SQL y sin pausas"** son Azure SQL Database (SQL Server) y Oracle HeatWave (MySQL), porque en su documentación no aparece una regla de pausa o borrado por inactividad. Ninguna es una garantía.
- **Detalles que cambian la decisión:** Azure SQL no funciona con la cuenta "Azure for Students Starter"; DynamoDB exige pasar a un plan de pago después de 6 meses en cuentas nuevas; Oracle Autonomous Database sí se detiene y se puede borrar.
- **Lo más cercano a una garantía real** es un plan de pago mínimo (Aiven Developer o Turso Developer, alrededor de 5 USD al mes).
- **Nada de esto asegura 20 años.** Por eso lo más seguro es combinar un proveedor con un respaldo propio.

## 9. Estrategia para que dure décadas

### A. Un proveedor con la menor exigencia de mantenimiento

Usar la opción gratuita que menos requiere según el motor que necesite (Oracle HeatWave para MySQL, Azure SQL para SQL Server, Cloudflare D1 para SQLite) o, si se necesita más seguridad, un plan de pago mínimo.

### B. Un respaldo propio, independiente de cualquier empresa

Guardar un archivo de respaldo para poder reconstruir la base en otro proveedor si el actual cambia sus reglas o desaparece:

```bash
# MySQL
mysqldump -h HOST -u USUARIO -p mi_base > respaldo-$(date +%F).sql

# PostgreSQL
pg_dump "postgresql://usuario:clave@host/db" > respaldo-$(date +%F).sql
```

El respaldo se puede guardar en un repositorio privado junto con un `docker-compose.yml` para recrear la base con un comando en cualquier computadora.

### C. Revisar y migrar cada cierto tiempo

Aunque el proveedor siga existiendo, revisar una o dos veces al año que el plan no haya cambiado y que el respaldo siga restaurándose bien.

## 10. Tamaño de la base de datos

Una base de datos de clase (por ejemplo, una plataforma de música con solo metadatos: usuarios, artistas, canciones, playlists) ocupa normalmente entre **10 y 50 MB**. Los archivos pesados (audio, imágenes) no se guardan dentro de la base; solo se almacena su URL. Eso cabe en cualquiera de los planes gratuitos anteriores, incluso en el más pequeño (500 MB por base en Cloudflare D1).

## 11. Recomendación final

| Si mi base es… | Opción recomendada | Por qué |
|---|---|---|
| MySQL | **Oracle HeatWave Always Free** | Gratis, permanente según Oracle, 50 GB y sin regla de inactividad encontrada |
| SQL Server | **Azure SQL Database (oferta gratuita)** | Gratis "por la vida de la suscripción", 32 GB, sin borrado por inactividad encontrado |
| SQLite y se usará desde Cloudflare | **Cloudflare D1** | La única que declara que siempre habrá plan gratuito |
| Quiero la mayor seguridad posible | **Plan de pago mínimo** (Aiven Developer o Turso Developer) | Relación comercial y sin apagado automático |
| En todos los casos | **Respaldo propio** (`.sql` + Docker en GitHub) | Protege contra que el proveedor cambie sus reglas o desaparezca |



## 12. Conclusión

Con esta investigación aprendí que no existe un hosting de bases de datos que garantice por contrato que va a seguir funcionando 20 años, ni gratis ni de pago. Lo que sí encontré es que hay una diferencia grande entre los planes gratuitos: algunos, como Oracle Autonomous Database, Supabase, Turso o MongoDB Atlas, se detienen, se pausan o se archivan si no los uso, mientras que otros, como Azure SQL Database, Oracle HeatWave Always Free y Cloudflare D1, el proveedor los describe como permanentes y no encontré una regla de inactividad en su documentación. También me sorprendió ver cuántos servicios gratuitos desaparecieron o cambiaron, como Heroku, ElephantSQL, PlanetScale y hasta CockroachDB hace pocas semanas, y por eso entendí que no era una tarea fácil: hay que leer la letra chica de cada plan y no confiar solo en la promesa de "gratis para siempre". Si yo quisiera que mi base siguiera funcionando dentro de 10 o 20 años, elegiría una de las opciones permanentes según el motor que use (o un plan de pago mínimo si necesitara más seguridad) y además guardaría un respaldo propio en formato `.sql` para poder migrarla si el proveedor cambia sus reglas.

## 13. Fuentes

Documentación y páginas oficiales:

- Oracle, Autonomous Database Always Free (inactividad y borrado): https://docs.oracle.com/en/cloud/paas/autonomous-data-warehouse-cloud/user/autonomous-always-free.html
- Oracle, recursos Always Free y reclamación de máquinas inactivas: https://docs.oracle.com/en-us/iaas/Content/FreeTier/resourceref.htm
- Oracle, MySQL HeatWave, servicio Always Free: https://docs.oracle.com/en-us/iaas/mysql-database/doc/features-mysql-heatwave-service.html
- Oracle, página de HeatWave Always Free: https://www.oracle.com/jp/heatwave/free/
- Oracle, anuncio de HeatWave Always Free: https://blogs.oracle.com/mysql/introducing-heatwave-always-free
- Microsoft, oferta gratuita de Azure SQL Database: https://learn.microsoft.com/en-us/azure/azure-sql/database/free-offer
- Microsoft, anuncio de disponibilidad general de la oferta gratuita: https://techcommunity.microsoft.com/blog/azuresqlblog/-/4372418
- Cloudflare, precios de D1 (incluye la pregunta "¿D1 siempre tendrá plan gratuito?"): https://developers.cloudflare.com/d1/platform/pricing/
- Cloudflare, límites de D1: https://developers.cloudflare.com/d1/platform/limits
- Turso, scale to zero y archivado por inactividad: https://docs.turso.tech/features/scale-to-zero
- Turso, precios: https://turso.tech/pricing.md
- Supabase, pausa de proyectos gratuitos: https://supabase.com/docs/guides/platform/free-project-pausing
- Supabase, cambio de restauración a 90 días (2024): https://supabase.com/changelog/27497-paused-free-plan-projects-are-restorable-for-90-days
- Aiven, plan gratuito de PostgreSQL: https://aiven.io/docs/products/postgresql/concepts/pg-free-tier
- Aiven, plan gratuito de MySQL: https://aiven.io/docs/products/mysql/concepts/mysql-free-tier
- Aiven, plan Developer: https://aiven.io/developer-tier-pg-mysql
- Neon, límites del plan gratuito: https://neon.com/faqs/free-plan-limits-and-quotas
- MongoDB Atlas, límites de clústeres gratuitos: https://mongodb.com/docs/atlas/reference/free-shared-limitations/
- CockroachDB, administración de clústeres Basic (borrado tras 6 meses sin actividad): https://www.cockroachlabs.com/docs/cockroachcloud/basic-cluster-management
- AWS Builder Center, cómo funciona el nivel gratuito de AWS: https://builder.aws.com/content/3BlPcDGddaB6WkPvUf1LIy1rzIm/everything-you-need-to-know-to-get-started-with-aws-free-tier

Comparativas y notas consultadas (datos marcados como "Por verificar" y casos de planes desaparecidos):

- Cierre del plan gratuito de CockroachDB (15 de septiembre de 2026): https://github.com/robhunter/agentdeals/issues/1933
- Neon y regiones de Azure (borrado de proyectos inactivos): https://agentdeals.dev/vendor/neon
- Cambios del nivel gratuito de AWS en 2025 y 2026: https://github.com/robhunter/agentdeals/issues/1435
- Recorte reportado de las máquinas gratuitas de Oracle en 2026: https://braindetox.kr/en/posts/oracle_always_free_backend_2026.html
- Swyftstack, PostgreSQL gratis (Heroku, ElephantSQL, Render): https://swyftstack.com/blog/free-postgresql-hosting
- freetier.co, bases de datos gratuitas en 2026: https://freetier.co/articles/best-free-database-free-tiers-2026
- usage.ai, DynamoDB Always Free: https://usage.ai/blogs/aws/reserved-instances/dynamodb/free-tier/
- Cloudinsight, lista de bases de datos gratuitas en 2026: https://cloudinsight.cc/en/blog/free-cloud-database
- Level Up Coding, PlanetScale elimina su plan gratuito: https://levelup.gitconnected.com/why-planetscale-removed-their-free-tier-64dd2953e438
- DEV Community, Heroku elimina sus planes gratuitos: https://www.dev.to/harshhhdev/migrating-your-postgresql-database-from-heroku-to-cockroachdb-bj8
- free-for-dev, ElephantSQL descontinúa su plan gratuito: https://git.hackliberty.org/Git-Mirrors/free-for-dev/commit/136f5901c8bba696b2d8a8750022b408d65a4e29
- Render, plataformas con plan gratuito real en 2026: https://render.com/articles/platforms-with-a-real-free-tier-for-developers-in-2026.md