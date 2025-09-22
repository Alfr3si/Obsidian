---
id: guia-postgresql
aliases:
  - Guia Postgresql
tags:
  - guia
---

# Guia Postgresql

## Descripcion

Esta guia de **postgresql** es creada con el fin de **recordar** los procedimientos de:

- Instalacion
- Configuracion
- Uso

Para facilitar el manejo de este **Sistema Gestor de Base de Datos** en **_Linux_**. Con la posibilidad de acceder a esta
informacion cuando sea requerida.

## Instalacion

> [!important]
> La instalacion de **postgresql** puede variar dependiendo del sistema operativo  
> en este caso estoy usando Arch Linux asi que la instalacion y esta informacion  
> hace enfasis en este sistema operativo

Para esto necesitas instalar:

- postgresql

luego necesitas realizar los siguientes comandos

```bash
    # Inicializar base de datos inicial
    sudo -iu postgres initdb -D /var/lib/postgres/data

    # iniciar el servicio
    sudo systemctl start httpd.service

    # o habilitar al arranque
    sudo systemctl enable httpd.service

```

## Crear Usuario y Base de Datso

Por defecto porstgresql crea el usuario **postgres**. Entra con:

```bash
    sudo -iu postgres psql

```

Dentro de la consola (psql) crea un usuario y base de datos

```sql

-- Crear usuario con contraseña
CREATE USER miusuario WITH PASSWORD 'mipassword';

-- Crear base de datos y asignar dueño
CREATE DATABASE midb OWNER miusuario;

-- Darle privilegios
GRANT ALL PRIVILEGES ON DATABASE midb TO miusuario;

-- Salir de psql
\q

```

Probar conexion

```bash

    psql -U miusuario -d midb

```

Si ves el propmt 'miproyecto=>', funciona. ✅

> [!tip]
> Como extra recomendaria darle permisos al esquema public a tu usuario  
> que es donde se crean las tablas por defecto.

1. Conectate como usuario postgres en la consola

```bash
    sudo -iu postgres
    psql

```

2. Cambia a tu base de datos

```sql
   \c midb

```

3. Dale permisos a tu suario en el esquema public

```sql
    GRANT ALL PRIVILEGES ON SCHEMA public TO miusuario;

    -- sal de psql
    \q

```

> [!note]
> Para usar la interfaz de vimdadbod en neovim debes usar el comando  
> :DBUI con esto puedes abrir la interfaz de bases de datos  
> tambien :DBUIAddConnection para agregar una nueva conexion.  
> Alli puedes colocar la URL de conexion a Postgrepsql  
> postgresql://alfr3d:12345@localhost:5432/miproyecto
