# 🔌 Actividad 1: Túnel a una base de datos con Pinggy

## 1. Objetivo

Crear un **túnel TCP público** con [Pinggy](https://pinggy.io) hacia una base de datos que corre en mi computadora, para que el profesor pueda conectarse con la dirección generada desde cualquier lugar, sin que yo tenga que abrir puertos en el router ni tener IP pública.

> La base de datos usada es **solo de prueba** (una tabla simple). El objetivo es demostrar el túnel, no el diseño de la BD.

## 2. Cómo funciona

```
 Profesor (cualquier lugar)
        │  mysql -h <host_pinggy> -P <puerto_publico> -u profe -p
        ▼
 ┌───────────────────┐
 │  Servidor Pinggy  │   ← dirección pública (host + puerto)
 └─────────┬─────────┘
           │  túnel SSH (lo inicio yo desde mi PC hacia afuera)
           ▼
 ┌───────────────────┐
 │  Mi computadora   │
 │  BD en localhost  │   ← puerto local (3306 MySQL / 5432 PostgreSQL)
 └───────────────────┘
```

## 3. Herramientas

| Herramienta | Versión | Uso |
|---|---|---|
| Sistema operativo | _Windows 11_ | |
| Motor de BD | _MySQL / PostgreSQL (versión)_ | BD de prueba |
| Pinggy | — | Túnel público |
| OpenSSH | _(versión)_ | Pinggy funciona sobre SSH |
| Cliente de BD | _DBeaver / Workbench / consola_ | Probar la conexión |

## 4. Procedimiento

### Paso 1. Crear una BD de prueba

```sql
CREATE DATABASE prueba_tunel;
USE prueba_tunel;

CREATE TABLE saludos (
  id INT PRIMARY KEY AUTO_INCREMENT,
  mensaje VARCHAR(100)
);

INSERT INTO saludos (mensaje) VALUES
  ('Hola profe, si ves esto el tunel funciona'),
  ('Conectado desde otra red');
```

📸 `img/01-bd-local.png`

### Paso 2. Crear un usuario de solo lectura para el profesor

No se comparte el usuario administrador.

```sql
-- MySQL
CREATE USER 'profe'@'%' IDENTIFIED BY 'CLAVE_TEMPORAL_FUERTE';
GRANT SELECT ON prueba_tunel.* TO 'profe'@'%';
FLUSH PRIVILEGES;
```

```sql
-- PostgreSQL
CREATE USER profe WITH PASSWORD 'CLAVE_TEMPORAL_FUERTE';
GRANT CONNECT ON DATABASE prueba_tunel TO profe;
GRANT USAGE ON SCHEMA public TO profe;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO profe;
```

📸 `img/02-usuario-profe.png`

### Paso 3. Abrir el túnel TCP

```bash
# MySQL
ssh -p 443 -R0:localhost:3306 tcp@a.pinggy.io

# PostgreSQL
ssh -p 443 -R0:localhost:5432 tcp@a.pinggy.io
```

- `-p 443`: se conecta a Pinggy por el puerto 443.
- `-R0:localhost:3306`: reenvía lo que llegue al puerto público hacia el puerto local de la BD.
- `tcp@a.pinggy.io`: pide un túnel de tipo **TCP** (el HTTP no sirve para bases de datos).

Pinggy imprime una dirección parecida a:

```
tcp://xxxxx.a.free.pinggy.link:NNNNN
```

- **Host:** `xxxxx.a.free.pinggy.link`
- **Puerto:** `NNNNN`

📸 `img/03-pinggy-url.png`

> ⚠️ El túnel solo vive mientras la terminal esté abierta, y en el plan gratuito tiene duración limitada y la dirección cambia al reiniciarlo (verificar límites actuales en la web de Pinggy).

### Paso 4. Probar desde otra red

Se probó usando los datos móviles del celular para confirmar que es accesible desde fuera de mi red.

```bash
# MySQL
mysql -h xxxxx.a.free.pinggy.link -P NNNNN -u profe -p

# PostgreSQL
psql -h xxxxx.a.free.pinggy.link -p NNNNN -U profe -d prueba_tunel
```

```sql
SELECT * FROM saludos;
```

📸 `img/04-conexion-remota.png`
📸 `img/05-consulta-remota.png`

### Paso 5. Entregar los datos al profesor

Se enviaron **por privado** (no en este repositorio):

| Dato | Valor |
|---|---|
| Host | _(por privado)_ |
| Puerto | _(por privado)_ |
| Usuario | `profe` |
| Contraseña | _(por privado)_ |
| Base de datos | `prueba_tunel` |

## 5. Problemas y soluciones

| Problema | Causa | Solución |
|---|---|---|
| _(ej. Connection refused)_ | _Túnel cerrado o BD apagada_ | _Reabrir túnel / iniciar servicio_ |
| _(agrega los tuyos)_ | | |

## 6. Seguridad

- ✅ Usuario de solo lectura, no administrador.
- ✅ Contraseña temporal; se elimina el usuario al terminar la revisión.
- ✅ Túnel cerrado cuando no se usa (`Ctrl + C`).
- ❌ No se suben contraseñas, hosts reales ni archivos `.env` al repositorio.

## 7. Conclusión

_(Escribe qué aprendiste y qué limitaciones notaste: el túnel es temporal y depende de que mi PC esté encendida.)_
