---
description: sp_detach_db (Transact-SQL)
title: sp_detach_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_detach_db
- sp_detach_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_detach_db
- detaching databases [SQL Server]
ms.assetid: abcb1407-ff78-4c76-b02e-509c86574462
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1d4b484cd6d440b14b7639b513fba7e53f615443
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200754"
---
# <a name="sp_detach_db-transact-sql"></a>sp_detach_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Separa una base de datos que actualmente no se está utilizando de una instancia del servidor y, opcionalmente, ejecuta UPDATE STATISTICS en todas las tablas antes de la separación.  
  
> [!IMPORTANT]  
>  Para separar una base de datos replicada, se debe anular su publicación. Para obtener más información, vea la sección “Comentarios” más adelante en este tema.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_detach_db [ @dbname= ] 'database_name'   
    [ , [ @skipchecks= ] 'skipchecks' ]   
    [ , [ @keepfulltextindexfile = ] 'KeepFulltextIndexFile' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dbname = ] 'database_name'` Es el nombre de la base de datos que se va a desasociar. *database_name* es un valor **sysname y su** valor predeterminado es NULL.  
  
`[ @skipchecks = ] 'skipchecks'` Especifica si se debe omitir o ejecutar UPDATE STATISTIC. *skipchecks* es un valor de tipo **nvarchar (10)** y su valor predeterminado es NULL. Para omitir UPDATE STATISTICs, especifique **true**. Para ejecutar explícitamente UPDATE STATISTICs, especifique **false**.  
  
 De forma predeterminada, UPDATE STATISTICS se ejecuta para actualizar información acerca de los datos de las tablas e índices. Ejecutar UPATE STATISTICS es útil para las bases de datos que se trasladan a medios de solo lectura.  
  
`[ @keepfulltextindexfile = ] 'KeepFulltextIndexFile'` Especifica que el archivo de índice de texto completo asociado a la base de datos que se está desasociando no se quitará durante la operación de separación de la base de datos. *KeepFulltextIndexFile* es un valor de tipo **nvarchar (10)** y su valor predeterminado es **true**. Si *KeepFulltextIndexFile* es **false**, se quitan todos los archivos de índice de texto completo asociados a la base de datos y los metadatos del índice de texto completo, a menos que la base de datos sea de solo lectura. Si es NULL o **true**, se conservan los metadatos relacionados con el texto completo.  
  
> [!IMPORTANT]
>  El parámetro **\@ keepfulltextindexfile** se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . No use este parámetro en nuevos trabajos de desarrollo, y modifique lo antes posible las aplicaciones que lo usen actualmente.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Cuando se desasocia una base de datos, todos sus metadatos se eliminan. Si la base de datos era la base de datos predeterminada de cualquier cuenta de inicio de sesión, **Master** se convierte en su base de datos predeterminada.  
  
> [!NOTE]  
>  Para obtener información sobre cómo ver la base de datos predeterminada de todas las cuentas de inicio de sesión, vea [sp_helplogins &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md). Si tiene los permisos necesarios, puede usar [ALTER login](../../t-sql/statements/alter-login-transact-sql.md) para asignar una nueva base de datos predeterminada a un inicio de sesión.  
  
## <a name="restrictions"></a>Restricciones  
 No se puede separar una base de datos si se da alguna de estas circunstancias:  
  
-   Se está utilizando la base de datos actualmente. Para obtener más información, vea "Obtener acceso exclusivo" más adelante en este tema.  
  
-   Si está replicada, la base de datos está publicada.  
  
     Antes de poder separar la base de datos, debe deshabilitar la publicación ejecutando [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
    > [!NOTE]  
    >  Si no puede usar **sp_replicationdboption**, puede quitar la replicación ejecutando [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
-   Existe una instantánea de base de datos en la base de datos.  
  
     Para poder separar la base de datos, debe quitar todas sus instantáneas. Para obtener más información, vea [Eliminar una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
    > [!NOTE]  
    >  No puede separar ni adjuntar una instantánea de base de datos.  
  
-   Se está creando un reflejo de la base de datos.  
  
     La base de datos no se puede separar hasta que finalice la sesión de creación de reflejo de la base de datos. Para obtener más información, vea [Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   La base de datos es sospechosa.  
  
     Las bases de datos sospechosas se deben poner en el modo de emergencia para poder desasociar la base de datos. Para obtener más información sobre cómo poner una base de datos en modo de emergencia, vea [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-   La base de datos es una base de datos del sistema.  
  
## <a name="obtaining-exclusive-access"></a>Obtener acceso exclusivo  
 Separar una base de datos requiere acceso exclusivo a la misma. Si la base de datos que desea separar está en uso, para poder separarla debe establecer la base de datos en modo SINGLE_USER para obtener acceso exclusivo.

 Antes de establecer la base de datos como SINGLE_USER, compruebe que la opción AUTO_UPDATE_STATISTICS_ASYNC está establecida en OFF. Cuando esta opción se establece en ON, el subproceso en segundo plano que se usa para actualizar las estadísticas realiza una conexión con la base de datos y no se podrá tener acceso a la base de datos en modo de usuario único. Para obtener más información, vea [establecer una base de datos en modo de usuario único](../databases/set-a-database-to-single-user-mode.md).

 Por ejemplo, la siguiente `ALTER DATABASE` instrucción obtiene acceso exclusivo a la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de datos después de que todos los usuarios actuales se desconecten de la base de datos.  
  
```  
USE master;  
ALTER DATABASE AdventureWorks2012  
SET SINGLE_USER;  
GO  
```  
  
> [!NOTE]  
>  Para forzar a los usuarios actuales a salir de la base de datos inmediatamente o dentro de un número especificado de segundos, use también la opción ROLLBACK: ALTER DATABASE *database_name* Set SINGLE_USER WITH rollback *rollback_option*. Para obtener más información, vea [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="reattaching-a-database"></a>Volver a adjuntar una base de datos  
 Los archivos separados permanecen y se pueden volver a adjuntar utilizando CREATE DATABASE (con la opción FOR ATTACH o FOR ATTACH_REBUILD_LOG). Los archivos se pueden mover a otro servidor y adjuntarse allí.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** o la pertenencia al rol **db_owner** de la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se separa la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de datos con *skipchecks* establecida en true.  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
 En el ejemplo siguiente se separa la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] y se conservan los archivos y los metadatos del índice de texto completo. Este comando ejecuta UPDATE STATISTICS, que es el comportamiento predeterminado.  
  
```  
exec sp_detach_db @dbname='AdventureWorks2012'  
    , @keepfulltextindexfile='true';  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Adjuntar y separar bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [Separar una base de datos](../../relational-databases/databases/detach-a-database.md)  
  
