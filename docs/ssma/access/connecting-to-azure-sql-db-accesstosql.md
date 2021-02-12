---
title: Conexión a Azure SQL Database (AccessToSQL) | Microsoft Docs
description: Obtenga información sobre cómo conectarse a una instancia de destino de Azure SQL Database para migrar bases de datos de Access. SSMA obtiene metadatos acerca de las bases de datos en Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- instance of SQL Azure
- metadata, refreshing
- refreshing metadata
- SQL Azure
- SQL Azure, connecting
- SQL Azure, connecting to
- SQL Azure, reconnecting
- SQL Azure, synchronizing metadata
ms.assetid: 1ba0d113-dc05-4431-8689-e14a8821bafd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4f31b2e46312e89830d977ca0ce61ad07572c875
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076276"
---
# <a name="connecting-to-azure-sql-database-accesstosql"></a>Conexión a Azure SQL Database (AccessToSQL)

Para migrar bases de datos de Access a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , debe conectarse a la instancia de de destino de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Cuando se conecta, SSMA obtiene los metadatos de todas las bases de datos de la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] y muestra los metadatos de la base de datos en el **Explorador de metadatos de Azure SQL Database**. SSMA almacena información acerca de la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] a la que está conectado, pero no almacena contraseñas.

La conexión a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si desea una conexión activa con el servidor. Puede trabajar sin conexión hasta que cargue los objetos [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] de base de datos en y migre los datos.

Los metadatos de la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] no se sincronizan automáticamente. En su lugar, para actualizar los metadatos en **Azure SQL Database explorador de metadatos**, debe actualizar manualmente los [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] metadatos. Para obtener más información, vea la sección "sincronizar metadatos de Azure SQL Database" más adelante en este tema.

## <a name="required-azure-sql-database-permissions"></a>Permisos Azure SQL Database necesarios

La cuenta que se usa para conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] requiere permisos diferentes en función de las acciones que realiza la cuenta:

- Para convertir objetos de Access en [!INCLUDE[tsql](../../includes/tsql-md.md)] sintaxis, actualizar metadatos de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] o guardar la sintaxis convertida en scripts de, la cuenta debe tener permiso para iniciar sesión en la instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- Para cargar los objetos de base de datos en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , la cuenta debe ser miembro del rol de base de datos **db_ddladmin** .

- Para migrar datos a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , la cuenta debe ser miembro del rol de base de datos **db_owner** .

## <a name="establishing-an-azure-sql-database-connection"></a>Establecimiento de una conexión Azure SQL Database

Antes de convertir los objetos de base [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] de datos de Access a la sintaxis de, debe establecer una conexión con la instancia de en la [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] que desea migrar la base de datos o las bases de datos de Access.

Al definir las propiedades de conexión, también se especifica la base de datos en la que se migrarán los objetos y los datos. Puede personalizar esta asignación en el nivel de esquema de acceso después de conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Para obtener más información, vea [asignación de bases de datos de Access a esquemas de SQL Server](mapping-source-and-target-databases-accesstosql.md).
  
> [!IMPORTANT]
> Antes de intentar conectarse a, asegúrese de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] que se permite la dirección IP a través del [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] firewall.
  
Para conectarse al [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]:

1. En el menú **archivo** , seleccione **conectarse a SQL Azure** (esta opción se habilita después de la creación de un proyecto).
   Si anteriormente se conectó a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , el nombre del comando se volverá **a conectar a SQL Azure**.

2. En el cuadro de diálogo conexión, escriba o seleccione el nombre del servidor [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Escriba, seleccione o **examine** el nombre de la base de datos.

4. Escriba o seleccione el **nombre de usuario**.

5. Escriba la **contraseña**.

6. SSMA recomienda conexión cifrada a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Haga clic en **Conectar**.
  
Si no hay ninguna base de datos en el [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , puede crear la primera base de datos mediante la opción **crear base de datos de Azure** que aparece en el botón hacer clic en **examinar** .

## <a name="synchronizing-azure-sql-database-metadata"></a>Sincronizar metadatos de Azure SQL Database

Los metadatos acerca de las bases de datos de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] no se actualizan automáticamente. Los metadatos de **Azure SQL Database explorador de metadatos** es una instantánea de los metadatos al conectarse por primera vez a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] o la última vez que se actualizaron los metadatos manualmente. Puede actualizar manualmente los metadatos de todas las bases de datos o de cualquier base de datos o objeto de base de datos. Para sincronizar los metadatos:

1. Asegúrese de que está conectado a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. En **Azure SQL Database explorador de metadatos**, active la casilla situada junto a la base de datos o el esquema de la base de datos que desea actualizar.
   Por ejemplo, para actualizar los metadatos de todas las bases de datos, active la casilla situada junto a **bases** de datos.

3. Haga clic con el botón derecho en **bases** de datos o en el esquema de base de datos o de base de datos individual y seleccione **sincronizar con base de datos**.

## <a name="refreshing-azure-sql-database-metadata"></a>Actualización de metadatos de Azure SQL Database

Si [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] los esquemas cambian después de conectarse, puede actualizar los metadatos desde el servidor.

Para actualizar los [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] metadatos:

- En **Azure SQL Database explorador de metadatos**, haga clic con el botón derecho en **bases** de datos y seleccione **actualizar desde base de** datos.

## <a name="reconnecting-to-azure-sql-database"></a>Volver a conectar a Azure SQL Database

La conexión a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si desea una conexión activa con el servidor. Puede trabajar sin conexión hasta que cargue los objetos [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] de base de datos en y migre los datos.

El procedimiento para volver a conectar a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] es el mismo que el procedimiento para establecer una conexión.

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso de la migración depende de las necesidades del proyecto:

- Para personalizar la asignación entre los esquemas de acceso y Azure SQL Database, vea [asignar bases de datos de Access a esquemas de SQL Server](mapping-source-and-target-databases-accesstosql.md).
- Para personalizar las opciones de configuración de los proyectos, consulte [configuración de las opciones del proyecto](setting-conversion-and-migration-options-accesstosql.md).
- Para personalizar la asignación de los tipos de datos de origen y de destino, vea [asignar tipos de datos de origen y de destino](mapping-source-and-target-data-types-accesstosql.md).
- Si no tiene que realizar ninguna de estas tareas, puede convertir las definiciones de objetos de base de datos de Access en SQL Azure definiciones de objeto. Para obtener más información, vea [convertir bases](converting-access-database-objects-accesstosql.md)de datos de Access.

## <a name="see-also"></a>Consulte también

[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
