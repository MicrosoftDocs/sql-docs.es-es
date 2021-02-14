---
title: Asignación de bases de datos de MySQL a esquemas de SQL Server (MySQLToSQL) | Microsoft Docs
description: Aprenda a personalizar SSMA para asignaciones de MySQL entre esquemas de MySQL y SQL Server o Azure SQL Database o acepte el valor predeterminado.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping, Modifying target database and schema
- Mapping, reverting to default database and schema
ms.assetid: 5c6fb445-92ae-4933-b77d-80230931c024
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 44ac691a0fe9cb4b513a2f9b435457c699c7e3e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076096"
---
# <a name="mapping-mysql-databases-to-sql-server-schemas-mysqltosql"></a>Asignación de bases de datos de MySQL a esquemas de SQL Server (MySQLToSQL)
De forma predeterminada, SSMA para MySQL migra todos los objetos de un esquema de MySQL a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database denominada para el esquema. Sin embargo, puede personalizar la asignación entre los esquemas de MySQL y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
## <a name="mysql-and-sql-server-or-sql-azure-schemas"></a>Esquemas de MySQL y SQL Server o SQL Azure  
El concepto de MySQL de un esquema se asigna al concepto de SQL Server de una base de datos y a uno de sus esquemas. SSMA hace referencia a la combinación de SQL Server de la base de datos y el esquema como un esquema.  
  
El concepto de MySQL de un esquema se asigna al concepto de SQL Server de una base de datos y a uno de sus esquemas. Por ejemplo, MySQL podría tener un esquema denominado **HR**. Una instancia de SQL Server podría tener una base de datos denominada **HR** y, dentro de esa base de datos, los esquemas. Un esquema es el esquema **DBO** (o el propietario de la base de datos). De forma predeterminada, el **HR** del esquema MySQL se asignará a la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de datos y el esquema **HR. DBO**. SSMA hace referencia a la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] combinación de base de datos y esquema como esquema.  
  
Puede modificar la asignación entre MySQL y los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquemas de Azure.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modificar la base de datos y el esquema de destino  
En SSMA, puede asignar un esquema de MySQL a cualquier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquema disponible o SQL Azure.  
  
**Para modificar la base de datos y el esquema**  
  
1.  En el explorador de metadatos de MySQL, seleccione **esquemas**.  
  
    La pestaña **asignación de esquema** también está disponible cuando se seleccionan esquemas individuales. La lista de la pestaña **asignación de esquema** está personalizada para el objeto seleccionado.  
  
2.  En el panel derecho, haga clic en la pestaña **asignación de esquema** .  
  
    Verá una lista de todos los esquemas de MySQL, seguidos de un valor de destino. Este destino se denota en una notación de dos partes (*Database. Schema*) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure donde se migrarán los objetos y los datos.  
  
3.  Seleccione la fila que contiene la asignación que desea cambiar y, a continuación, haga clic en **modificar**.  
  
    En el cuadro de diálogo **elegir esquema de destino** , puede buscar el esquema y la base de datos de destino disponibles o escribir la base de datos y el nombre de esquema en el cuadro de texto en una notación de dos partes (Database. Schema) y, a continuación, hacer clic en **Aceptar**.  
  
4.  El destino cambia en la pestaña **asignación de esquema** .  
  
**Modos de asignación**  
  
-   Asignar a SQL Server  
  
Puede asignar una base de datos de origen a cualquier base de datos de destino. De forma predeterminada, la base de datos de origen se asigna a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de datos de destino con la que se ha conectado mediante SSMA. Si la base de datos de destino que se está asignando no existe en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se le mostrará un mensaje que indica que la base de datos o **el esquema no existen en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatos de destino. Se creará durante la sincronización. ¿Desea continuar? "** Haga clic en Sí. Del mismo modo, puede asignar un esquema a un esquema no existente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de datos de destino que se creará durante la sincronización.  
  
-   Asignar a SQL Azure  
  
Puede asignar la base de datos de origen a la base de datos de destino conectada [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a cualquier esquema de la base de datos de destino conectada [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si asigna un esquema de origen a cualquier esquema no existente en la base de datos de destino conectada, se le pedirá un mensaje que indica que el **esquema no existe en los metadatos de destino. Se creará durante la sincronización. ¿Desea continuar? "** Haga clic en sí.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Revertir a la base de datos y el esquema predeterminados  
Si personaliza la asignación entre un esquema de MySQL y un esquema de SQL Server, puede revertir la asignación a los valores predeterminados.  
  
**Para revertir a la base de datos y el esquema predeterminados**  
  
1.  En la pestaña asignación de esquema, seleccione cualquier fila y haga clic en **Restablecer valores predeterminados** para revertir a la base de datos y el esquema predeterminados.  
  
## <a name="next-steps"></a>Pasos siguientes  
Si desea analizar la conversión de objetos de MySQL en SQL Server o SQL Azure objetos, puede [crear un informe de conversión](assessing-mysql-databases-for-conversion-mysqltosql.md) ; de lo contrario, puede [convertir las definiciones de objetos de base de datos de MySQL](converting-mysql-databases-mysqltosql.md) en esquemas de SQL Server o SQL Azure  
  
## <a name="see-also"></a>Consulte también  
[Configuración del proyecto &#40;conversión&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
[Conexión a Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[Conexión a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
