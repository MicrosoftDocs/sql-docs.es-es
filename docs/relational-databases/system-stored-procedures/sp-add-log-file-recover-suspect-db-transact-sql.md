---
description: sp_add_log_file_recover_suspect_db (Transact-SQL)
title: sp_add_log_file_recover_suspect_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_file_recover_suspect_db_TSQL
- sp_add_log_file_recover_suspect_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_file_recover_suspect_db
ms.assetid: b41ca3a5-7222-4c22-a012-e66a577a82f6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ecde40e66d5754638872a12c79ca580b0b5f2982
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206736"
---
# <a name="sp_add_log_file_recover_suspect_db-transact-sql"></a>sp_add_log_file_recover_suspect_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega un archivo de registro a un grupo de archivos cuando la recuperación no puede completarse en una base de datos debido a un error de espacio de registro insuficiente (9002). Una vez agregado el archivo, **sp_add_log_file_recover_suspect_db** desactiva el valor sospechoso y completa la recuperación de la base de datos. Los parámetros son los mismos que los de ALTER DATABASE *database_name* Agregar archivo de registro.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_log_file_recover_suspect_db [ @dbName= ] 'database' ,   
    [ @name = ] 'logical_file_name' ,   
    [ @filename= ] 'os_file_name' ,   
    [ @size = ] 'size' ,   
    [ @maxsize = ] 'max_size' ,   
    [ @filegrowth = ] 'growth_increment'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dbName = ] 'database'` Es el nombre de la base de datos. *Database* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @name = ] 'logical_file_name'` Es el nombre que se usa en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se hace referencia al archivo. El nombre debe ser único en el servidor. *logical_file_name* es de tipo **nvarchar (260)** y no tiene ningún valor predeterminado.  
  
`[ @filename = ] 'os_file_name'` Es la ruta de acceso y el nombre de archivo que usa el sistema operativo para el archivo. El archivo debe residir en el servidor donde esté instalado el [!INCLUDE[ssDE](../../includes/ssde-md.md)]. *os_file_name* es de tipo **nvarchar (260)** y no tiene ningún valor predeterminado.  
  
`[ @size = ] 'size_ '` Es el tamaño inicial del archivo. *size* es de tipo **nvarchar (20)** y su valor predeterminado es NULL. Especifique un número entero; no incluya decimales. Se pueden utilizar los sufijos MB y KB para especificar megabytes o kilobytes, respectivamente. El valor predeterminado es MB. El valor mínimo es 512 KB. Si no se especifica *size* , el valor predeterminado es 1 MB.  
  
`[ @maxsize = ] 'max_size_ '` Es el tamaño máximo que puede alcanzar el archivo. *max_size* es de tipo **nvarchar (20)** y su valor predeterminado es NULL. Especifique un número entero; no incluya decimales. Se pueden utilizar los sufijos MB y KB para especificar megabytes o kilobytes, respectivamente. El valor predeterminado es MB.  
  
 Si no se especifica *max_size* , el archivo aumentará hasta que el disco esté lleno. El registro de aplicación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows avisa a un administrador cuando un disco está a punto de llenarse.  
  
`[ @filegrowth = ] 'growth_increment_ '` Es la cantidad de espacio que se agrega al archivo cada vez que se necesita espacio nuevo. *growth_increment* es de tipo **nvarchar (20)** y su valor predeterminado es NULL. Un valor 0 indica que no hay crecimiento. Especifique un número entero; no incluya decimales. El valor se puede especificar en MB, KB o como un porcentaje (%). Cuando se especifica%, el incremento de crecimiento es el porcentaje especificado del tamaño del archivo en el momento en que se produce el incremento. Si se especifica un número sin los sufijos MB, KB o %, el valor predeterminado es MB.  
  
 Si *growth_increment* es null, el valor predeterminado es 10% y el valor de tamaño mínimo es 64 KB. El tamaño especificado se redondea al múltiplo de 64 KB más cercano.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los permisos de ejecución son miembros del rol fijo de servidor **sysadmin** . Estos permisos no se pueden transferir.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente, la base de datos `db1` se ha marcado como sospechosa durante la recuperación debido a un error de espacio de registro insuficiente (error 9002).  
  
```  
USE master;  
GO  
EXEC sp_add_log_file_recover_suspect_db db1, logfile2,  
'C:\Program Files\Microsoft SQL  
    Server\MSSQL13.MSSQLSERVER\MSSQL\Data\db1_logfile2.ldf',   
    '1MB';  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_add_data_file_recover_suspect_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-data-file-recover-suspect-db-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
