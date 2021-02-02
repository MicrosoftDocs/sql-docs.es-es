---
description: Cambiar el nombre de una base de datos
title: Cambio de nombre de una base de datos | Microsoft Docs
ms.custom: ''
ms.date: 10/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], renaming
- renaming databases
ms.assetid: 44c69d35-abcb-4da3-9370-5e0bc9a28496
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e22b6ae7e10347933b55ad643f6c69c6370340cf
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813006"
---
# <a name="rename-a-database"></a>Cambiar el nombre de una base de datos

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo cambiar el nombre de una base de datos definida por el usuario en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] o Azure SQL Database mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. El nombre de la base de datos puede incluir cualquier carácter que se ajuste a las reglas para identificadores.  
  
## <a name="in-this-topic"></a>En este tema
  
- Antes de empezar:  
  
     [Limitaciones y restricciones](#limitations-and-restrictions)  
  
     [Seguridad](#security)  
  
- Para cambiar el nombre de una base de datos, use:  
  
     [SQL Server Management Studio](#rename-a-database-using-sql-server-management-studio)  
  
     [Transact-SQL](#rename-a-database-using-transact-sql)  
  
- **Follow Up:**  [After renaming a database](#backup-after-renaming-a-database)  

> [!NOTE]
> Para cambiar el nombre de una base de datos en Azure Synapse Analytics o en Almacenamiento de datos paralelos, utilice la instrucción [RENAME (Transact-SQL)](../../t-sql/statements/rename-transact-sql.md).
  
## <a name="before-you-begin"></a>Antes de empezar
  
### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
  
- No se puede cambiar el nombre de las bases de datos del sistema.
- No se puede cambiar el nombre de la base de datos mientras otros usuarios estén accediendo a la base de datos. 
  - En SQL Server, puede establecer una base de datos en modo de usuario único para cerrar cualquier conexión abierta. Para más información, vea [Establecer una base de datos en modo de usuario único](../../relational-databases/databases/set-a-database-to-single-user-mode.md).
  - En Azure SQL Database, debe asegurarse de que ningún otro usuario tiene una conexión abierta a la base de datos cuyo nombre se va a cambiar.
  
### <a name="security"></a>Seguridad  
  
#### <a name="permissions"></a>Permisos

Requiere el permiso ALTER en la base de datos.  
  
## <a name="rename-a-database-using-sql-server-management-studio"></a>Cambiar el nombre de una base de datos con SQL Server Management Studio

Siga estos pasos para cambiar el nombre de una base de datos de SQL Server o Azure SQL Database mediante SQL Server Management Studio.

  
1. En el **Explorador de objetos**, conéctese a la instancia de SQL.  
  
2. Asegúrese de que no hay ninguna conexión abierta a la base de datos. Si usa SQL Server, puede [establecer la base de datos en el modo de usuario único](../../relational-databases/databases/set-a-database-to-single-user-mode.md) para cerrar todas las conexiones abiertas y evitar que otros usuarios se conecten mientras está cambiando el nombre de la base de datos.  
  
3. En el Explorador de objetos, expanda **Bases de datos**, haga clic con el botón derecho en la base de datos cuyo nombre quiere cambiar y luego haga clic en **Cambiar nombre**.  
  
4. Escriba el nuevo nombre de la base de datos y haga clic en **Aceptar**.  
  
5. Opcionalmente, si la base de datos era la predeterminada, vea [Restablecimiento de la base de datos predeterminada después del cambio de nombre](#reset-your-default-database-after-rename).

## <a name="rename-a-database-using-transact-sql"></a>Cambiar el nombre de una base de datos mediante Transact-SQL  
  
### <a name="to-rename-a-sql-server-database-by-placing-it-in-single-user-mode"></a>Para cambiar el nombre de una base de datos de SQL Server cambiándola al modo de usuario único

Siga estos pasos para cambiar el nombre de una base de datos de SQL Server mediante T-SQL en SQL Server Management Studio, incluidos los pasos para establecer la base de datos en modo de usuario único y, después de cambiar el nombre, volver a establecer la base de datos en modo multiusuario.
  
1. Conéctese a la base de datos `master` de la instancia.  
2. Abra una ventana de consulta.  
3. Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo cambia el nombre de la base de datos `MyTestDatabase` a `MyTestDatabaseCopy`.
  
   ```sql
   USE master;  
   GO  
   ALTER DATABASE MyTestDatabase SET SINGLE_USER WITH ROLLBACK IMMEDIATE
   GO
   ALTER DATABASE MyTestDatabase MODIFY NAME = MyTestDatabaseCopy ;
   GO  
   ALTER DATABASE MyTestDatabaseCopy SET MULTI_USER
   GO
   ```  

4. Opcionalmente, si la base de datos era la predeterminada, vea [Restablecimiento de la base de datos predeterminada después del cambio de nombre](#reset-your-default-database-after-rename).

### <a name="to-rename-an-azure-sql-database-database"></a>Para cambiar el nombre de una base de datos de Azure SQL Database

Siga estos pasos para cambiar el nombre de una base de datos de Azure SQL Database mediante T-SQL en SQL Server Management Studio.
  
1. Conéctese a la base de datos `master` de la instancia.  
2. Abra una ventana de consulta.
3. Asegúrese de que nadie está usando la base de datos.
4. Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo cambia el nombre de la base de datos `MyTestDatabase` a `MyTestDatabaseCopy`.
  
   ```sql
   ALTER DATABASE MyTestDatabase MODIFY NAME = MyTestDatabaseCopy ;
   ```  

## <a name="backup-after-renaming-a-database"></a>Copia de seguridad después de cambiar el nombre de una base de datos  

Después de cambiar el nombre de una base de datos en SQL Server, haga una copia de seguridad de la base de datos `master`. En Azure SQL Database, esto no es necesario porque las copias de seguridad se realizan automáticamente.  
  
## <a name="reset-your-default-database-after-rename"></a>Restablecimiento de la base de datos predeterminada después de cambiar el nombre

Si la base de datos de la que va a cambiar el nombre se ha establecido como la predeterminada, use el comando siguiente para restablecer la predeterminada en la base de datos cuyo nombre se ha cambiado:


```sql
USE [master]
GO
ALTER LOGIN [your-login] WITH DEFAULT_DATABASE=[new-database-name]
GO
```


## <a name="see-also"></a>Consulte también

- [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)
- [Identificadores de base de datos](../../relational-databases/databases/database-identifiers.md)  
