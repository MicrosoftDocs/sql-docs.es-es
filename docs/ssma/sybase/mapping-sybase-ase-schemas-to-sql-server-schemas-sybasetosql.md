---
description: Asignación de esquemas de Sybase ASE a esquemas de SQL Server (SybaseToSQL)
title: Asignación de esquemas de Sybase ASE a esquemas de SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Schema Mapping
ms.assetid: 2c927003-c49d-4fe1-8e3e-5b2899166268
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a0084551b2e1ab6de4c3731f33525ccd3a8a48d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017543"
---
# <a name="mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql"></a>Asignación de esquemas de Sybase ASE a esquemas de SQL Server (SybaseToSQL)
En Sybase Adaptive Server Enterprise (ASE), cada base de datos tiene uno o más esquemas. De forma predeterminada, SSMA migra todos los objetos de una base de datos y un esquema a la misma base de datos y esquema en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Sin embargo, puede personalizar la asignación entre ASE y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
## <a name="ase-and-sql-server-or-sql-azure-schemas"></a>Esquemas de ASE y SQL Server o SQL Azure  
ASE y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] or SQL Azure especifican las bases de datos y sus esquemas mediante la notación de dos partes como *Database. Schema*. Por ejemplo, en una base de datos de **demostración** de ase, puede haber un esquema **DBO** . La base de datos y el par de esquema se especifican como **Demo. DBO**. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure tienen la misma base de datos y el mismo esquema, el par también se especifica como **Demo. DBO**.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modificar la base de datos y el esquema de destino  
En SSMA, puede asignar un esquema ASE a cualquier esquema disponible [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
**Para modificar la base de datos y el esquema**  
  
1.  En el explorador de metadatos de Sybase, seleccione **bases** de datos.  
  
    La pestaña **asignación de esquema** también está disponible cuando se selecciona una base de datos individual, la carpeta **esquemas** o los esquemas individuales. La lista de la pestaña **asignación de esquema** está personalizada para el objeto seleccionado.  
  
2.  En el panel derecho, haga clic en la pestaña **asignación de esquema** .  
  
    Verá una lista de todas las bases de datos de ASE con sus esquemas, seguidos de un valor de destino. Este destino se denota en una notación de dos partes (*Database. Schema*) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure donde se migrarán los objetos y los datos.  
  
3.  Seleccione la fila que contiene la asignación que desea cambiar y, a continuación, haga clic en **modificar**.  
  
4.  En el cuadro de diálogo **elegir esquema de destino** , puede buscar el esquema y la base de datos de destino disponibles o escribir la base de datos y el nombre de esquema en el cuadro de texto en una notación de dos partes (Database. Schema) y, a continuación, hacer clic en **Aceptar**.  
  
5.  El destino cambia en la pestaña **asignación de esquema** .  
  
**Modos de asignación**  
  
-   Asignar a SQL Server  
  
Puede asignar una base de datos de origen a cualquier base de datos de destino. De forma predeterminada, la base de datos de origen se asigna a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de datos de destino con la que se ha conectado mediante SSMA. Si la base de datos de destino que se está asignando no existe en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se le mostrará un mensaje que indica que la base de datos o **el esquema no existen en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatos de destino. Se creará durante la sincronización. ¿Desea continuar? "** Haga clic en Sí. Del mismo modo, puede asignar un esquema a un esquema no existente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de datos de destino que se creará durante la sincronización.  
  
-   Asignar a SQL Azure  
  
Puede asignar la base de datos de origen al destino conectado Azure SQL Database o a cualquier esquema en el Azure SQL Database de destino conectado. Si asigna un esquema de origen a cualquier esquema no existente en la base de datos de destino conectada, se le pedirá un mensaje que indica que el **esquema no existe en los metadatos de destino. Se creará durante la sincronización. ¿Desea continuar? "** Haga clic en sí.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Revertir a la base de datos y el esquema predeterminados  
Si personaliza la asignación entre un esquema de ASE y un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure esquema, puede revertir la asignación a los valores predeterminados.  
  
**Para revertir a la base de datos y el esquema predeterminados**  
  
1.  En la pestaña asignación de esquema, seleccione cualquier fila y haga clic en **Restablecer valores predeterminados** para revertir a la base de datos y el esquema predeterminados.  
  
## <a name="next-steps"></a>Pasos siguientes  
Si desea analizar la conversión de objetos de Sybase ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos o SQL Azure, puede [crear un informe de conversión](assessing-sybase-ase-database-objects-for-conversion-sybasetosql.md). De lo contrario, puede [convertir las definiciones de objetos de base de datos ase](converting-sybase-ase-database-objects-sybasetosql.md) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure definiciones de objeto.  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de Sybase ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
