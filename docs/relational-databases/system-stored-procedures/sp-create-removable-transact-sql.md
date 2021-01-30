---
description: sp_create_removable (Transact-SQL)
title: sp_create_removable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_removable
- sp_create_removable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_removable
ms.assetid: 06e36ae5-f70d-4a26-9a7f-ee4b9360b355
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7ffe2273f7bd2ca63143f77a4d0c5726709d8440
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205202"
---
# <a name="sp_create_removable-transact-sql"></a>sp_create_removable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una base de datos de medios extraíbles. Genera tres archivos o más (uno para las tablas de catálogo del sistema, otro para el registro de transacciones y uno o más archivos para las tablas de datos) y coloca la base de datos en esos archivos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Se recomienda usar [Create Database](../../t-sql/statements/create-database-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_create_removable   
   [ @dbname = ] 'dbname',   
   [ @syslogical= ] 'syslogical',   
   [ @sysphysical = ] 'sysphysical',   
   [ @syssize = ] syssize,   
   [ @loglogical = ] 'loglogical',   
   [ @logphysical = ] 'logphysical',   
   [ @logsize = ] logsize,   
   [ @datalogical1 = ] 'datalogical1',   
   [ @dataphysical1 = ] 'dataphysical1',   
   [ @datasize1 = ] datasize1 ,   
   [ @datalogical16 = ] 'datalogical16',   
   [ @dataphysical16 = ] 'dataphysical16',   
   [ @datasize16 = ] datasize16 ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dbname = ] 'dbname'` Es el nombre de la base de datos que se va a crear para su uso en medios extraíbles. *dbname* es de **tipo sysname**.  
  
`[ @syslogical = ] 'syslogical'` Es el nombre lógico del archivo que contiene las tablas del catálogo del sistema. *syslogic* es **sysname**.  
  
`[ @sysphysical = ] 'sysphysical'` Es el nombre físico. Se incluye la ruta de acceso completa al archivo que contiene las tablas del catálogo del sistema. *sysphysical* es **nvarchar (260)**.  
  
`[ @syssize = ] syssize` Es el tamaño, en megabytes, del archivo que contiene las tablas del catálogo del sistema. *syssize* es de **tipo int**. El valor mínimo de *syssize* es 1.  
  
`[ @loglogical = ] 'loglogical'` Es el nombre lógico del archivo que contiene el registro de transacciones. *loglogical* es de **tipo sysname**.  
  
`[ @logphysical = ] 'logphysical'` Es el nombre físico. Se incluye la ruta de acceso completa al archivo que contiene el registro de transacciones. *logphysical* es **nvarchar (260)**.  
  
`[ @logsize = ] logsize` Es el tamaño, en megabytes, del archivo que contiene el registro de transacciones. *es de* **tipo int**. El mínimo de *registro* es 1.  
  
`[ @datalogical1 = ] 'datalogical'` Es el nombre lógico de un archivo que contiene las tablas de datos. *Valor_lógico* es **sysname**.  
  
 Debe ser un número de archivos de datos comprendido entre 1 y 16. Normalmente, se crea más de un archivo de datos cuando se espera que la base de datos tenga un gran tamaño y deba distribuirse en varios discos.  
  
`[ @dataphysical1 = ] 'dataphysical'` Es el nombre físico. Se incluye la ruta de acceso completa a un archivo que contiene tablas de datos. *nonfísica* es **nvarchar (260)**.  
  
`[ @datasize1 = ] 'datasize'` Es el tamaño, en megabytes, de un archivo que contiene tablas de datos. *datasize* es de **tipo int**. El tamaño mínimo de los valores de *datasize* es 1.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Utilice este procedimiento almacenado si desea realizar una copia de la base de datos en un medio extraíble (como un disco compacto) y distribuir la base de datos a otros usuarios.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CREATE DATABASE, CREATE ANY DATABASE o ALTER ANY DATABASE.  
  
 Para mantener el control del uso del disco en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el permiso para crear bases de datos suele limitarse a un número reducido de cuentas de inicio de sesión.  
  
### <a name="permissions-on-data-and-log-files"></a>Permisos en archivos de datos y de registro  
 Siempre que se realizan ciertas operaciones en una base de datos, los permisos correspondientes se establecen en sus datos y archivos de registro. Los permisos evitan que los archivos se modifiquen accidentalmente si residen en un directorio sin restricción de permisos.  
  
|Operación en base de datos|Permisos establecidos en archivos|  
|---------------------------|------------------------------|  
|Modificada para agregar un nuevo archivo|Creado|  
|Realización de copia de seguridad|Adjuntada|  
|Restaurada|Separada|  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no establece permisos en archivos de datos y de registro.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea la base de datos `inventory` como una base de datos extraíble.  
  
```  
EXEC sp_create_removable 'inventory',   
   'invsys',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invsys.mdf'  
, 2,   
   'invlog',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invlog.ldf', 4,  
   'invdata',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invdata.ndf',   
10;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Adjuntar y separar bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_certify_removable &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-certify-removable-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_dbremove &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbremove-transact-sql.md)   
 [sp_detach_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_helpfilegroup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
