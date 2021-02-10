---
description: Conexión a Azure SQL Database (MySQLToSQL)
title: Conexión a Azure SQL Database (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to Azure SQL Database, SQL Azure permissions
- Connecting to Azure SQL Database, synchronization
ms.assetid: d0b6f16a-1880-459d-a0c7-28b7ef15c56a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: ad03f4f1dda77ddae21138af8581dfba4c8f8bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068963"
---
# <a name="connecting-to-azure-sql-database-mysqltosql"></a>Conexión a Azure SQL Database (MySQLToSQL)

Para migrar las bases de datos de MySQL a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , debe conectarse a la instancia de destino de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Cuando se conecta, SSMA obtiene los metadatos de todas las bases de datos de la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] y muestra los metadatos de la base de datos en el **Explorador de metadatos de Azure SQL Database**. SSMA almacena información de la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] a la que está conectado, pero no almacena contraseñas.

La conexión a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si desea una conexión activa con el servidor. Puede trabajar sin conexión hasta que cargue los objetos [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] de base de datos en y migre los datos.

Los metadatos de la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] no se sincronizan automáticamente. En su lugar, para actualizar los metadatos en **Azure SQL Database explorador de metadatos**, debe actualizar manualmente los [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] metadatos. Para obtener más información, vea la sección "sincronizar metadatos de Azure SQL Database" más adelante en este tema.

## <a name="required-azure-sql-database-permissions"></a>Permisos Azure SQL Database necesarios

La cuenta que se usa para conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] requiere permisos diferentes en función de las acciones que realiza la cuenta:

- Para convertir objetos MySQL en [!INCLUDE[tsql](../../includes/tsql-md.md)] sintaxis, actualizar metadatos de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] o guardar la sintaxis convertida en scripts de, la cuenta debe tener permiso para iniciar sesión en la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- Para cargar los objetos de base de datos en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , la cuenta debe ser miembro del rol de base de datos **db_ddladmin** .

- Para migrar datos a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , la cuenta debe ser miembro del rol de base de datos **db_owner** .

## <a name="establishing-an-azure-sql-database-connection"></a>Establecimiento de una conexión Azure SQL Database

Antes de convertir los objetos de base [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] de datos de MySQL en sintaxis, debe establecer una conexión con la instancia de en la [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] que desea migrar la base de datos o las bases de datos de MySQL.

Al definir las propiedades de conexión, también se especifica la base de datos en la que se migrarán los objetos y los datos. Puede personalizar esta asignación en el nivel de esquema de MySQL después de conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Para obtener más información, consulte [asignación de bases de datos de MySQL a esquemas de SQL Server &#40;&#41;MySQLToSQL ](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).

> [!IMPORTANT]
> Antes de intentar conectarse a, asegúrese de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] que se permite la dirección IP a través del [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] firewall.

Para conectarse al [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]:

1. En el menú **archivo** , seleccione **conectarse a Azure SQL Database** (esta opción se habilita después de la creación de un proyecto).
   Si previamente se ha conectado a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , el nombre del comando se volverá **a conectar a Azure SQL Database**.

2. En el cuadro de diálogo conexión, escriba o seleccione el nombre del servidor [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Escriba, seleccione o **examine** el nombre de la base de datos.

4. Escriba o seleccione el **nombre de usuario**.

5. Escriba la **contraseña**.

6. SSMA recomienda conexión cifrada a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Haga clic en **Conectar**.
  
## <a name="synchronizing-azure-sql-database-metadata"></a>Sincronizar metadatos de Azure SQL Database

Los metadatos acerca de las bases de datos de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] no se actualizan automáticamente. Los metadatos de **Azure SQL Database explorador de metadatos** es una instantánea de los metadatos al conectarse por primera vez a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] o la última vez que se actualizaron los metadatos manualmente. Puede actualizar manualmente los metadatos de todas las bases de datos o de cualquier base de datos o objeto de base de datos. Para sincronizar los metadatos:

1. Asegúrese de que está conectado a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. En **Azure SQL Database explorador de metadatos**, active la casilla situada junto a la base de datos o el esquema de la base de datos que desea actualizar.
   Por ejemplo, para actualizar los metadatos de todas las bases de datos, active la casilla situada junto a **bases** de datos.

3. Haga clic con el botón derecho en **bases** de datos o en el esquema de base de datos o de base de datos individual y seleccione **sincronizar con base de datos**.

## <a name="next-step"></a>siguiente paso

El siguiente paso de la migración depende de las necesidades del proyecto:

- Para personalizar la asignación entre los esquemas de MySQL y [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , consulte [asignación de bases de datos de MySQL a esquemas de SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).
- Para personalizar las opciones de configuración de los proyectos, vea [establecer opciones de proyecto &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).
- Para personalizar la asignación de los tipos de datos de origen y de destino, consulte [asignación de tipos de datos de MySQL y SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).
- Si no tiene que realizar ninguna de estas tareas, puede convertir las definiciones de objetos de base de datos de MySQL en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] definiciones de objeto. Para obtener más información, vea [convertir las bases de datos MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md).

## <a name="see-also"></a>Consulte también

[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)
