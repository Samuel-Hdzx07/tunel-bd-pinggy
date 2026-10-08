# ☁️ Actividad 2: Hosting de bases de datos a largo plazo

## 1. Objetivo

Encontrar un servicio donde pueda subir mi base de datos y que siga funcionando durante muchos años (meta: 10 a 20 o más), sin depender de mi computadora.

## 2. Hallazgo principal

**Ningún proveedor gratuito garantiza por contrato 20 años de servicio.** Los planes gratuitos cambian, se reducen o desaparecen. Ejemplos:

- Fly.io ya no ofrece plan gratuito a usuarios nuevos.
- Render elimina sus bases de datos PostgreSQL gratuitas a los 30 días aproximadamente.
- Supabase pausa los proyectos gratuitos inactivos.

Por eso la estrategia realista es: **proveedor con plan permanente + respaldo propio portable**.

## 3. Criterios de evaluación

| Criterio | Pregunta |
|---|---|
| Permanencia | ¿El plan es permanente o es una prueba que expira? |
| Inactividad | ¿Pausa o borra la BD si no se usa? |
| Capacidad | ¿Almacenamiento suficiente? |
| Portabilidad | ¿Puedo exportar los datos fácilmente? |
| Motor | ¿MySQL, PostgreSQL, MongoDB…? |
| Tarjeta | ¿Pide tarjeta de crédito? |

## 4. Candidatos analizados

> ⚠️ Los planes gratuitos cambian. Verifica cada dato en la página oficial y anota la fecha de consulta: _____

| Proveedor | Motor | Plan gratuito (según fuentes consultadas) | Riesgo | Duración esperada |
|---|---|---|---|---|
| **Oracle Cloud (OCI) Always Free** | MySQL HeatWave / Autonomous Database | Recursos gratuitos "de por vida de la cuenta" según Oracle: MySQL HeatWave con 50 GB de datos + 50 GB de respaldos, y 2 instancias de Autonomous Database de 20 GB | Son límites, no ilimitado; depende de la región; cuentas inactivas 30+ días pueden suspenderse; sin SLA | **Larga (mejor opción encontrada)** |
| **Neon** | PostgreSQL | Plan gratuito con "scale to zero" y cuotas mensuales | Los límites pueden cambiar | Media-larga |
| **Supabase** | PostgreSQL | 2 proyectos, 500 MB por proyecto | Pausa proyectos inactivos | Media |
| **Aiven** | PostgreSQL / MySQL / Valkey | Plan gratuito de un nodo pequeño (las fuentes difieren en el disco: verificar) | Puede apagarse sin actividad | Media |
| **Couchbase Capella** | NoSQL | "Forever free", 1 nodo, hasta 8 GB | Motor no SQL tradicional | Media-larga |
| **Turso** | SQLite (libSQL) | Muchas bases pequeñas | Límite de tamaño por BD | Larga |
| **filess.io** | MySQL, MariaDB, PostgreSQL, MongoDB | 2 BD de hasta 10 MB | Capacidad mínima | Incierta |
| **Render (BD gratuita)** | PostgreSQL | Se elimina a los 30 días | ❌ No sirve | Corta |
| **Fly.io** | — | Sin plan gratuito para nuevos usuarios | ❌ Descartado | — |

_Fuentes: comparativas públicas de planes gratuitos (Render, Koyeb, flaviocopes.com, swyftstack.com, agentdeals.dev, pandastack.io), la documentación de Oracle y las páginas de cada proveedor, consultadas en octubre de 2026._

## 5. Estrategia para que dure décadas

### A. Plan permanente + respaldo

Usar Oracle Cloud Always Free (u otra alternativa) y entrar periódicamente para que la cuenta no se considere abandonada. Además, guardar respaldos:

```bash
# MySQL
mysqldump -h HOST -u USUARIO -p mi_base > respaldo-$(date +%F).sql

# PostgreSQL
pg_dump "postgresql://usuario:clave@host/db" > respaldo-$(date +%F).sql
```

### B. Respaldo independiente de cualquier empresa

Guardar un `dump.sql` y un `docker-compose.yml` en un repositorio: la BD se reconstruye en cualquier máquina o proveedor con un comando. SQLite también es una opción, porque toda la base es un solo archivo.

### C. Pagar poco

Un VPS barato o plan pagado mínimo es la única opción con compromiso comercial real.

## 6. Recomendación final

| Prioridad | Opción |
|---|---|
| 🥇 | **Oracle Cloud Always Free** (MySQL HeatWave o Autonomous Database) |
| 🥈 | Neon, Aiven, Couchbase Capella o Turso, según el motor |
| 🥉 | `dump.sql` + Docker en GitHub como respaldo |
| Con garantía | VPS o plan pagado mínimo |

### Tamaño estimado

Una BD de clase (por ejemplo tipo plataforma de música, solo con metadatos) pesa entre **10 y 50 MB**, muy por debajo de los límites de cualquier plan gratuito. Los archivos pesados (audio, imágenes) se guardan fuera de la BD y solo se almacena su URL.

## 7. Evidencia de la prueba

- Proveedor elegido: _____
- Fecha de creación: _____
- Captura del panel con la BD creada: `img/01-hosting-panel.png`
- Captura de conexión exitosa: `img/02-hosting-conexion.png`

## 8. Conclusión

_(Escribe tu conclusión: qué proveedor elegirías y por qué.)_
